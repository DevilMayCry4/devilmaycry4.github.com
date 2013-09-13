---
layout: post
title: "python 去掉@2x脚本"
date: 2013-05-29 11:47
comments: true
categories: Other
---
{%  codeblock lang:py %}
import os
def rename():
    path = input("请输入要处理的文件夹路径")
    print(path)
    old_ext = "@2x.png"
    new_ext = ".png"
    for (path, dirs, files) in os.walk(path):#遍历目录树
        for filename in files:
            ext = os.path.splitext(filename)[1]
            all = os.path.normpath(filename)
            a = all.find(old_ext)
            if(a != -1):
                newname = filename.replace(old_ext,new_ext)
                oldpath = path + "/" + filename
                newpath = path + "/" + newname
                os.rename(oldpath, newpath)


if __name__ == '__main__':
    print("去掉png文件命中的@2x")
    rename()
    print("finish")
{% endcodeblock %}
