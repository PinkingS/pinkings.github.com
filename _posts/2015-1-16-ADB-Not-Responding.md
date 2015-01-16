---
layout: post
time: 2015-1-16
title: Android Studio ADB响应失败解决方法
category: Android Studio
keywords: Android Studio, ADB
tags: 
description: Android Studio因为ADB没有响应而无法启动的解决方案。
---

当启动Android Studio时，如果弹出

> adb not responding. you can wait more,or kill "adb.exe" process manually and click 'Restart'

说明ADB响应失败，此时点击**wait more**就会不断弹出这个对话框，点击**Restart**也无济于事。

##解决方法：

1.打开cmd，输入`adb kill-server`,`adb start-server`，`adb nodaemon server`,显示

![img](/assets/image/posts/2015-1-16-ADB/1.jpg)

说明执行`adb start-server`后启动不起来是因为adb的端口被占用了。

2.输入`netstat -aon|findstr "5037"`，可以看到进程号为10624的进程（这个进程号因机器和时间而异）在占用5037端口（adb需要使用此端口）

![img](/assets/image/posts/2015-1-16-ADB/2.jpg)

3.打开任务管理器，选择“**进程**”选项卡，点击选项栏“**查看**-**选择列...**”，勾选“**PID(进程标识符)**”，点确定。会看到每个进程都会显示它们的PID了。找到进程号为10624的进程，结束这个进程。

![img](/assets/image/posts/2015-1-16-ADB/3.jpg)

4.在cmd中，重新`adb start-server`，会看到成功启动了。

![img](/assets/image/posts/2015-1-16-ADB/4.jpg)

5.重启Android Studio，正常启动完成。

![img](/assets/image/posts/2015-1-16-ADB/5.jpg)

