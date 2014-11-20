---
layout: post
title: "Xcode Debug"
date: 2013-07-01 09:43
comments: true
categories: IOS
---

#ifdef DEBUG
#   define NSSLog(fmt, ...) {NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__);}
#else
#   define NSSLog(...)
#endif

