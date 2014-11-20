---
layout: post
title: "给目录下的所有文件添加前缀"
date: 2013-07-25 09:39
comments: true
categories: IOS
---
{% codeblock lang:python %}
import os
def rename():
    path = input("给目录下的文件添加前缀")
    print(path)
    Prefix = "PRE_"
    for (path, dirs, files) in os.walk(path):#遍历目录树
        for filename in files:
            ext = os.path.splitext(filename)[1]
            all = os.path.normpath(filename)
            oldpath = path + "/" + filename
            newpath = path + "/" + Prefix + filename
            os.rename(oldpath, newpath)



if __name__ == '__main__':
    print("给png文件添加@2x")
    rename()
    print("finish")
