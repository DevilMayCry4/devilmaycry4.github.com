---
layout: post
title: "NSUserDefaults standardUserDefaults注意事项"
date: 2013-04-22 14:34
comments: true
categories: IOS
---
<p>NSUserDefaults standardUserDefaults一种便利的序列化方式、当使用</p>


[[NSUserDefaults  standardUserDefaults] setObject:object textforKey:key];


<p>这时候该对象只是写在内存中。要真正保存到磁盘、应该还要调用</p>


[[NSUserDefaults  standardUserDefaults] synchronize];


