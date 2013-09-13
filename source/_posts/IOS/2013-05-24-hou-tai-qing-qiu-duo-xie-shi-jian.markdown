---
layout: post
title: "后台请求多些时间"
date: 2013-05-24 15:15
comments: true
categories: IOS
---
{% codeblock lang:objc %}
   UIApplication *app = [UIApplication sharedApplication];
    
    //一个后台任务标识符
    UIBackgroundTaskIdentifier taskID;
    taskID = [app beginBackgroundTaskWithExpirationHandler:^{
        //如果系统觉得我们还是运行了太久，将执行这个程序块，并停止运行应用程序
        [app endBackgroundTask:taskID];
    }];
    //UIBackgroundTaskInvalid表示系统没有为我们提供额外的时候
    if (taskID == UIBackgroundTaskInvalid) {
        NSLog(@"Failed to start background task!");
        return;
    }
    while (app.backgroundTimeRemaining >0.0)
    {
        
    }
    NSLog(@"Starting background task with %f seconds remaining", app.backgroundTimeRemaining);
    [NSThread sleepForTimeInterval:10];
    NSLog(@"Finishing background task with %f seconds remaining",app.backgroundTimeRemaining);
    //告诉系统我们完成了
    [app endBackgroundTask:taskID];
{% endcodeblock %}
