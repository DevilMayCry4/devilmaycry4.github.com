---
layout: post
title: "FTP文件类型"
date: 2013-05-22 09:54
comments: true
categories: IOS
---
<p>
IOS FTP获取文件列表信息后。kCFFTPResourceType 对应的是文件类型。这个在 sys/dirent.h 头文件中有定义。
</p>


#define	DT_UNKNOWN	 0
#define	DT_FIFO		 1
#define	DT_CHR		 2
#define	DT_DIR		 4
#define	DT_BLK		 6
#define	DT_REG		 8
#define	DT_LNK		10
#define	DT_SOCK		12
#define	DT_WHT		14




DT_UNKNOWN    未知类型

DT_RET        普通文件

DT_DIR        目录文件

DT_FIFO       命名管道

DT_SOCK       本地套接口

DT_CHR        字符设备文件

DT_BLK        块设备文件


