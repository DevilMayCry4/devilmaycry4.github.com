---
layout: post
title: "int型和NSdata型互相转化"
date: 2013-04-22 13:42
comments: true
categories: IOS
---
<p>int转化成NSdata</p>

int i =1;
NSData *data =[NSDatadataWithBytes:&i length:sizeof(i)];


<p>NSdata转化成int</p>


int i = [data getBytes:&i length:sizeof(i)];

