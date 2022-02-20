---
title: 计算机网络基础
date: 2020-04-28
categories: Computer Network
tags: Computer Network
---

> _A **computer network** is a set of computers sharing resources located on or provided by network nodes. The computers use common communication protocols over digital interconnections to communicate with each other._

<!--more-->

## OSI 七层网络结构 (Open Systems Interconnection Model)
7、应用层 (Application Layer)

6、表示层 (Presentation Layer)

5、会话层 (Session Layer)

4、传输层 (Transport Layer)

3、网络层 (Network Layer)

2、数据链路层 (Data Link Layer)

1、物理层 (Physical Layer)

## 常见网络协议
HTTP (Hyper Text Transfer Protocol) 超文本协议
应用层，基于TCP/IP。

POP3 (Post Office Protocol version 3) 邮局协议第三版
RFC 1939定义。应用层。

IPv4 (Internet Protocol version 4) 互联网通信协议第四版
RFC 791定义。网络层，无连接的协议。

## IP分类
IP地址=网络号+主机号，分为A、B、C三类及特殊地址D、E。

A类
32位地址中第一位必须为0，前一个字节为网络号
地址范围：1.0.0.1-126.255.255.254
默认子网掩码：255.0.0.0

B类
32位地址中前两位必须为10，前两个字节为网络号
地址范围：128.0.0.1-191.255.255.254
默认子网掩码：255.255.0.0

C类
32位地址中前三位必须为110，前三个字节为网络号
地址范围：192.0.0.1-.223.255.254
默认子网掩码：255.255.255.0

D类（多播地址）
32位地址中前四位必须为1110
地址范围：224.0.0.1-239.255.255.254

E类（保留地址）
32位地址中前4为必须为1111
地址范围：240.0.0.1-255.255.255.254

回送地址
地址范围：127.0.0.1-127.255.255.254