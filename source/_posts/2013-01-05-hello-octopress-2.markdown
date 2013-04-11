---
layout: post
title: "hello octopress 2：流程记录"
date: 2013-01-05 18:01
comments: true
categories: [helloworld]
---
打了一段时间页游，好久不更新这里了，果然意志不坚啊。再写的时候生疏了很多，所以简单记录一下整个流程，方便以后参考。
<!--more-->
进入octopress的目录
```
cd /directory/to/octopress
```
使配置生效。这个应该可以改变写配置命令的地方使其自动生效吧，我懒得改了。
```
source ~/.bash_profile
```
生成新博客，注意方括号之前不要留空格
```
rake new_post["title"]
```
修改生成的页面，其中category的写法是这样滴
```
# Multiple categories example 1
categories: [CSS3, Sass, Media Queries]
```
写好后在本机4000端口实时预览
```
rake preview
```
生成文件
```
rake generate 
```
发布文件到github
```
rake deploy 
```
搞定！

可以不时备个份
```
git add .
git commit -m "changes"
git push origin source
```