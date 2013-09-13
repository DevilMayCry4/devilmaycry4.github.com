---
layout: post
title: "Line With Arrow"
date: 2013-06-17 09:19
comments: true
categories: IOS
---
{% codeblock lang:objc %}
- (void)drawLine: (CGPoint) from to: (CGPoint) to singleArrow:(BOOL)single
{
    CGFloat radian = 0.5;
    CGFloat length = 5;
 
    
    int a = to.x > from.x?-1:1 ;
    CGFloat  x1 = to.x + a*length*cos(atan((to.y - from.y)/(to.x - from.x))- radian);
    CGFloat  y1 = to.y + a*length*sin(atan((to.y - from.y)/(to.x - from.x)) - radian);
    
   
    a = to.y > from.y? -1:1;
    CGFloat  x2 = to.x + a*length*sin(atan((to.x - from.x)/(to.y - from.y)) - radian);
    CGFloat  y2 = to.y + a*length*cos(atan((to.x - from.x)/(to.y - from.y)) - radian);
 
 
       
    if (!single)
    {

        a = to.y > from.y?  1:-1;
        CGFloat  x3 = from.x +  a*length*sin(atan((to.x - from.x)/(to.y - from.y)) - radian);
        CGFloat  y3 = from.y +  a*length*cos(atan((to.x - from.x)/(to.y - from.y)) - radian);
        [_currentStroke.path addLineToPoint:CGPointMake(x3,y3)];
        
         
         a = to.x > from.x?1:-1 ;
        CGFloat  x4 = from.x + a*length*cos(atan((to.y - from.y)/(to.x - from.x)) - radian);
        CGFloat  y4 = from.y + a*length*sin(atan((to.y - from.y)/(to.x - from.x)) - radian);
        
    }
}

{% endcodeblock %}
