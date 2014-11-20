---
layout: post
title: "系统UI本地化"
date: 2013-05-14 12:18
comments: true
categories: IOS
---

<p>
在iOS应用中，有时候会需要调用系统的一些UI控件，例如：

在UIWebView中长按会弹出系统的上下文菜单
在UIImagePickerController中会使用系统的照相机界面
在编译状态下的UITableViewCell，处于待删除时，会有一个系统的删除按钮。
以上这些UI控件中，其显示的语言并不是和你当前手机的系统语言一致的。而是根据你的App内部的语言设置来显示。结果就是，如果你没有设置恰当的话，你的中文App可能会出现一些英文的控件文字。
</p>

<p>
用vim直接打开工程的Info.plist文件，在文件中增加如下内容即可：
</p>


<key>CFBundleLocalizations</key>
   <array>
           <string>zh_CN</string>
           <string>en</string>
   </array>


<p>from:
<a href="http://blog.devtang.com/blog/2013/01/23/set-ios-system-ui-language/">
http://blog.devtang.com/blog/2013/01/23/set-ios-system-ui-language/
</a>
</p>
