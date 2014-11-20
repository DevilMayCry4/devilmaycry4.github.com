---
layout: post
title: "只运行一次的代码"
date: 2013-05-29 15:37
comments: true
categories: IOS
---

  static dispatch_once_t onceToken;
        dispatch_once(&onceToken, ^{
            <#code to be executed once#>
        });

