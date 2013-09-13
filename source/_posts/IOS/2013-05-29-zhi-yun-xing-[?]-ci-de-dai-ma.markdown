---
layout: post
title: "只运行一次的代码"
date: 2013-05-29 15:37
comments: true
categories: IOS
---
{% codeblock lang:objc %}
  static dispatch_once_t onceToken;
        dispatch_once(&onceToken, ^{
            <#code to be executed once#>
        });
{% endcodeblock %}
