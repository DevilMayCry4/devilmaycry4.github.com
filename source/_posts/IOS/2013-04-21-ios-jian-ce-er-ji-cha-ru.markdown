---
layout: post
title: "IOS 检测耳机插入"
date: 2013-04-21 20:46
comments: true
categories: IOS
--- 
   {% blockquote %}
    UInt32 dataSize;
    CFStringRef currentRoute;
    currentRoute = NULL;
    dataSize = sizeof(CFStringRef);
    AudioSessionInitialize(NULL, NULL, NULL, NULL);
    AudioSessionGetProperty(kAudioSessionProperty_AudioRoute, &dataSize, &currentRoute);
    if([(NSString *) currentRoute hasPrefix: @"Headphone"])
    {
        //插入耳机后想执行的操作
    }{% blockquote %}
