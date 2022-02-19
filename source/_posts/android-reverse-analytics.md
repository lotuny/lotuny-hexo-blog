---
title: 安卓反向分析学习
date: 2020-04-24
categories: Android
tags: Security
toc: true
---

对于安卓反向分析我其实只是刚刚入门，或者说入门都不算，所以写这篇文章的目的并不是要教什么干货，而是一方面作为自己的学习笔记方便学而时习之，另一方面希望能给想入门的纯小白一点借鉴作用（在安装工具方面什么的），毕竟我也是小白，看问题的角度应该比较相似叭。

<!--more-->

## 相关概念

我要说的是那些在搞安卓反向分析之前都不太有机会去深入理解的概念，以及在这个学习课题中比较重要的概念。不全，但至少是我跟着Writeup做了两道14年阿里移动安全挑战赛的题之后认为需要的基本知识储备。

- 编译(Compile)和汇编(Assemble)

    编译指的是将高级语言程序转化成汇编语言程序，汇编则是将汇编语言程序转化成可运行的二进制文件。搞开发的小伙伴应该很熟悉自己使用的高级语言如C、Java、Python，不过运行在虚拟机上的语言并不遵循前面说的“高级语言->汇编语言->二进制文件”。

    高级语言大家都比较熟悉，那什么是汇编语言呢（这句话怎么有股老营销号的感觉）？其实汇编语言就是使用助记符（Mnemonics）来代替和表示特定指令的语言，一个助记符就对应着指令集中的一条命令，一种汇编语言就对应一套指令集，指令集和CPU架构也是一一对应的。

- 指令集

    指令集顾名思义就是指令的集合，指令就是控制CPU的单元操作，不可再分。现存指令集都分为两种：RISC (Reduced Instruction Set Computing)和CISC (Complex Instruction Set Computing)，前者是精简指令集，后者是复杂指令集，区别就是单条指令能做多少事。

    主流指令集就是：Intel x86指令集（复杂）、ARM指令集（精简）和MIPS指令集（精简）。后面提到的IDA Pro这个反编译工具则使用的是ARM指令集的汇编助记符。

- Dalvik & Dex字节码

    虽然安卓应用使用Java语言来开发，但是安卓有自己的虚拟机，还有自己的Android SDK (Software Development Kit)。Dalvik就是安卓虚拟机，在Android 5.0之后被ART (Android Runtime)取代。Dex字节码相当于Java程序的.class文件，是可以直接在安卓虚拟机上运行的文件，文件后缀为dex。

- Smali & Baksmali

    这两个分为是Dalvik的汇编器和反汇编器，三个词都是冰岛语。这里汇编的输入是.smali文件，输出是.dex文件。Android项目在生成apk安装包的过程其实并不产生smali文件。由于已经生成的.dex文件是没办法反编译回java文件的，而.dex文件又是人类不可读的，所以便产生了这么一个中间物，方便程序员调试自己的项目，当然，也方便了攻击者篡改代码。既然是人类可读的，那就有一定的语法，所以smali也算是一种语言。

    {% asset_img compilation-assembly.png %}

    编译汇编理解图图片是根据我自己的理解画的，虚线部分不是特别确定。其中左边是一般高级语言程序到可执行文件的编译汇编流程（参考C语言），右边分别是Java和Android的对比图，灰色表示过程，橙色表示虚拟机，方框表示可运行在对应虚拟机上的文件，蓝色表示工具。

- Apk安装包

    [Apk (Android application package)](https://en.wikipedia.org/wiki/Apk_(file_format))就是安卓的应用安装包，本质就是个压缩包，除了包含程序的所有运行文件以外，还有一些格式文件、签名信息等。

- so文件

- native方法

## 步骤

1. 打开安卓模拟器安装apk；
2. 打开UI Automator Viewer查看关键组件的名称；
3. 打开Jadx反编译apk找到组件创建位置并分析上下文：
    1. 如果可以通过修改语句实现目的，则打开Android Killer修改smali文件；
    2. 否则如果用了关键方法为native方法，就找到对应so文件并将其导入IDA Pro进行分析，按F5可查看伪代码，找到关键处可用插件Keypatch修改指令字节；
4. 用Android Killer重新编译项目打包成apk，并安装到模拟器上查看效果。

## 工具

- Android Studio/安卓模拟器
- UI Automator Viewer
- Jadx
- Android Killer
- IDA Pro + Keypatch