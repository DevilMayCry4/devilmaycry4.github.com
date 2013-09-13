---
layout: post
title: "IOS设备获取本机IP地址"
date: 2013-04-22 12:10
comments: true
categories: IOS
---
{% codeblock lang:objc %}
#include <ifaddrs.h>
#include <arpa/inet.h>
- (NSString*)getIPAddress
{
   NSString*address = @"error";
   structifaddrs*interfaces = NULL;
   structifaddrs*temp_addr = NULL;
   intsuccess = 0;
    
   // retrieve the current interfaces - returns 0 on success
   success = getifaddrs(&interfaces);
   if(success == 0)
     {
   // Loop through linked list of interfaces
        temp_addr = interfaces;
        while(temp_addr != NULL) 
        {
            if( temp_addr->ifa_addr->sa_family== AF_INET) 
            {
   // Check if interface is en0 which is the wifi connection on the iPhone
                 if([[NSStringstringWithUTF8String:temp_addr->ifa_name] isEqualToString:@"en0"])
                {
  // Get NSString from C String
                   address = [NSStringstringWithUTF8String:inet_ntoa(((structsockaddr_in*)temp_addr->ifa_addr)->sin_addr)];
                }
            }
            
            temp_addr = temp_addr->ifa_next;
        }
    }
    
    // Free memory    
    freeifaddrs(interfaces);
    returnaddress;
}


{% endcodeblock %}