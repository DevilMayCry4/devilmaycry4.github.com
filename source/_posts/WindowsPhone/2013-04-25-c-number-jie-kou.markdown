---
layout: post
title: "C# 接口"
date: 2013-04-25 15:11
comments: true
categories: windowsphone
---
<p>
定义接口，里面包含方法，但没有方法具体实现的代码，然后在继承该接口的类里面要实现接口的所有方法的代码。
</p>

<p>定义接口</p]>

{% codeblock lang:csharp %}
public interface IBark
{
    void Bark();
}
{% endcodeblock %}

<p>
再定义一个类,继承于IBark,并且必需实现其中的Bark()方法。
</p>

{% codeblock lang:csharp %}
public class Dog:IBark
{
    public Dog()
    {}
    public void Bark()
    {
       Consol.write("汪汪");
     }
}
{% endcodeblock %}