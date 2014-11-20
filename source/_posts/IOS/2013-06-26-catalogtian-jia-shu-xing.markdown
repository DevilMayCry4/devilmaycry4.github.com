---
layout: post
title: "Catalog添加属性"
date: 2013-06-26 10:50
comments: true
categories: IOS
---
<p>
Category是Objective-C中常用的语法特性，通过它可以很方便的为已有的类来添加函数。但是Category不允许为已有的类添加新的属性或者成员变量。    
一种常见的办法是通过runtime.h中objc_getAssociatedObject / objc_setAssociatedObject来访问和生成关联对象。通过这种方法来模拟生成属性。
</p>

//NSObject+IndieBandName.h
@interface NSObject (IndieBandName)
@property (nonatomic, strong) NSString *indieBandName;
@end

// NSObject+IndieBandName.m   
 #import "NSObject+Extension.h"
 #import <objc/runtime.h>
static const void *IndieBandNameKey = &IndieBandNameKey;   
@implementation NSObject (IndieBandName)
@dynamic indieBandName;

- (NSString *)indieBandName {
    return objc_getAssociatedObject(self, IndieBandNameKey);
}

- (void)setIndieBandName:(NSString *)indieBandName{
    objc_setAssociatedObject(self, IndieBandNameKey, indieBandName, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

@end DLIntrospection




<p>
这个和Category无关，但是也是runtime.h的一种应用。DLIntrospection，是 一个NSObject Category。它为NSObject提供了一系列扩展函数： 
</p>


@interface NSObject (DLIntrospection)

+ (NSArray *)classes;
+ (NSArray *)properties;
+ (NSArray *)instanceVariables;
+ (NSArray *)classMethods;
+ (NSArray *)instanceMethods;

+ (NSArray *)protocols;
+ (NSDictionary *)descriptionForProtocol:(Protocol *)proto;


+ (NSString *)parentClassHierarchy;
@end


<p>
通过这些函数，你可以在调试时（通过po命令）或者运行时获得对象的各种信息。
</p>