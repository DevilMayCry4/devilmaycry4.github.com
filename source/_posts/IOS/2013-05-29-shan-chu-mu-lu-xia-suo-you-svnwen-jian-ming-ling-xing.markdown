---
layout: post
title: "删除目录下所有svn文件命令行"
date: 2013-05-29 11:24
comments: true
categories: IOS
---
{% codeblock lang:c %}
sudo find /Users/user/Desktop/svn目录/ -name ".svn" -exec rm -r {} \;


