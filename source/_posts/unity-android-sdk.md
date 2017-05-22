---

title: Unity && Android && SDK
date: 2017-04-24 17:00:00
tags: 技术分享

---

---

{% asset_img 0.jpg unity-android-sdk %}

---

<!-- more -->

## 0.开篇

<!-- 抓耳挠腮图 -->

隔壁老王又在捶桌子了，抓耳挠腮，一副要把手里的谷歌亲儿子摔掉的架势。

上前一问才知道，是因为用 Unity 接入安卓的 SDK 导致游戏包闪退，闪了一天了，还没找到问题。

遇到这种情况也确实挺崩溃的，尤其是距离游戏更新的审判日没几天了。

我决定帮老王解决这个问题。

## 1.安卓

### 安卓系统的历史

- **Android，中文俗称安卓，是一个基于 Linux 内核的开放源代码移动操作系统。**由 Google 成立的 Open Handset Alliance（OHA，开放手持设备联盟）持续领导与开发，主要设计用于触屏移动设备如智能手机和平板电脑与其他便携式设备。

- Android 最初由安迪·鲁宾（Andy Rubin）等人开发制作，最初开发这个系统的目的是创建一个数码相机的先进操作系统；但是后来发现市场需求不够大，加上智能手机市场快速成长，于是 Android 被改造为一款面向智能手机的操作系统。

- 于2005年7月11日被美国科技企业 Google 收购。

- 2017年3月，Android 全球网络流量和设备超越 Microsoft Windows，正式成为全球第一大操作系统。

### 安卓名称的由来

- Android 一词最早出现于法国作家利尔亚当（Auguste Villiers de l'Isle-Adam）在1886年发表的科幻小说《未来夏娃》（L'Ève future）中。他将外表像人的机器人取名为 Android。

### 安卓系统架构与设计

- **Android 操作系统的核心属于 Linux 内核的一个分支**，具有典型的 Linux 调度和功能，除此之外，Google 为了能让 Linux 在移动设备上良好的运行，对其进行了修改和扩充。

- 安卓系统特性：显示布局，数据存储，网络，信息，语言，浏览器，Java，媒体支持，流媒体支持，硬件支持，多点触控，蓝牙，多任务处理，语音功能，无线共享功能，截图功能。

- 安卓系统架构分为四个层：
	- 最底层为 Linux 内核；
	- 第二层为库文件和安卓运行时环境；
	- 第三层为应用程序框架层；
	- 最上层为应用程序。

---

![image](http://img.blog.csdn.net/20140311140541765)

![image](http://images.cnitblog.com/i/480488/201407/211342070883976.jpg)

---

- 虽然 Android 操作系统中的应用程序大部分都是由 Java 编写的，但是 Android 却是以转换为 Dalvik executables（.dex） 的文件在 Dalvik 虚拟机上运行的。由于 Android 中并不自带 Java 虚拟机，因此无法直接运行 Java 程序。不过 Android 平台上提供了多个 Java 虚拟机供用户下载使用，安装了 Java 虚拟机的 Android 系统可以运行 Java_ME 的程序。5.0 版（Lolipop）开始以 Android Runtime（ART）取代 Dalvik 虚拟机。

- 安卓开发的四大组件:
	- Activity
	- Service
	- Broadcast Receiver
	- Content Provider

- <https://developer.android.com/guide/components/fundamentals.html#Components>

- <https://w.magictavern.com/display/BAC/Android+Development+Basic?preview=/2886187/2886188/Android%20Learner.pptx>

### 安卓与Java

- Java是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级Web应用开发和移动应用开发。

- 任职于太阳微系统的詹姆斯·高斯林等人于1990年代初开发Java语言的雏形，最初被命名为Oak，目标设置在家用电器等小型系统的程序语言，应用在电视机、电话、闹钟、烤面包机等家用电器的控制和通信。由于这些智能化家电的市场需求没有预期的高，Sun公司放弃了该项计划。随着1990年代互联网的发展，Sun公司看见Oak在互联网上应用的前景，于是改造了Oak，于1995年5月以Java的名称正式发布。Java伴随着互联网的迅猛发展而发展，逐渐成为重要的网络编程语言。

- 与甲骨文公司的Java纠纷

关于甲骨文公司就Android所使用的开发语言平台Java为最引人关注的权利纠纷事件。
2010年8月，甲骨文公司就开始对Google无授权使用Java语言实现侵犯了公司的专利在美国加州北区地方法院提起控诉，要求高达90亿美元的赔偿，其中牵涉了原供职于Sun公司的Java开发人员在转职Google开发Android的Java实现使用了原公司的实现，API接口的实现是否具有专利版权性，Android的Java实现是否对甲骨文公司的Java移动平台系列产品做成冲击而形成不正当垄断等问题。
2012年5月的诉讼结果为陪审团支持Google的诉求，认为API只是系统或操作的方法，不受版权保护。
2012年10月甲骨文公司上诉，2014年5月，美国联邦巡回上诉法院认为API属于“计算机程序”仍受版权保护，判决Android侵犯了甲骨文公司Java的版权，但并不排除谷歌对其拥有合理使用性的权利。
2014年10月Google向美国最高法院申请调卷令，请求最高院介入。
2015年6月29日调卷令被拒绝，发往旧金山联邦法院进行审理。
2016年5月，旧金山联邦法院陪审团认定Android实现Java的API命名结构属于合理使用，不构成侵权，最终判Google胜诉。
2016年8月22日，Google在最新的Android 7.0 Nougat中，将专利的JDK替换成开源方案的OpenJDK，以彻底解决Java的专利问题。

### 安卓与系统版本命名

- 安卓系统版本命名使用了从小到大的各种甜点。（4.0.3 Ice Cream Sandwich 冰淇淋三明治 API 15 97%+）

### 安卓与APK

- Android应用程序包 (英语：Android application package，APK) 是Android操作系统使用的一种应用程序包文件格式，用于分发和安装移动应用及中间件。一个Android应用程序的代码想要在Android设备上运行，必须先进行编译，然后被打包成为一个被Android系统所能识别的文件才可以被运行，而这种能被Android系统识别并运行的文件格式便是“APK”。 一个APK文件内包含被编译的代码文件(.dex 文件)，文件资源（resources）， assets，证书（certificates），和清单文件（manifest file）。APK 文件基于 ZIP 文件格式，它与JAR文件的构造方式相似。它的互联网媒体类型是application/vnd.android.package-archive.

- Unity 导出APK后的大部分资源都在 asset 下。

## 2.Unity 发布安卓 APK，都干了些什么

The Internal build system creates an APK by invoking Android SDK utilities in a specific order. Unity automatically performs a number of steps to create the APK, including:

- Preparing and building the Unity Assets
- Compiling scripts
- Processing the plug-ins
- Splitting the resources into the parts that go to the APK and the OBB, if Split Application Binary is selected
- Building the Android resources using the AAPT utility
- Generating the Android manifest and merging the library manifests into it
- Compiling the Java code into the Dalvik Executable format (DEX)
- Building the IL2CPP library, if IL2CPP Scripting Backend is selected
- Building and optimizing the APK and OBB packages

Unity 会按照一定次序调用 Android SDK 命令行，构建 APK

- 准备和构建 Unity 资源
- 编译脚本
- 处理插件
- 如果勾选程序包分离，将资源分离到 APK 和 OBB 包
- 使用AAPT工具构建安卓资源
- 生成安卓配置清单文件，并将各类库的子配置清单合并到主配置清单中
- 将 Java 代码编译为可执行的 .dex 文件
- 如果勾选了 IL2CPP ，构建 ILCPP 类库
- 构建和优化 APK 和 OBB 包

- <https://docs.unity3d.com/Manual/android-BuildProcess.html>

## 3.Unity 编辑器中 Android 相关设置

- Unity / Preferences / External Tools
	- Android SDK
	- JDK
	- NDK

<!-- image here -->

- File / Build Settings / Android
	- Texture Compression
		- <https://docs.unity3d.com/Manual/class-TextureImporterOverride.html>
	- Build System
		- Internal
		- Gradle
		- ADT
	- Export Project
	- Development Build
	- Autoconnect Profiler
	- Script Debugging
	- Player settings
		- Disable Depth and Stencil
		- Other Settings
		<!-- image here -->
		- Publish Settings
			- Keystore and Key
			- Split Application Binary

	<!-- image here -->
	
- Edit / Project Settings / Editor / Unity Remote
<!-- image here -->

## 4.Unity 与 Java 通信

<https://docs.unity3d.com/Manual/PluginsForAndroid.html>

## 5.如何编写中间件

现状：当下很多第三方的 SDK 只有针对原生 Android 系统的组件包，并没有针对 Unity 的。而每次导出为 Google 工程实现接入又太耗费时间和精力，阻碍了自动化开发。所以有必要针对第三方组件，针对 Unity 编写中间组件，免去中间的接入步骤。

- Jar
- AAR
- 导出 AAR

## 6.Unity 集成多个安卓 SDK

通常来讲，上线的项目在复杂的需求环境下可能需要接入若干第三方应用程序开发包，而这些开发包“们”在融合之初可能并不会预见到彼此会相遇。

尤其在软硬件差异化如此巨大的安卓平台，一行配置冲突可能就会导致程序发布失败，甚至崩溃闪退。
再加上我们使用的 Unity 引擎开发游戏，又依赖 Unity 自动构建管线 （会自动构建 APK 包），并没有在 Android IDE 下集成开发的有利环境。

现在主流的做法都是将中间件在 Android IDE 中完成封装，连同 SDK 一同导入到 Unity 编辑器中，最终编译到 APK 包中完成集成。

这样做的好处有，中间件只需要编写一次，在下次更新中间件之前，都不需要再做其他额外的工作了。
弊端也就是在编写的时候，错误率高，容易造成一些未知的错误；有新的 SDK 集成进来之后可能会与老的中间件冲突，增加额外繁琐的工作量。

当然，这些问题也是可以通过规范的流程很好的解决的。

- 编写中间件
	- 重写 Application 入口类
		
		遇到很多的情况是，一些 SDK 可能由于一些未知的原因，要求在应用的 Application 入口的 onCreate 生命周期里搞事情。
		
		当然简单的做法就是写一个 Java 类继承自 Application 类，覆写 onCreate 方法，然后在应用程序的 AndroidManifest.xml 中 Application 节点中的 android:name 属性中填写这个类。
		应用启动后会第一时间执行 onCreate 方法体里的内容。
		
		如果在一个工程中有多个 SDK 都需要覆写 Aplication 入口，那么有以下方法供选择：
		首选方案：编写订制通用 Application 入口类，将所有需要覆写的内容都写在这个类中。统一管理，修改方便。
		
		次选方案：如果 Application 的入口已经被某 SDK 强行继承，并且封装成程序包不能做修改时。可以选择依次继承，A 继承 Application , B 继承 A 。AndroidManifest.xml  中的 android:name 填写最终的继承者 （B）。

	- 重写 UnityPlayerActivity 入口类

	Unity 发布的应用到安卓的原理是，整个游戏内容作为安卓中的一个 Activity 存在。
	
	UnityPlayerActivity 继承自 Activity ，作为安卓程序启动后第一个被唤醒的入口 Activity。
	
	同样，很多 SDK 也需要在 Activity 被唤醒的时候执行一些逻辑，或者在这个 Activity 生命周期中的某个节点，响应某些操作。
	
	建议的做法和处理 Application 入口一样，可以选择编写统一的订制入口 Activity ，继承自 UnityPlayerActivity 类。或者采取依次继承的方法。
	
	- 中间件类库模块化

		在编写中间件类库的时候，可能需要注意的地方：
		
		一般来讲，为了保证中间件能编译成功，会将一些其他引用到的类库导入到中间件工程中，但是我们往往不会把这些类也编译到中间件中。除非能够确定这些类不会在 Unity 工程中再出现一份，否则就会编译报错。
		
		要尽量减少因为中间件包名或者类名的命名，给后来使用者带来的困惑。比如一个类 com.a.b.ClassC，发现使用的时候实现的是 D 功能。或者一个局域的包名，在实现全局的功能。
		
