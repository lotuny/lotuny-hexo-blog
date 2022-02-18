---
title: Hexo+GitHub个人博客部署
date: 2018-11-07
categories: _Other
tags: Blog
---

Hexo+GitHub个人博客部署

<!--more-->

## 前置环境及条件
- git
- github账号
- Node.js

## GitHub代码库部署
在github创建新仓库，取名为xxx.github.io，其中xxx是自己决定的名字，之后xxx.github.io将成为访问个人博客的域名。

## Hexo安装和初始化
hexo是基于markdown的个人博客框架，使用者编写markdown文章，hexo将自动生成html静态网页文件及相关的css和js等文件。

1. 在命令提示符输入```$ npm install hexo-cli -g```并回车（注意：Node.js配置失败将不能使用```$ npm```命令），接着等待hexo下载完成
2. 进入想要放置博客项目的位置下，输入```$ hexo init xxx```并回车，其中xxx为博客项目名称,也可以创建好项目空文件夹，进入后直接输入```$ hexo init```。
3. 在博客项目的位置下打开命令提示符，输入```$ hexo server```运行服务器。在浏览器输入默认地址localhost:4000，如果浏览器显示成功则hexo初始化成功。如果显示localhost发送的响应无效，请检查协议是否为https，若是请改为http。

## Hexo创建博客
如初始化过程中所说创建好博客项目，则会看到自动创建好的目录结构，其中source和themes是使用者最常用的两个文件夹。
- source: 存放markdown文件形式的文章，以及图片和视频等资源
- themes: 存放博客主题，默认为landscape

1. 寻找并设置自己喜欢的主题

    1. 寻找主题
    在[https://hexo.io/themes/index.html](https://hexo.io/themes/index.html)可以查找，但是这些主题貌似都不是官方设计的，相当于主题商城，只不过都是免费的。所以该网页不直接提供下载链接，只提供演示链接；有两种方法找到下载链接，一种在演示博客中找到github项目的链接；另一种直接在github搜索主题名字。

    2. 下载配置主题
    将GitHub上对应主题项目克隆到本地项目下themes文件夹中，在_config.yml配置文件中修改主题项目的名称，比如这里使用的是[tufu9441/maupassant-hexo](https://github.com/tufu9441/maupassant-hexo)，修改博客项目根目录下_config.yml：
    ```
    # Extensions
    ## Plugins: https://hexo.io/plugins/
    ## Themes: https://hexo.io/themes/
    theme: xxx
    ```
    接着就可以通过修改主题文件夹下_config.yml文件（注意不是根目录下的那个）进行个性化设置了，具体操作因不同主题而异，设置规范以主题项目说明为准。

2. 修改_config.yml基本配置

    - 网页基本信息
    ```
    # Site
    title: xxx
    subtitle:
    description:
    keywords:
    author: xxx
    language: zh-CN
    timezone: Asia/Shanghai
    ```

    - 网页地址
    ```
    # URL
    url: http://xxx.github.io
    root: /
    permalink: :year/:month/:day/:title/
    permalink_defaults:
    ```

    - GitHub连接
    ```
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
    type: git
    repo: https://github.com/xxx/xxx.github.io.git
    branch: master
    ```
    其中repo为GitHub博客项目的位置，比如我的项目位置https://github.com/lotuny/lotuny.github.io.git

3. 生成博客并上传到GitHub

    使用Hexo自动生成静态网页，这里提供了常用命令以便快速查阅。详细文档可查询https://hexo.io/docs/

    - ```$ hexo server```或```$ hexo s```在本地运行服务器预览博客效果
    - ```$ hexo generate```或```$ hexo g```生成静态网页并建好网站项目
    - ```$ hexo deploy```或```$ hexo d```将博客网站项目推到GitHub

    **Tip(s)**: generate一定要在deploy之前完成！generate出来的网页文件存放在/public文件夹中，然后deploy才能将public下的所有文件push到GitHub。
    
## 避坑指南
本地Hexo博客项目和GitHub博客项目是不一样的，可以理解为本地Hexo项目包含了GitHub博客项目。这就意味着，如果你想多地保存你的Hexo项目及其markdown文件，请务必将其完整地储存在云端，而依赖deploy自动push得到的代码库（xxx.github.io）是不能反向恢复成Hexo项目的。