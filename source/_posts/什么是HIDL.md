---
title: 什么是HIDL
date: 2018-09-23 10:16:41
tags: Hidl
categories: Android
---

### HIDL的定义：
HIDL的全称为 HAL Interface Description Language,用于定义HAL与其用户之间接口的描述语言。其可用于在独立编译的代码库之间进行通信的系统。
HIDL用于进程间通信，进程间的通信可以称为Binder化(Binderized).

###HIDL的设计目的：
主要是将framework和hal隔离开，framework可以单独覆盖、更新而不用对hal做改动。引入hidl，可以将hal从system.img中移除出去，方便android版本的升级。

###设计的几个要点：
- HIDL接口是版本化的，在发布后不能更改。
- HIDL尝试将拷贝操作最小化，HIDL定义的数据会被交付到c++标准布局（c++ Standard Layout）数据结构中的c++代码，这些数据可以在不打包的情况下使用。HIDL还是用了共享内存（shared memory）的接口。
  - 共享内存
  - 快速消息队列（Fast Message Queue，FMQ）

[Android官方文档对Hidl的介绍](https://source.android.google.cn/devices/architecture/hidl/
)