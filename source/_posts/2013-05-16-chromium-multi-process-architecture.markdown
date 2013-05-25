---
layout: post
title: "『整理翻译』Chromium多进程架构"
date: 2013-05-16 23:58
comments: true
categories: [chromium]
---
不是完全翻译，只是在理解的基础上摘录了大致意思，而且对我自己感兴趣的部分可能详细一点，对其他简略一点

原网址[http://www.chromium.org/developers/design-documents/multi-process-architecture](http://www.chromium.org/developers/design-documents/multi-process-architecture)
<!--more-->
本文档描述了Chromium的上层架构。

##问题
浏览器的渲染引擎容易崩溃，安全性也不完美，且单个网页的问题可能影响整个浏览器关闭。

##架构概览
主要进程称作"browser process"或"browser"，运行UI和管理tab和plugin的进程。针对标签的进程称作"render processes"或者"renderers".
{% img /images/chromium-multi-process-architecture.png 'Chromium架构' '图片加载失败 :('  %}

###管理render processes
每个render process有一个全局的`RenderProcess`对象。browser为每个render process维持一个相应的`RenderProcessHost`。browser和renderers通过IPC通信。

###管理视图（view）
每个render process有一个或多个`RenderView`对象，被`RenderProcess`管理，对应于内容标签。相应的`RenderProcessHost`维持一个`RenderViewHost`。每个view有一个view ID，在一个renderer中是唯一的，但在一个browser中不唯一。

##组件和接口
在render process:

- `RenderProcess`处理IPC，每个render process有且只有一个`RenderProcess`
- `RenderView`与对应的`RenderViewHost`通信（通过`RenderProcess`），以及与我们的WebKit嵌入层通信。这个对象代表网页内容

在browser process:

- `Browser`对象代表顶层浏览器窗口
- `RenderProcessHost`代表单个browser<->renderer IPC连接的browser端。每个render process有一个`RenderProcessHost`在browser中。
- `RenderViewHost`封装和`RenderView`的通信，`RenderWidgetHost`为browser里的`RenderWidget`处理输入和绘制

##共享render process
某些情况下会共享

##检测崩溃或行为错误的renderers
每个IPC连接会监测process句柄。如果句柄接收到信号，说明render process崩溃。

##沙箱
限制renderer对文件系统、网络、用户显示相关对象的访问

##归还内存
当系统需要内存时，后台的标签的render process内存优先被释放，通过`working set`的大小

##插件
NPAPI插件工作在单独进程