---
title: Selenium自动化测试
date: 2020-04-24
categories: Test
tags: Automation Test
---

> _Primarily **Selenium** is for automating web applications for testing purposes, but is certainly not limited to just that. Boring web-based administration tasks can (and should) also be automated as well._

<!--more-->

## 介绍
Selenium支持多种语言，包括Java、JS和Python等。

## 准备工作
1. 下载安装Selenium：```$ pip install Selenium```
2. 下载chromedriver

## 打开测试页面
```
from selenium import webdriver

driver = webdriver.Chrome(executable_path='<driver_path>/chromedriver')
driver.get("<url>")
```
其中```<driver_path>```指的是存放chromedriver的位置，```<url>```是我们要进行测试的页面地址。做完了这一步，运行就可以看到chrome浏览器自动打开到指定的页面了。

## 选择及操作元素

## 其它有用操作

## 可能遇到的问题
iframe
已打开页面挂载脚本