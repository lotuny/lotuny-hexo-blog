---
title: Git常用命令
date: 2018-11-07
categories: Command
tags: Git
---

&nbsp;

<!--more-->

以下命令中，origin常用作远程版本库名，master则为主要分支名。

## 本地创建版本库
新建一个空文件夹，在该路径下打开bash并输入命令$ git init，将生成一个.git隐藏文件。

## add&commit
```$ git add <file>```
```$ git commit -m <message>```

## 添加远程版本库
```$ git remote add orign <URL>```

## 查看关联远程版本库
```$ git remote```

## 更新远程版本库版本信息到本地
```$ git fetch```

## 同步远程版本库到本地
```$ git pull -u origin master```

## 同步本地版本库到远程
```$ git push -u origin master```

## 克隆远程版本库到本地（在此基础上push则无须添加远程版本库）
```$ git clone <URL>（等效于 init+remote add+pull）```

## 新建及更换分支（本地）
```$ git checkout -b <branch>```

## 合并分支到目前工作分支
```$ git merge <branch-to-be-merged>```

## 版本回退
```$ git reset [option] <head> ([option]=[–soft | –hard | –mixed]) ```

## 版本恢复（错误回退后恢复到未来版本）
```$ git reflog```查看最近版本信息+版本回退