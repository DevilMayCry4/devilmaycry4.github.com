---
layout: post
title: "压缩视频"
date: 2013-05-24 11:57
comments: true
categories: IOS
---

- (void) lowQuailtyWithInputURL:(NSURL*)inputURL
                                   outputURL:(NSURL*)outputURL
                                     blockHandler:(void (^)(AVAssetExportSession*))handler
{
    AVURLAsset *asset = [AVURLAsset URLAssetWithURL:inputURL options:nil];
    AVAssetExportSession *session = [[AVAssetExportSession alloc] initWithAsset:asset     presetName:AVAssetExportPresetMediumQuality];
    session.outputURL = outputURL;
    session.outputFileType = AVFileTypeQuickTimeMovie;
    [session exportAsynchronouslyWithCompletionHandler:^(void)
     {
         handler(session);
     }];
}




[self lowQuailtyWithInputURL:video outputURL:output blockHandler:^(AVAssetExportSession *session)
{
     if (session.status == AVAssetExportSessionStatusCompleted)
     {
         
     }
     else
     {
         
         
     }
}];



<p>
在block里面检测成功，失败，或者是取消，然后释放session.

期间可以通过不断的查看session的progress属性来获取转换的进度。

可以设置这些压缩质量

AVF_EXPORT NSString *const AVAssetExportPresetLowQuality        NS_AVAILABLE_IOS(4_0);
AVF_EXPORT NSString *const AVAssetExportPresetMediumQuality     NS_AVAILABLE_IOS(4_0);
AVF_EXPORT NSString *const AVAssetExportPresetHighestQuality    NS_AVAILABLE_IOS(4_0);
</p>