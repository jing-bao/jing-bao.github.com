---
layout: post
title: "『旧』 hello android 1.0：Linux下开发环境搭建"
date: 2012-11-26 23:32
comments: true
categories: [android, helloworld, old]
---
这是4月左右写的，软件的版本可能比较旧了，仅供参考

以下为搭建过程，包括文件下载、java安装、eclipse和ADT安装、SDK Manager配置等
<!--more-->
##需要准备的安装文件
- android-sdk_r17-linux.tgz
- ADT-17.0.0.zip
- jdk-7u3-linux-i586.tar.gz 
- eclipse-jee-indigo-SR2-linux-gtk.tar.gz
###下载地址
[http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)

[http://developer.android.com/sdk/eclipse-adt.html](http://developer.android.com/sdk/eclipse-adt.html)

[http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

[http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/)

将其拷贝到一个文件夹，以/home/android/为例
##Java环境安装配置 
###1.解压文件
进入安装文件目录`cd /home/android/`

在lib下创建目录用于安装`sudo mkdir /usr/lib/java`

解压到目录下`sudo tar zxvf ./jdk-7u3-linux-i586.tar.gz -C /usr/lib/java/`
###2.配置环境变量
	sudo gedit /etc/bash.bashrc
在其中加入如下内容： 

	export JAVA_HOME=/usr/lib/java/jdk1.7.0_03
	export JRE_HOME=${JAVA_HOME}/jre
	export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
	export PATH=${JAVA_HOME}/bin:$PATH
保存退出，重新开启终端
###3.验证是否安装成功
执行命令

	java -version 
显示
	
	java version "1.7.0_03" 
	Java(TM) SE Runtime Environment (build 1.7.0_03-b04) 
	Java HotSpot(TM) Client VM (build 22.1-b02, mixed mode)
说明安装成功
##eclipse安装
进入安装文件目录`cd /home/android/`

解压文件`tar zxvf eclipse-jee-indigo-SR2-linux-gtk.tar.gz` 

进入解压后的eclipse目录，执行eclipse文件，即可启动eclipse
##android SDK安装
###1.解压文件
进入安装文件目录`cd /home/android/` 

解压文件`tar zxvf android-sdk_r17-linux.tgz` 
###2.安装ADT插件
在线安装容易连接不上网站，所以选择离线安装

启动 Eclipse, 在菜单栏选择 Help > Install New Software.... 

单击右上角的Add按钮，在弹出的对话框中，Name填写 "ADT",单击Archive按钮，找到插件所在到包ADT-17.0.0.zip，点击OK

{% img /images/hello-android-1-0-add.png 'add repository' '图片加载失败 :('  %}

选择所有安装包，点击next根据提示进行安装，同意所有条款。把Contact all update sites during install to find required software 前面的复选框取消掉，否则下载会很慢

{% img /images/hello-android-1-0-adt-install.png 'ADT install' '图片加载失败 :('  %}

安装完重启eclipse，开头的对话框可以先取消，后续会安装

在eclipse中选择Window > Preferences..，在弹出的对话框中左边选择Android标签，右边SDK location选择Android SDK的目录，单击apply，再单击ok。在此过程中会弹出一个对话框问你是否要参加google的满意度调查报告，可以选择参加还是不参加，单击process按钮即可

{% img /images/hello-android-1-0-preferences.png 'eclipse preferences' '图片加载失败 :(' %}

###3.配置Android SDK Manager
进入android sdk安装目录下的tools目录，运行Android SDK Manager
	cd /home/android/android-sdk-linux/tools/ 
	./android 
在tools>options选中force https……http，等待列表刷新后，选中tools和想要版本的android平台，进行安装

{% img /images/hello-android-1-0-sdkManager-setting.png 'SDK Manager setting' '图片加载失败 :('  %}
{% img /images/hello-android-1-0-sdkManager.png 'SDK Manager' '图片加载失败 :('  %}

*不知道为什么，从eclipse启动SDK Manager时，tools选项里没有options……推荐从目录运行

*一些第三方插件如htc的工具等，需要注册，可以暂时不安装