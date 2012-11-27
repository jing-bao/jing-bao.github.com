---
layout: post
title: "hello android 1：Linux下开发环境搭建"
date: 2012-11-25 14:46
comments: true
categories: [android, helloworld]
---
想试试android的游戏开发，但想换台笔记本做，于是翻出了之前自己写的环境搭建入门，居然已经是半年前了……我果然没什么坚持能力……去android开发官网看了下，google在API Level 17下提供了新的ADT Bundle，集成了Eclipse和ADT plugin，意味着环境搭建被大大简化，再也不用卡在ADT安装的各种网络连接问题上了:)

于是新开了一个ubuntu12.04虚拟机，重新搭建了一次开发环境。根据官网所说，如果已经有eclipse或者其他IDE，可以继续使用单独的SDK安装。半年前的搭建入门在[这里](/blog/2012/11/26/old-hello-android-1-dot-0/),供参考

以下为使用ADT Bundle的具体环境搭建过程
<!--more-->
##需要准备的安装文件
- adt-bundle-linux-x86.zip 

	ADT Bundle 包括了开发环境所需的绝大多数内容
	
	- Eclipse + ADT plugin
	- Android SDK Tools
	- Android Platform-tools
	- The latest Android platform
	- The latest Android system image for the emulator

- jdk-7u9-linux-i586.tar.gz

将所有安装文件放入一个文件夹，例如~/android

###下载地址  
[下载adt bundle](http://developer.android.com/sdk/index.html)

[下载jdk](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

##Java环境安装配置 
###1.解压文件
进入安装文件目录`cd ~/android/`
 
在lib下创建目录用于安装`sudo mkdir /usr/lib/java`

解压到目录下`sudo tar zxvf ./jdk-7u9-linux-i586.tar.gz -C /usr/lib/java/`
###2.配置环境变量
	sudo gedit /etc/profile
在其中加入如下内容： 

	export JAVA_HOME=/usr/lib/java/jdk1.7.0_09
	export JRE_HOME=${JAVA_HOME}/jre
	export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
	export PATH=${JAVA_HOME}/bin:$PATH

保存退出，重启ubuntu
###3.验证是否安装成功
执行命令`java -version`

显示  
	java version "1.7.0_09"

说明安装成功
##Android Adt Bundle安装 
###1.解压文件
进入安装文件目录`cd ~/android/`

解压`unzip ./adt-bundle-linux-x86.zip` 
###2.首次启动
进入解压后的adt-bundle-linux-x86/eclipse目录，执行eclipse文件，即可启动eclipse。第一次启动可以选择workspace和是否参加google信息收集
##Android SDK Manager配置

Adt Bundle自带了最新版本的android平台，要开发其他平台的android程序或其他工具包，可以在SDK Manager中安装

进入android sdk安装目录下的tools目录，运行Android SDK Manager

	cd ~/android/adt-bundle-linux-x86/sdk/tools/
	./android

选中所需的平台进行安装，我安装了2.3.3，这是我手机的android版本

{% img /images/hello-android-1-sdkManager.png 'sdkManager' '图片加载失败 :('  %}

在tools>options中，可以根据网络情况设置代理或选中  
force https://... sources to be fetched using http://...

google要求必须安装的包是

- SDK Tools
- SDK Platform-tools
- SDK Platform（至少一个版本）

推荐安装的包是

- System Image（便于使用android模拟器）
- Android Support
- SDK Samples

