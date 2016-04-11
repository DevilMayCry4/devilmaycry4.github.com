title: 使用python编译ipa，并上传到FTP
date: 2014-11-19 13:24:33
tags: IOS
---
<p>
人工发布版本需要archive完之后、拖到itunes里、再把ipa上传到FTP上、费时费力、下面的这个python脚本可以自动完成这些工作、而且完成的时候、发出通知、并把ipa的ftp地址放到粘贴板里。
<a href="https://github.com/DevilMayCry4/python/blob/master/build.py">脚本地址</a>
<p>

<p>
这个脚本只需要设置
DefaultProjectDir:默认的工程文件所在文件夹,或者工程文件的路径。
OutPutDir：#ipa 文件输出的文件夹
还有些ftp的设置
ipa的名字是productname+.月+.日,
如果输出文件夹已经有该名字，会在后面加上数字、依次递增
</p>

<p>
可以在终端中执行这个脚本、这个脚本可以接收程文件所在文件夹,或者工程文件的路径的参数，如果没有使用默认的路径。形如：python /users/user/build.py /users/user/documents/project
</p>
<p>
也可以给这个脚本执行的权限、就不用写得这么复杂，
先执行:chmod +x  /users/user/build.py
在添加环境变量：vim .profile
加上 export PATH=/users/user/build.py:$PATH
执行的时候使用：./build.py  /users/user/documents/project
</p>
<a href="mailto:aaa@5icool.org">发送邮件</a>

<a href="tel:18688888888">拨号</a>   
<a href="sms:18688888888">发短信</a>


