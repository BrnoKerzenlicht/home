---
title: SpringCloudDemo
date: 2019-01-23
---

Spring Cloud Demo项目，包含Eureka、Ribbon、Feign、Hystrix的简单使用

<!-- more -->

## 模块结构

#### server1
Eureka服务注册中心

#### provider1

Eureka服务提供者1，使用fastjson返回结果

#### provider2
Eureka服务提供者2，使用默认HttpMessageConverter返回结果

#### provider3
Eureka服务提供者3，使用默认HttpMessageConverter返回结果，controller层通过参数列表直接接收参数

#### model
包含自定义类的模块

#### consumer1
服务消费者1，使用ribbon+restTemplate作的服务消费者，包含Hystrix，使用默认ribbon配置，并且配置了Hystrix Dashboard

#### consumer2
服务消费者2，使用Feign作为服务消费者，配置使用了Hystrix、HTTP client、Ribbon

#### consumer3
服务消费者3，使用Feign作为服务消费者，配置使用了Hystrix、Ribbon

#### turbineDemo
turbine Demo，整合consumer1、consumer2、consumer3的hystrix信息，需要consumer1、consumer2、consumer3都注册到注册中心

#### zuulDemo
zuul Demo,可以实现动态路由、监控、安全、认证鉴权、压力测试、金丝雀测试[^1]、审查、服务迁移、负载剪裁、静态应答处理等；实现路由时，serviceId必须是注册到注册中心的serviceId

#### test
测试模块，可以模拟发送http get请求，用于验证ribbon配置策略

### provider、consumer信息

|module|是否在server上注册|controller method|service method|service name|hystrix配置|
|:-------:|:-:|:-:|:-:|:-:|---|
|provider1|是 |String getString(HttpServletRequest)<br/>User getUser(HttpServletRequest)<br/>String getUserString(HttpServletRequest)||service-demo|getUser间隔sleep1000|
|provider2|是 |String getString(HttpServletRequest)<br/>User getUser(HttpServletRequest)<br/>String getUserString(HttpServletRequest)||service-demo||
|provider3|是 |String getString(String)||service-demo1||
|consumer1|是 |String string(HttpServletRequest)<br/>User user(HttpServletRequest)<br/>String userString(HttpServletRequest)<br/>String getString(HttpServletRequest)<br/>String discovery()|String DemoService.string(String)<br/>User DemoService.user(String)<br/>String DemoService.userString(String)<br/>String DemoService.getString(String)|service-demo<br/>service-demo<br/>service-demo<br/>service-demo1|线程池coreSize500超时设置30000|
|consumer2|是 |String string(HttpServletRequest)<br/>User user(HttpServletRequest)<br/>String string1(HttpServletRequest)|String IDemoService.getString(String)<br/>User IDemoService.getUser(String)<br/>String Service0Client.test(String)|s vice-demo<br/>service-demo<br/>service-demo1|超时设置100|
|consumer3|是 |String string(HttpServletRequest)<br/>String string1(HttpServletRequest)<br/>String consumer3.str(HttpServletRequest)|String IDemoService.getString(String)<br/>String IDemo1Service.getString(String)|service-demo<br/>service-demo1|超时设置100|

源码地址：https://gitee.com/brnokerzenlicht/springclouddemo

[^1]:“金丝雀部署”是增量发布的一种类型，它的执行方式是在原有软件生产版本可用的情况下，同时部署一个新的版本。同时运行同一个软件产品的多个版本需要软件针对配置和完美自动化部署进行特别设计。[在生产中使用金丝雀部署来进行测试](http://www.infoq.com/cn/news/2013/03/canary-release-improve-quality%20%27%E5%9C%A8%E7%94%9F%E4%BA%A7%E4%B8%AD%E4%BD%BF%E7%94%A8%E9%87%91%E4%B8%9D%E9%9B%80%E9%83%A8%E7%BD%B2%E6%9D%A5%E8%BF%9B%E8%A1%8C%E6%B5%8B%E8%AF%95%27)

