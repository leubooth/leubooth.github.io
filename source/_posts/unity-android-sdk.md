---
title: Unity && Android && SDK
date: 2017-04-24 17:00:00
tags: 技术分享
---

---

{% asset_img 0.jpg unity-android %}

---

<!-- more -->

### 0.开篇

众所周知，Unity3D 是一个风靡全球的游戏引擎。

广大的游戏开发者能够在 Unity3D 上开发出针对各个平台的优秀作品。

Android 就是众多平台其中的一个。

### 1.安卓系统

**Android，中文俗称安卓，是一个基于 Linux 内核的开放源代码移动操作系统。**

由 Google 成立的 Open Handset Alliance（OHA，开放手持设备联盟）持续领导与开发，主要设计用于触屏移动设备如智能手机和平板电脑与其他便携式设备。

Android 最初由安迪·鲁宾（Andy Rubin）等人开发制作，最初开发这个系统的目的是创建一个数码相机的先进操作系统；但是后来发现市场需求不够大，加上智能手机市场快速成长，于是 Android 被改造为一款面向智能手机的操作系统。

于2005年7月11日被美国科技企业 Google 收购。

2017年3月，Android 全球网络流量和设备超越 Microsoft Windows，正式成为全球第一大操作系统。

Android 一词最早出现于法国作家利尔亚当（Auguste Villiers de l'Isle-Adam）在1886年发表的科幻小说《未来夏娃》（L'Ève future）中。

他将外表像人的机器人取名为 Android。

- <https://w.magictavern.com/display/BAC/Android+Development+Basic?preview=/2886187/2886188/Android%20Learner.pptx>

- Android 操作系统的核心属于 Linux 内核的一个分支，具有典型的 Linux 调度和功能，除此之外，Google 为了能让 Linux 在移动设备上良好的运行，对其进行了修改和扩充。

- 安卓系统架构分为四个层：
- 最底层为 Linux 内核；
- 第二层为库文件和安卓运行时环境；
- 第三层为应用程序框架层；
- 最上层为应用程序。

![image](http://img.blog.csdn.net/20140311140541765)

![image](http://images.cnitblog.com/i/480488/201407/211342070883976.jpg)

- 安卓开发的四大组件
	- Activity
	- Service
	- Broadcast Receiver	
	- Content Provider 
	- <https://developer.android.com/guide/components/fundamentals.html#Components>
- 虽然 Android 操作系统中的应用程序大部分都是由 Java 编写的，但是 Android 却是以转换为 Dalvik executables（.dex） 的文件在 Dalvik 虚拟机上运行的。由于 Android 中并不自带 Java 虚拟机，因此无法直接运行 Java 程序。不过 Android 平台上提供了多个 Java 虚拟机供用户下载使用，安装了 Java 虚拟机的 Android 系统可以运行 Java_ME 的程序。5.0 版（Lolipop）开始以 Android Runtime（ART）取代 Dalvik 虚拟机。
- Java 
1990 oak oracle 
oracle 和 google 打官司

- 安卓系统特性：显示布局，数据存储，网络，信息，语言，浏览器，Java，媒体支持，流媒体支持，硬件支持，多点触控，蓝牙，多任务处理，语音功能，无线共享功能，截图功能。

- 安卓系统版本命名 使用了从小到大的各种甜点。（4.0.3 Ice Cream Sandwich 冰淇淋三明治 API 15）

- Android应用程序包 (英语：Android application package，APK) 是Android操作系统使用的一种应用程序包文件格式，用于分发和安装移动应用及中间件。一个Android应用程序的代码想要在Android设备上运行，必须先进行编译，然后被打包成为一个被Android系统所能识别的文件才可以被运行，而这种能被Android系统识别并运行的文件格式便是“APK”。 一个APK文件内包含被编译的代码文件(.dex 文件)，文件资源（resources）， assets，证书（certificates），和清单文件（manifest file）。APK 文件基于 ZIP 文件格式，它与JAR文件的构造方式相似。它的互联网媒体类型是application/vnd.android.package-archive.
- asset 下存有 unity 导出后的大部分资源。

### 2.Android 在 Unity 中的应用场景

	- unity 是如何编译成安卓工程或者apk的
	- elipse 是什么
	- android studio 是什么
	- unity 编辑器中的实际应用场景
		- Build Settings
			- Texture Compression
			- 导出工程 eclipse gradle
			- Develop build
			- autoconnecto profiler
			- script debugging
		- Player settings
			- Disable Depth and stencil
			- Other Settings
			- Publish Settings
			- Keystore and Key alias
		- Preference
			- SDK / JDK / NDK
		- Editor Settings
			- Unity Remote

### 3.Unity 与 Java 通信

<https://docs.unity3d.com/Manual/PluginsForAndroid.html>

### 4.如何编写中间件

现状：当下很多第三方的 SDK 只有针对原生 Android 系统的组件包，并没有针对 Unity 的。而每次导出为 Google 工程实现接入又太耗费时间和精力，阻碍了自动化开发。所以有必要针对第三方组件，针对 Unity 编写中间组件，免去中间的接入步骤。

- Jar
- AAR
- 导出 AAR
- 
		
