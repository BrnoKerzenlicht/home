---
title: 使用hexo搭建个人博客操作记录
date: 2019-01-20
---



利用 github Pages + Hexo 搭建个人博客记录，内容大都来源网上教程，具体操作的时候有些和教程的不太一样，所以记录一下，以便换电脑时能记得怎么操作。

<!-- more -->



## 环境要求

本地安装 Git、Node.js

## Hexo

### 初始化个人博客

使用 Hexo 来搭建个人博客，具体操作如下：

1. 通过命令行操作安装 Hexo ，命令： `npm install hexo -g` 

2. 创建一个文件夹作为博客的目录（假定为 F://Hexo ），通过命令`hexo init`进行初始化

3. 通过命令`hexo server`进行预览，命令执行后会有提示

   ```
   PS F:\hexo> hexo server
   INFO  Start processing
   INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
   ```

   这时通过浏览器访问 localhost:4000 ，就可以看到使用 Hexo 搭建的博客了

### 修改博客配置

初始化的博客许多内容都是默认的，这个时候通过修改配置文件的方式进行调整，配置文件路径：`F://Hexo/_config.yml`

主要修改了以下内容

- Site下的 title 、subtitle 、author，其余保持默认值

```
# Site
title: Brno Kerzenlicht's Home
subtitle:
description:
keywords:
author: Brno Kerzenlicht
language:
timezone:
```
- Deploy 下的相关内容

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:xxx/xxx.github.io.git
  branch: master
```

### 修改主题

觉得默认的主题不好看时，可以去 [hexo 官网](https://hexo.io/themes/)挑选一个自己喜欢的主题进行替换，修改方式也比较简单

- 找到主题的 github 地址，以 still 为例， github 地址是 https://github.com/JeremyFan/hexo-theme-still
- 将主题仓库中的内容都下载下来，什么方式都可以
- 将下载好的主题文件放到主题目录（`F://Hexo/themes/still`）下，`still`是主题名称，里面就是主题仓库里的所有文件
- 修改博客配置文件（就是上面的那个_config.yml）

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: still
```

- 修改主题的配置文件，主题里面也有一些内容是需要调整的，最简单的方式也是修改配置文件，配置文件路径 `F://Hexo/themes/still/_config.yml`

- 如果发现有些内容还需要调整的话，就只能去主题的具体文件里去修改了
- 使用主题的时候最好看一下主题仓库中的 README，最开始使用的时候发现不好用，后面才发现是因为没有安装 `pug renderer` (安装命令 `npm install hexo-render-pug --save`)

## 同步到GitHub

通过上面的步骤就可以在本地看到一个自己还算满意的博客了，剩下的内容就是同步到 github 了。具体操作如下：

- 创建一个仓库，要求仓库名称为 xxx.github.io ，如果仓库名称不符合这个约定，也可以搭建，但是访问时的路径会是 https://xxx.github.io/仓库名，而不是 https://xxx.github.io ；而且使用 hexo 生成的静态文件在获取资源时会找不到相关 css 等文件，所以仓库名还是按约定设置会比较好

- 在博客目录（`F://Hexo`）下创建一个本地 git 仓库，并关联到 github 远程仓库

- 通过 Hexo 生成静态文件 `hexo generate`

- 通过 Hexo 进行部署，命令 `hexo deploy`

- 在 github 仓库页面中，选择 `Settings`，找到 `GitHub Pages`部分，点击`Source`下拉框，选择`master branch` ，保存之后就会看到页面提示，可以通过浏览器访问 https://xxx.github.io/ 查看博客内容了

## 写一篇博客试试

已经搭建完博客了，这个时候可以试着写一篇博客了

- 通过 Hexo 创建一篇新的博客，命令：`hexo new [title]` ，title 就是博客的标题
- 在博客目录中找到生成的文件 `F:\hexo\source\_posts\[title].md`，直接编辑这个文件就可以了，发布之前还是可以通过 Hexo 先预览看下的，执行完 `hexo server`之后，可以实时看到新写的文章，调整文章内容保持后，刷新页面就可以了

## 附录&小工具

1. Hexo 官网	

   https://hexo.io

2. GitHub Pages

   https://pages.github.com/

3. Typora：一个觉得很好用的本地 markdown 编辑器

   https://typora.io/

4. 在线图床：

   https://sm.ms/

5. 微博图床：chrome 插件，需要登录微博之后才能使用

   https://chrome.google.com/webstore/detail/fdfdnfpdplfbbnemmmoklbfjbhecpnhf