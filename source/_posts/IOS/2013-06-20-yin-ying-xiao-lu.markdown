---
layout: post
title: "阴影效率"
date: 2013-06-20 16:24
comments: true
categories: IOS
---
<p>
UIView 如果没有设置阴影的边框会导致、在旋转屏幕或者滑动的时候，界面卡顿，因为设置了阴影的view会不断地重新绘制。解决的方法:
</p>

{% codeblock lang:objc %}
        self.layer.shadowColor = [UIColor blackColor].CGColor;
        self.layer.shadowOffset = CGSizeMake(0, 3);
        self.layer.shadowOpacity = 0.6;
        self.layer.masksToBounds = NO;
        /*********
           self.layer.shadowPath = [UIBezierPath bezierPathWithRect:self.bounds].CGPath;
           *********/
{% endcodeblock %}