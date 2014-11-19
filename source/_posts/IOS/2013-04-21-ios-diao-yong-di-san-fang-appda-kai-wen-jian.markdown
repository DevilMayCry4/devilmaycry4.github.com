---
layout: post
title: "IOS 调用第三方App打开文件"
date: 2013-04-21 10:22
comments: true
categories: IOS
---
{% blockquote lang:objc %}
NSString  *filePath =[[NSBundlemainBundle]pathForResource:@"1"ofType:@"docx"];
UIDocumentInteractionController  *documentController=[UIDocumentInteractionControllerinteractionControllerWithURL:[NSURLfileURLWithPath:filePath]];
documentController.UTI= @"com.microsoft.word.doc";
[documentController presentOpenInMenuFromRect:CGRectZero
                    		    	 inView:self.view
                                     animated:YES];
{% blockquote %}

<p><a href="https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html#//apple_ref/doc/uid/TP40009259-SW1">UTI 列表</a></p>