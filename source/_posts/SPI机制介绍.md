---
title: Java SPI机制介绍
date: 2019-02-15
---

Java SPI 全称为 (Service Provider Interface) ，是JDK内置的一种服务提供发现机制
<!-- more -->

### 概念

SPI 全称为 (Service Provider Interface) ，是**JDK内置的一种服务提供发现机制**。

Wikipedia对其的解释

> Service Provider Interface (SPI) is an API intended to be implemented or extended by a third party. It can be used to enable framework extension and replaceable components.
>
> SPI是一个API，它是由第三方实现或扩展的。它可以用于支持框架扩展和可替换组件。

Java SPI 实际上是“**基于接口的编程＋策略模式＋配置文件**”组合实现的动态加载机制。

服务提供者接口(SPI)是服务定义的公共接口和抽象类的集合。SPI 定义了应用程序可用的类和方法。 服务提供者实现 SPI。具有可扩展服务的应用程序将允许供应商甚至客户在不修改原始应用程序的情况下添加服务提供者。

![Java SPI机制](http://ww1.sinaimg.cn/large/6dbd1580ly1g079546z1wj20m107e410.jpg)



### 应用实例

当服务的提供者提供了一种接口的实现之后，需要在 classpath 下的**META-INF/services/目录**里创建一个**以服务接口命名的文件，这个文件里的内容就是这个接口的具体的实现类**。当其他的程序需要这个服务的时候，就可以通过查找这个jar包（一般都是以jar包做依赖）的 META-INF/services/ 中的配置文件，配置文件中有接口的具体实现类名，可以根据这个类名进行加载实例化，就可以使用该服务了。JDK中查找服务实现的工具类是：**java.util.ServiceLoader**。

#### SPI 接口

定义了一个对象序列化接口，内有三个方法：序列化方法、反序列化方法和序列化名称。

```java
public interface ObjectSerializer {
    byte[] serialize(Object obj) throws Exception;
    String getSchemeName();
}
```

#### SPI 具体实现

1. Kryo 实现

```java
public class KryoSerializer implements ObjectSerializer {
    @Override
    public byte[] serialize(Object obj) throws Exception {
        byte[] bytes;
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        try {
            //获取kryo对象
            Kryo kryo = new Kryo();
            Output output = new Output(outputStream);
            kryo.writeObject(output, obj);
            bytes = output.toBytes();
            output.flush();
        }
        catch (Exception ex) {
            throw new Exception("kryo serialize error" + ex.getMessage());
        }
        finally {
            try {
                outputStream.flush();
                outputStream.close();
            }
            catch (IOException e) {
            }
        }
        return bytes;
    }
	@Override
    public String getSchemeName() {
        return "kryoSerializer";
    }
}
```

2. Java 原生实现
```java
public class JavaSerializer implements ObjectSerializer {
    @Override
    public byte[] serialize(Object obj) throws Exception {
        ByteArrayOutputStream arrayOutputStream;
        try {
            arrayOutputStream = new ByteArrayOutputStream();
            ObjectOutput objectOutput = new ObjectOutputStream(arrayOutputStream);
            objectOutput.writeObject(obj);
            objectOutput.flush();
            objectOutput.close();
        }
        catch (IOException e) {
            throw new Exception("JAVA serialize error " + e.getMessage());
        }
        return arrayOutputStream.toByteArray();
    }
	@Override
    public String getSchemeName() {
        return "javaSerializer";
    }
}
```

#### 增加配置文件

Resource 下面创建 META-INF/services  目录里创建一个以服务接口命名的文件,内容如下

```
KryoSerializer
JavaSerializer
```

#### 测试类

```java
public class Test {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("1", "2", "3");
        ServiceLoader<ObjectSerializer> serviceLoader = ServiceLoader.load(ObjectSerializer.class);
        Iterator<ObjectSerializer> iterator = serviceLoader.iterator();
        iterator.forEachRemaining(serializer -> {
            try {
                System.out.println(serializer.getSchemeName() + ":" + Arrays.toString(serializer.serialize(list)));
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        });
    }
}
```


### SPI 思想的应用

#### 数据库驱动加载

在访问数据库时，需要根据数据库类型加载不同的数据库驱动，之前往往都需要通过`Class.forName`显示加载，从 JDBC 4.0 开始，`DriverManager`中使用了 SPI 机制，在类加载时会通过 SPI 加载所有的`java.sql.Driver`接口的实现类，在程序中直接调用 `DriverManager.getConnection()`方法就可以获取数据库连接。主要代码在`loadInitialDrivers`方法中

```java
private static void loadInitialDrivers() {
    ...
    AccessController.doPrivileged(new PrivilegedAction<Void>() {
        public Void run() {
            ServiceLoader<Driver> loadedDrivers = ServiceLoader.load(Driver.class);
            //获取迭代器
            Iterator<Driver> driversIterator = loadedDrivers.iterator();
            try{
                //遍历
                while(driversIterator.hasNext()) {
                    driversIterator.next();
                    //可以做具体的业务逻辑
                }
            } catch(Throwable t) {
            // Do nothing
            }
            return null;
        }
    });
    ...
}
```

注：OJDBC 驱动没有提供 SPI 实现机制

#### 日志门面接口实现类加载

SLF4J 通过 SPI 机制加载不同提供商的日志实现类，主要代码如下

```java
//org.slf4j.LoggerFactory 1.8.0-beta0
private static List<SLF4JServiceProvider> findServiceProviders() {
    ServiceLoader<SLF4JServiceProvider> serviceLoader = ServiceLoader.load(SLF4JServiceProvider.class);
    List<SLF4JServiceProvider> providerList = new ArrayList();
    Iterator i$ = serviceLoader.iterator();

    while(i$.hasNext()) {
        SLF4JServiceProvider provider = (SLF4JServiceProvider)i$.next();
        providerList.add(provider);
    }

    return providerList;
}
```

logback 1.3.0-alpha0中有对应的配置文件

![image](http://ww1.sinaimg.cn/large/6dbd1580ly1fzhs7kg2e3j20qx0b0q3e.jpg)

注：slf4j 1.7.2 没有使用此机制，1.8.0-beta0 有使用

#### Eclipse 插件

Eclipse 使用 OSGi 作为插件系统的基础，动态添加新插件和停止现有插件，以动态的方式管理组件生命周期。

一般来说，插件的文件结构必须在指定目录下包含以下三个文件：

- META-INF/MANIFEST.MF: 项目基本配置信息，版本、名称、启动器等

- build.properties: 项目的编译配置信息，包括，源代码路径、输出路径

- plugin.xml：插件的操作配置信息，包含弹出菜单及点击菜单后对应的操作执行类等 

当 eclipse 启动时，会遍历 plugins 文件夹中的目录，扫描每个插件的清单文件 MANIFEST.MF，并建立一个内部模型来记录它所找到的每个插件的信息，就实现了动态添加新的插件。

#### Spring SPI 机制

通过 SpringFactoriesLoader 代替 JDK 中 ServiceLoader，通过 META-INF/spring.factories 文件代替 META-INF/service 目录下的描述文件，具体实现步骤不同，但原理类似。

#### Dubbo SPI 扩展

Dubbo 的扩展机制和 Java 的 SPI 机制非常相似，Dubbo SPI 的相关逻辑被封装在了 ExtensionLoader 类中，通过 ExtensionLoader，可以加载指定的实现类。Dubbo SPI 所需的配置文件需放置在 META-INF/dubbo 路径下，通过键值对的方式进行配置，接口上需要有 @SPI 注解。

### 小结

使用 Java SPI 机制的优势是实现解耦，使得第三方服务模块的装配控制的逻辑与调用者的业务代码分离，而不是耦合在一起。应用程序可以根据实际业务情况启用框架扩展或替换框架组件。虽然 ServiceLoader 也算是使用的延迟加载，但是基本只能通过遍历全部获取，也就是接口的实现类全部加载并实例化一遍。如果你并不想用某些实现类，它也被加载并实例化了，这就造成了浪费。此外获取某个实现类的方式也不够灵活。