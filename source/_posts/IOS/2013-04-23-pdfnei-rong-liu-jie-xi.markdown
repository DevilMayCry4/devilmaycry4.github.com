---
layout: post
title: "PDF内容流解析"
date: 2013-04-23 15:56
comments: true
categories: IOS
---

###PDF Document Parsing 

<p>Quartz提供了让你检查PDF文档结构和内容流的方法。通过这些方法，你可以获取文档目录的条目和与每个条目相关的内容。一个PDF的内容流（contentstream）正如其名字所暗示的—一个连续的数据流 例如'BT 12 /F71 Tf (draw thistext) Tj . . . '此处PDF操作符以及他们的描述符都混有实际的PDF内容。检查内容流,你需要按顺序访问它。</p>

###PDF操作符


<p>1). General graphics state(普通图形状态操作符)</p>
{% codeblock lang:c %}
 w: 设置线的宽度
 J: 设置线端点风格. Butt/Round/Projecting square 
 j: 设置线交叉风格. Miter/Round/Bevel
 M: 设置Miter Limit
 d: 设置虚线风格.
 ri: 设置Rendering Intent(呈色意向)
 i: 设置平面化容忍度.
 gs: 设置图形状态参数.


<p>2). Special graphics state(特殊图形状态操作符)</p>
{% codeblock lang:c %}
 q: 保存当前图形状态
 Q：回复图形状态.
 cm：设置当前装换矩阵.


<p>3). Path construction(路径构建操作符)</p>
{% codeblock lang:c %}
 m: 移动当前指针到指定位置.
 l: 添加一条连接当前指针到指定位置的线段.
 c: 添加一条Bezier曲线， 有2个控制点，2个端点.
 v: 添加一条Bezier曲线， 2个控制点重合.
 y: 添加一条Bezier曲线， 第二个控制点和第二个端点重合.
 h: 闭合路径
 re: 添加一个矩形.

<!--more-->

<p>4). Path painting(路径绘制操作符)</p>
{% codeblock lang:c %}
 S: 描绘路径.
 s: 闭合路径并描绘路径.
 f: 填充路径，使用非零回转数规则确定区域，路径在填充之前闭合.
 F: 等同f，为了兼容.
 f*: 填充路径，使用奇偶规则确定区域.
 B: 填充路径，使用非零回转数规则确定区域， 并描绘路径.
 B*: 填充路径，使用奇偶规则确定区域， 并描绘路径.
 b: 闭合路径, 填充路径，使用非零回转数规则确定区域， 并描绘路径.
 b*: 闭合路径，使用奇偶规则确定区域， 并描绘路径.
 n: 结束路径，不做任何描绘和填充.



<p>5). Clipping paths(路径修剪操作符)</p>
{% codeblock lang:c %}
 W: 将当前修剪区域和当前路径做交，使用非零回转数规则.
 W*: 将当前修剪区域和当前路径做交，使用奇偶规则.



<p>6). Text  objects(文本对象操作符)</p>
{% codeblock lang:c %}
 BT: 开始一个文本对象.
 ET: 结束一个文本对象.



<p>7). Text  state(文本状态操作符)</p>
{% codeblock lang:c %}
 Tc: 设置字符间隔.
 Tw: 设置单词间隔.
 Tz: 设置水平缩放.
 TL: 设置Leading.
 Tf: 设置文本字体.
 Tr: 设置Render(渲染)模式.
 Ts: 设置Rise



<p>8). Text  positioning(文本位置操作符)</p>
{% codeblock lang:c %}
 Td: 移动到下一行的开始，通过偏移(tx,ty).
 TD: 移动到下一行的开始，通过偏移(tx,ty). 同时设置Leading为-ty.
 Tm: 设置文本矩阵和文本线矩阵.
 T*: 移动到下一行的开始位置. 和0 Tl Td相同.



<p>9). Text  showing(文本显示操作符)</p>
{% codeblock lang:c %}
 Tj: 显示一个文本字符串.
 TJ: 显示一个或者多个文本字符串，允许独立的制定各个字型的位置.
 ': 移动到下一行并显示一个文本字符串.
 ": 移动到下一行并显示一个文本字符串. 并指定字符间距为ac, 单词间距为aw.



<p>10). Type3 fonts(type3字体操作符)</p>
{% codeblock lang:c %}
 d0:  设置字型的宽度.
 d1: 设置字型的宽度及自行的bounding box(边界矩形).



<p>11). Color(颜色操作符)</p>
{% codeblock lang:c %}
 CS: 设置描绘颜色空间.
 cs: 设置非描绘颜色空间.
 SC: 设置描绘颜色值，针对一般颜色空间.
 SCN: 设置描绘颜色值，允许特殊颜色空间.
 sc: 设置非描绘颜色值，针对一般颜色空间.
 scn: 设置非描绘颜色值，允许特殊颜色空间.
 G: 设置描绘颜色空间为DeviceGray，并设置颜色值.
 g: 设置非描绘颜色空间为DeviceGray, 并设置颜色值.
 RG: 设置描绘颜色空间为DeviceRGB，并设置颜色值.
 rg: 设置非描绘样色空间为DeviceRGB，并设置颜色值.
 K: 设置描绘颜色空间为DeviceCMYK，并设置颜色值.
 k: 设置非描绘颜色空间为DeviceCMYK，并设置颜色值.


<p>12). Shading patterns(渐变样式操作符)</p>
{% codeblock lang:c %}
 sh: 输出一个shading对象.


<p>13). Inline images(内联图像操作符)</p>
{% codeblock lang:c %}
 BI: 开始一个内联图像.
 ID: 开始内联图像数据.
 EI: 结束一个内敛图像.


<p>14). XObjects(外部对象操作符)</p>
{% codeblock lang:c %}
 Do: 输出一个外部对象.


<p>15). Marked content(标记内容操作符)</p>
{% codeblock lang:c %}
 MP: 定义一个标记内容点.
 DP: 定义一个带属性列表的标记内容点.
 BMC: 开始一个标记内容序列.
 BDC: 开始一个带属性列表的标记内容序列.
 EMC: 结束一个标记内容序列.



<p>16). Compatibility(兼容性操作符)</p>
{% codeblock lang:c %}
 BX: 开始一个兼容段.
 EX: 结束一个兼容段.
