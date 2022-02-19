---
title: Linux常用指令
date: 2020-04-28
categories: Command
tags: Linux
---

&nbsp;

<!--more-->

## 解压缩
.tar
```$ tar -xvf <file_name>.tar```
```$ tar -cvf <file_name>.tar <directory_name>```

.gz
tar -xzf <file_name>.gz
tar -czf <file_name>.gz <directory_name>

.tar.gz
gzip / gunzip

.rar
unrar

.zip
unzip

## 修改文件操作权限
$ chmod

数字设定：4r 2w 1x, e.g. $ chmod 777 a.exe
符号设定：u拥有者 g所属群组 o其他人 a所有人 +添加 -移除 =设定 r读 w写 x执行，e.g. $chmod a+x, g+w a.exe

## 查看文件
cat 跳到最后一页
more 回车可翻页
less 光标上下浏览

## 输出当前路径
$ pwd (print working directory)