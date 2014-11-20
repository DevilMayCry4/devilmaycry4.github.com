title: ios7 tips
date: 2013-10-21 13:56:35
tags: IOS
---
<p>当以navigation的方式pushcontroller的时候，如果plist表的View controller-based status bar appearance = yes时、状态栏的显示隐藏由navgtion里的viewcontrollers自己控制、状态栏的风格由navigation自己控制
</p>
<p>
ios7如果某个item左右两边的items不相等，即使左右各放置一个UIBarButtonSystemItemFlexibleSpace也是没法使这个item居中的、可以放置一些隐藏的item使两边相等。
</p>

<p>
调整viewcontroller适配ios7状态栏
</p>

if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 7) 
{
        self.view.bounds = CGRectMake(0, -20, self.view.frame.size.width, self.view.frame.size.height );
    }
 
 
 <p>
 参考：http://www.ifun.cc/blog/2013/09/28/gua-pei-ios7kai-fa/
 <p>