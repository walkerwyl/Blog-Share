---
layout:     post
title:      "Android: Xposed框架的学习前导-Xposed的工作原理"
date:       2019-04-12
author:     "桂志强"
header-img: "img/post-bg-2015.jpg"
tags:
    - Android
    - Xposed
---



### Xposed介绍

在百度百科上，有：**Xposed框架是一款可以在不修改APK的情况下影响程序运行(修改系统)的框架服务，基于它可以制作出许多功能强大的模块，且在功能不冲突的情况下同时运作**

通过Xposed框架我们可以做很多有趣的事情。



### Xposed的工作原理

在了解具体原理之前，需要了解一下**Zygote**这个东西。

我们知道Android是基于Linux内核的操作系统。而zygote本身是一个应用层的进程。由linux的init 启动它。

zygote负责创建出每一个应用程序的进程。同时，Zygote创建出一个监听的进程SystemServer为之服务 。SystemServer与Zygote都是app_process在内存中映射的进程。

Zygote进程在启动时还会创建一个Dalvik虚拟机实例，每当它创建一个新的应用程序进程时，都会将这个Dalvik虚拟机实例复制到新的应用程序进程里面去，从而使得每一个应用程序进程都有一个独立的Dalvik虚拟机实例。

补充：Dalvik：Dalvik是Google公司自己设计用于Android平台的虚拟机，负责完成对象生命周期管理，堆栈管理，线程管理，安全和异常管理，垃圾回收，以及进程隔离和线程管理等等。（大概类似于JVM？）

而一个应用程序进程被Zygote进程创建出来的时候，不仅会获得Zygote进程中的Dalvik虚拟机实例拷贝，还会与Zygote一起共享Java运行库，这也就是可以将XposedBridge这个jar包加载到每一个Android应用程序中的原因。



**在[XposedBridge](<https://github.com/rovo89/XposedBridge>)中有介绍到Xposed的工作原理。**

Xposed框架的原理是通过替换/system/bin/app_process程序控制zygote进程，使得app_process在启动过程中会加载XposedBridge.jar这个jar包，从而完成对Zygote进程及其创建的Dalvik虚拟机的劫持。

而在Xposed框架中真正重要的是hook功能。在传统方法中，要想改变一个程序，那么需要先反编译，再修改，最后重新编译。而Xposed的hook功能则不需要如此。

Xposed框架中有这样一个private, native 的方法-`hookMethodNative`。这个方法也在扩展的app_process中实现。

它将把方法类型更改为“native”，并将方法实现链接到它自己的native泛型方法中。这意味着每次调用hook方法时，都会调用泛型方法，而调用方并不知道它。该方法调用XposedBridge中的方法`handleHookedMethod`，将参数传递给方法调用、this引用等。然后这个方法负责调用为这个方法调用注册的回调。

它可以做非常多的事情，包括更改调用的参数、更改实例/静态变量、调用其他方法等等。



