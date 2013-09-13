---
layout: post
title: "NSTimer的使用"
date: 2013-06-17 09:37
comments: true
categories: IOS
---
<p>
NSTimer 定时器，在编程的时候是十分常用的，但是其实很多时候都没有用对，我们经常都在Controller里创建NStimer,而期待在controller dealloc的时候，去停止定时器：
</p>
{% codeblock lang:objc %}
- (void)dealloc
{
   if(_timer != nil)
   {
     [_timer invalidate];
   }
   [super dealloc];
}
{% endcodeblock %}
<p>
但实际上contoller永远都不会调用dealloc，因为_timer会retaincontroller一次。
在NSTimer创建的时候、NSRunloop会retian NSTimer一次、已保证NSTimer在执行完之前NSTimer不会被dealloc。而且一般NSTimer创建的时候会有个回调的target、而为了保证回调的正常、NSTimer会
retian target一次、而我们经常使用controller作为target，这样controller就从来都不会释放了。造成内存泄露。
</p>