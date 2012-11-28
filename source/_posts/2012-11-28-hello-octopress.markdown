---
layout: post
title: "hello octopress"
date: 2012-11-28 13:56
comments: true
categories: [helloworld]
---
理论上来说，这才应该是我第一篇博客……描述博客的搭建过程。只是作为一个博客菜鸟 + octopress菜鸟 + linux菜鸟，我其实也是依葫芦画瓢，照着官网的步骤和别人的博客一步一步做的。所以过程就不赘述了，列一下我的主要参考网站和解决的一些小问题吧
<!--more-->
##主要参考网站
- [octopress官网](http://octopress.org/)

	官网的文档写的非常简明清晰，其实不需要其他教程，基本上照着做就行了

- [搭建参考](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)

	这篇博文总结了根据国内需求进行的一些配置，比如删除twitter和增加分享到国内微博等等

- [markdown语法](http://wowubuntu.com/markdown/#link)

	我是最近用github之后才开始接触markdown的，所以还是经常要查一下语法

##遇到的问题
###用rbenv安装ruby
我不得不说，如果没有特殊理由的话，还是用rvm吧……万一遇到问题，网上可以搜到很多解决方法。我没了解过ruby，随意挑了rbenv来安装，过程实在是纠结……

####1.ruby的版本
官网上在[Installing Ruby With Rbenv](http://octopress.org/docs/setup/rbenv/)页面说用`rbenv install 1.9.3-p0`安装ruby，但我clone的octopress项目中`.rbenv-version`文件其实写的是`1.9.3-p194`，所以会提示版本未安装。直接按照`.rbenv-version`里要求的版本安装就行。

####2.缺少zlib依赖
	sudo apt-get install zlib1g-dev

####3.缺少openssl依赖
这个问题纠结了我半天，反复卸载安装了好几次。网上的解决方案大多是针对rvm的，搜寻资料也很痛苦。最后在[这里](https://gist.github.com/1200482)找到了解决方案

安装依赖库，我开始只安装了openssl，是不够的

	sudo apt-get install openssl
	sudo apt-get install libssl-dev
	sudo apt-get install libssl0.9.8
安装rbenv，按照官网来就行

安装ruby-build

	git clone git@github.com:sstephenson/ruby-build.git
	cd ruby-build
	./install.sh
然后才能安装ruby

	ruby-build 1.9.3-p194 ~/.rbenv/versions/1.9.3-p194 --with-openssl-dir=/usr/local
	rbenv rehash
如果要设置全局ruby的版本的话可以用`rbenv global 1.9.3-p194`

进入octopress文件夹下可以查询ruby是否安装成功`ruby -v`

检查下openssl是否正常，应该返回`true`

	irb
	require 'openssl' 
###用root用户`rake deploy`时提示ssh密钥错误
这大概是只有我这种小白才会犯的错误……是因为root用户和普通用户存放密钥的文件夹不一样，所以尽量用同一个用户操作，或者把密钥拷贝一份

###添加Jiathis分享插件时显示一小块空白
参考[这篇博文](http://geeksavetheworld.com/blog/2012/11/05/add-jiathis-to-octopress-blog/)

###在侧栏添加分类列表
我主要是觉得侧栏太空了……参考[这篇博文](http://codemacro.com/2012/07/18/add-category-list-to-octopress/)
##其他
其实对octopress博客的个性化还有很多，可以搜到很多教程，只是我比较懒。作为有一个菜鸟，我觉得默认主题已经很漂亮啦:)

我之前做项目接触了github，然后搜教程的时候突然发现可以用它写博客，所以一时好奇才写的。希望能记录下平时看到或者学到的东西，虽然水平有限，不值一哂，但对我自己来说毕竟是花过力气的东西，还是留些痕迹做纪念吧，也省得经常觉得自己很颓废

最近会整理一些以前的文档，用【旧】标注的文章可能会有些过时。我其实是个好奇心甚重的人，但是有点浅尝辄止的毛病，所以可以预感到我的“helloworld”分类下会有很多乱七八糟的东西……
###希望我能坚持下去