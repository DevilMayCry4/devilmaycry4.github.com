---
layout: post
title: "NSTimer释放"
date: 2013-04-22 11:58
comments: true
categories: 
--- IOS
<p>NSTimer 是一个比较麻烦的东东，理解不清楚的时候释放它、很可能会出现crash。
NSTimer的创建一般使用俩种方式：</p>
<p>1， 类方法直接创建：</p>


+ (NSTimer*)timerWithTimeInterval:(NSTimeInterval)ti invocation:(NSInvocation*)invocation repeats:(BOOL)yesOrNo;

+ (NSTimer*)scheduledTimerWithTimeInterval:(NSTimeInterval)ti invocation:(NSInvocation*)invocation repeats:(BOOL)yesOrNo;

+ (NSTimer*)timerWithTimeInterval:(NSTimeInterval)ti target:(id)aTarget selector:(SEL)aSelector userInfo:(id)userInfo repeats:(BOOL)yesOrNo;

+ (NSTimer*)scheduledTimerWithTimeInterval:(NSTimeInterval)ti target:(id)aTarget selector:(SEL)aSelector userInfo:(id)userInfo repeats:(BOOL)yesOrNo;

<p>由这些方法所创建的NSTimer的retain count是由NSRunloop保持、所以我们不需要对这个对象进行release的操作。但是如果我们想要在某个具体的地方
停止这个计数器的时候。如果repeats参数是YES的时候我们不需要增加这个对象的retain count。就可以在其他地方引用这个对象了。如果repeats参数shi是NO的时候、我们就应该手动增加、再release。如果不retain它、如果在你引用这个对象的时候、定时器的时间还没到，方法还没执行。NSRunloop
还保留这retain count。这时候是没问题的、但是如果方法已经执行完了、NSRunloop释放了NSTimer这个对象。这个时候就会crash了。</p>

<p>2、直接创建：</p]>

- (id)initWithFireDate:(NSDate*)date interval:(NSTimeInterval)ti target:(id)t selector:(SEL)s userInfo:(id)ui repeats:(BOOL)rep;

