---
layout: post
title: "google 地图的路线获取和解析"
date: 2013-04-21 20:18
comments: true
categories: IOS
---


<p>头文件和宏定义</p>


#import <MapKit/MapKit.h>
#import "RegexKitLite.h"
#import "JSONKit.h"

#define Mode_Walk    @"walking"
#define Mode_Drive   @"driving"
#define Mode_Bycle   @"bicycling"
#define Mode_Transit @"transit"

<p>获取两个点之间的路线</p>
- (NSArray *)calculateRoutesFrom:(CLLocationCoordinate2D)f to:(CLLocationCoordinate2D)t mode:(NSString *)mode
{
    NSString    *saddr = [NSString stringWithFormat:@"%f,%f", f.latitude, f.longitude];
    NSString    *daddr = [NSString stringWithFormat:@"%f,%f", t.latitude, t.longitude];

    NSString    *apiUrlStr = [NSString stringWithFormat:@"http://maps.google.com/maps/api/directions/json?origin=%@&destination=%@&sensor=false&mode=%@", saddr, daddr,mode];
   
    NSURL       *apiUrl = [NSURL URLWithString:apiUrlStr];
    NSString    *apiResponse = [NSString stringWithContentsOfURL:apiUrl encoding:NSASCIIStringEncoding error:nil];
    NSDictionary *dictionaryFromJson = [apiResponse  objectFromJSONString];
    if([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"OK"])
    {
        NSDictionary  *dict = [[[[dictionaryFromJson objectForKey:@"routes"] objectAtIndex:0] objectForKey:@"legs"] objectAtIndex:0];
        NSArray *legArray = [dict objectForKey:@"steps"];
        
        NSMutableArray *stepArray = [NSMutableArray array];
        NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
        for (id step in legArray)
        {
            id start = [step objectForKey:@"start_location"];
            
            CLLocation  *loc0 = [[[CLLocation alloc] initWithLatitude:[[start objectForKey:@"lat"] doubleValue]
                                                            longitude:[[start objectForKey:@"lng"] doubleValue]] autorelease];
            
            [stepArray addObject:loc0];
            
            NSString *polyline = [[step objectForKey:@"polyline"] objectForKey:@"points"];
            NSArray *polylineArrary = [self decodePolyLine:[[polyline mutableCopy] autorelease]];
            [stepArray addObjectsFromArray:polylineArrary];
            
            id end = [step objectForKey:@"end_location"];
            
            CLLocation  *loc1 = [[[CLLocation alloc] initWithLatitude:[[end objectForKey:@"lat"] floatValue]
                                                            longitude:[[end objectForKey:@"lng"] floatValue]] autorelease];
            [stepArray addObject:loc1];
            
            
        }
        [pool drain];
        return stepArray;
  
    }
    else
    {
        NSString *tipString = nil;
        if ([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"NOT_FOUND"])
        {
             tipString = @"请求的起点、终点或路标中指定的至少一个位置无法进行地理编码";
        }
        else if ([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"ZERO_RESULTS"])
        {
            tipString = @"起点和终点之间找不到路线";
        }
        else if ([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"MAX_WAYPOINTS_EXCEEDED"])
        {
            tipString = @"请求中提供的 waypoints 过多";
        }
        else if ([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"INVALID_REQUEST"])
        {
            tipString = @"提供的请求无效";
        }
        else if([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"OVER_QUERY_LIMIT"])
        {
            tipString = @"该服务在允许的时间段内从您的应用收到的请求过多";
        }
        else if([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"REQUEST_DENIED"])
        {
             tipString = @"该服务已拒绝您的应用使用路线服务";
        }
        else if([[dictionaryFromJson objectForKey:@"status"] isEqualToString:@"OVER_QUERY_LIMIT"])
        {
            tipString = @"路线请求因服务器出错而无法得到处理。如果您重试一次，该请求可能就会成功";
            
        }
        dispatch_async(dispatch_get_main_queue(), ^{
            UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"提示"
                                                                message:tipString
                                                               delegate:nil
                                                      cancelButtonTitle:@"OK"
                                                      otherButtonTitles:nil, nil];
            [alertView show];
            [alertView release];
        });
       
        
        return nil;
    }

}
<!--more-->
<p>解析两个点之间路线点的函数:</p>
- (NSMutableArray *)decodePolyLine:(NSMutableString *)encoded
{
    [encoded replaceOccurrencesOfString:@"\\\\" withString:@"\\"
    options :NSLiteralSearch
    range   :NSMakeRange(0, [encoded length])];
    NSInteger       len = [encoded length];
    NSInteger       index = 0;
    NSMutableArray  *array = [[[NSMutableArray alloc] init] autorelease];
    NSInteger       lat = 0;
    NSInteger       lng = 0;
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
    while (index < len)
    {
        NSInteger   b;
        NSInteger   shift = 0;
        NSInteger   result = 0;
        do
        {
            b = [encoded characterAtIndex:index++] - 63;
            result |= (b & 0x1f) << shift;
            shift += 5;
        }
        while (b >= 0x20);
        NSInteger dlat = ((result & 1) ? ~(result >> 1) : (result >> 1));
        lat += dlat;
        shift = 0;
        result = 0;
        do
        {
            b = [encoded characterAtIndex:index++] - 63;
            result |= (b & 0x1f) << shift;
            shift += 5;
        }
        while (b >= 0x20);
        NSInteger dlng = ((result & 1) ? ~(result >> 1) : (result >> 1));
        lng += dlng;
        NSNumber    *latitude = [[[NSNumber alloc] initWithFloat:lat * 1e-5] autorelease];
        NSNumber    *longitude = [[[NSNumber alloc] initWithFloat:lng * 1e-5] autorelease];
        CLLocation  *loc = [[[CLLocation alloc] initWithLatitude:[latitude floatValue] longitude:[longitude floatValue]] autorelease];
        [array addObject:loc];
    }
    [pool drain];
    return array;
}

<p>绘制函数:</p>
- (void)drawFrom:(CLLocationCoordinate2D)from to:(CLLocationCoordinate2D)destination mode:(NSString *)mode
{
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        NSArray *array =  [self calculateRoutesFrom:from to:destination mode:mode];
        dispatch_async(dispatch_get_main_queue(), ^{
            [self drawLineWithLocationArray:array];
        });
    });
}
</code></pre></p>

<p><pre><code>- (void)drawLineWithLocationArray:(NSArray *)locationArray
{
    int                     pointCount = [locationArray count];
    CLLocationCoordinate2D  *coordinateArray = (CLLocationCoordinate2D *)malloc(pointCount * sizeof(CLLocationCoordinate2D));

    for (int i = 0; i < pointCount; ++i)
    {
        CLLocation *location = [locationArray objectAtIndex:i];
        coordinateArray[i] = [location coordinate];
    }

    self.routeLine = [MKPolyline polylineWithCoordinates:coordinateArray count:pointCount];
    [self.googleView setVisibleMapRect:[self.routeLine boundingMapRect]];
    [self.googleView addOverlay:self.routeLine];

    free(coordinateArray);
    coordinateArray = NULL;
}

<p>调用示例:

- (void)drawTestLine
{
    CLLocationCoordinate2D  cll0 = CLLocationCoordinate2DMake(23.132901, 113.357223);
    CLLocationCoordinate2D  cll1 = CLLocationCoordinate2DMake(23.051757, 113.392832);
    [self drawFrom:cll0 to:cll1 mode:Mode_Drive];
}
<p>参考资料</p>
<p><a href="https://developers.google.com/maps/documentation/directions/?hl=zh-CN#Limits">谷歌说明文档</a></p>
<p><a href="https://developers.google.com/maps/documentation/utilities/polylinealgorithm?hl=zh-CN">路线解码算法</a></p>
