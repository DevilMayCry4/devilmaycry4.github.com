---
layout: post
title: "Mac 虚拟机搭建 WP8开发环境"
date: 2013-04-21 20:40
comments: true
categories: windowsphone
---
<p>使用Vmware Fusion 5 安装win8 pro 
打开虚拟机文件包，在***.vmx文件中添加
hypervisor.cpuid.v0 = “FALSE”
vhv.enable = ”TRUE“
在虚拟机设置---->高级中,首选虚拟化引擎选项选择 带有拓展页面表的 Intel VT-X
处理器和内存设置选项，chu处理器核心选择2个以上
并在高级选项中选中 在此虚拟机中启用虚拟化管理程序</p>