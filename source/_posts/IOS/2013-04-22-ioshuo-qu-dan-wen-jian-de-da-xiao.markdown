---
layout: post
title: "IOS获取单文件的大小"
date: 2013-04-22 11:26
comments: true
categories: IOS
---
<p>c语言实现：</p>

#include "sys/stat.h"
- (long long) fileSizeAtPath:(NSString*) filePath
{  
    struct stat st;  
    if(lstat([filePath cStringUsingEncoding:NSUTF8StringEncoding], &st) == 0)
    {  
        return st.st_size;  
    }  
    return 0;  
}  



<p>object-c实现：</p>



-(long long) fileSizeAtPath:(NSString*) filePath
{  
  NSFileManager* manager = [NSFileManager defaultManager];  
  if ([manager fileExistsAtPath:filePath])
  {  
    return [[manager attributesOfItemAtPath:filePath error:nil] fileSize];  
  }  
  return 0;  
}  

