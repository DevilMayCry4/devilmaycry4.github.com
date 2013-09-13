---
layout: post
title: "svn 上传.a文件"
date: 2013-04-22 11:18
comments: true
categories: IOS
---
<p>1. 在每个用户主文件夹下有一个名为.subversion的隐藏文件夹，打开里面的config文件。</p>
<p>2. 查找 [miscellany] 字段，即可看到下面有个 global-ignores 键名，默认为注释掉了的，这表示SVN已经将它们作为默认值了。</p>
<p>3. 取消注释，把 *.so *.so.[0-9]* *.a 去掉，当然你也可以根据需要增加或减少你的过滤选项。</p>
