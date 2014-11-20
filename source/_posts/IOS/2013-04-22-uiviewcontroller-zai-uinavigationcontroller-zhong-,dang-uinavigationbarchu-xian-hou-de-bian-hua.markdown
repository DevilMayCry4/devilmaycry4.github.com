---
layout: post
title: "UIViewController 在UINavigationController 中，当UINavigationBar出现后的变化"
date: 2013-04-22 12:03
comments: true
categories: IOS
---

<p>如果UIViewController 是在导航控制器中
在navigationbar 出现后self.view 的高度减少了44。如：
在viewDidLoad 中 </p>


NSLog(@"self height %f",self.view.frame.size.heigt);  
 
 <p>
在viewDidAppear 中 </p>

NSLog(@"self height %f",self.view.frame.size.heigt);  
 
<p> 两个结果相差 44。</p>