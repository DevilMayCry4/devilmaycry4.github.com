---
layout: post
title: "C#delegate和event"
date: 2013-04-28 14:31
comments: true
categories: windowsphone
---
####C# Delegate概念
<p>
在基于Windows平台的程序设计中，事件（event）是一个很重要的概念。因为在几乎所有的Windows应用程序中，都会涉及大量的异步调 用，比如响应点击按钮、处理Windows系统消息等，这些异步调用都需要通过事件的方式来完成。即使在下一代开发平台——.NET中也不例外。
那么什么是事件呢？所谓事件，就是由某个对象发出的消息，这个消息标志着某个特定的行为发生了，或者某个特定的条件成立了。比如用户点击了鼠标、 socket上有数据到达等。那个触发（raise）事件的对象称为事件的发送者（event sender），捕获并响应事件的对象称为事件的接收者（event receiver）。
在这里，我们将要讨论的是，在.NET的主流开发语言C#中如何使用自定义的事件来实现我们自己的异步调用。
在C#中，事件的实现依赖于delegate，因此我们有必要先了解一下delegate的概念。
delegate是C#中的一种类型，它实际上是一个能够持有对某个方法的引用的类。与其它的类不同，delegate类能够拥有一个签名 （signature），并且它只能持有与它的签名相匹配的方法的引用。它所实现的功能与C/C++中的函数指针十分相似。它允许你传递一个类A的方法m 给另一个类B的对象，使得类B的对象能够调用这个方法m。但与函数指针相比，delegate有许多函数指针不具备的优点。首先，函数指针只能指向静态函 数，而delegate既可以引用静态函数，又可以引用非静态成员函数。在引用非静态成员函数时，delegate不但保存了对此函数入口指针的引用，而 且还保存了调用此函数的类实例的引用。其次，与函数指针相比，delegate是面向对象、类型安全、可靠的受控（managed）对象。也就是 说，runtime能够保证delegate指向一个有效的方法，你无须担心delegate会指向无效地址或者越界地址。
实现一个C# delegate是很简单的，通过以下3个步骤即可实现一个delegate：
1． 声明一个delegate对象，它应当与你想要传递的方法具有相同的参数和返回值类型。
2． 创建delegate对象，并将你想要传递的函数作为参数传入。
3． 在要实现异步调用的地方，通过上一步创建的对象来调用方法。
下面是一个简单的例子：

</p>
{% codeblock lang:csharp %}
using System;  
public class MyDelegateTest  
{  
// 步骤1，声明delegate对象  
public delegate void MyDelegate(string name);  
// 这是我们欲传递的方法，它与MyDelegate具有相同的参数和返回值类型  
public static void MyDelegateFunc(string name)  
{  
Console.WriteLine("Hello, {0}", name);  
}  
 
public static void Main()  
{  
// 步骤2，创建delegate对象  
MyDelegate md = new MyDelegate(MyDelegateTest.MyDelegateFunc);  
// 步骤3，调用delegate  
md("sam1111");  
}  
} 
{% endcodeblock %}

<p>
输出结果是：Hello, sam1111
了解了delegate，下面我们来看看，在C#中对event是如何处理的。
</p>
<!--more-->

####C# event

<p>
C#中的事件处理实际上是一种具有特殊签名的delegate，象下面这个样子：
public delegate void MyEventHandler(object sender, MyEventArgs e);
其中的两个参数，sender代表事件发送者，e是事件参数类。MyEventArgs类用来包含与事件相关的数据，所有的事件参数类都必须从 System.EventArgs类派生。当然，如果你的事件不含参数，那么可以直接用System.EventArgs类作为参数。
就是这么简单，结合delegate的实现，我们可以将自定义事件的实现归结为以下几步：
1． 定义delegate对象类型，它有两个参数，第一个参数是事件发送者对象，第二个参数是事件参数类对象。
2． 定义事件参数类，此类应当从System.EventArgs类派生。如果事件不带参数，这一步可以省略。
3． 定义事件处理方法，它应当与delegate对象具有相同的参数和返回值类型。
4． 用C# event关键字定义事件对象，它同时也是一个delegate对象。
5． 用+=操作符添加事件到事件队列中（-=操作符能够将事件从队列中删除）。
6． 在需要触发事件的地方用调用delegate的方式写事件触发方法。一般来说，此方法应为protected访问限制，既不能以public方式调用，但可以被子类继承。名字是OnEventName。
7． 在适当的地方调用事件触发方法触发事件。
下面是一个简单的例子：
</p>
{% codeblock lang:csharp %}
using System;  
public class EventTest  
{  
// 步骤1，定义delegate对象  
public delegate void MyEventHandler(object sender, System.EventArgs e);  
// 步骤2省略  
public class MyEventCls  
{  
// 步骤3，定义事件处理方法，它与delegate对象具有相同的参数和返回值类型  
public void MyEventFunc(object sender, System.EventArgs e)  
{  
Console.WriteLine("My event is ok!");  
}  
}  
// 步骤4，用event关键字定义事件对象  
private event MyEventHandler myevent;  
private MyEventCls myecls;  
public EventTest()  
{  
myecls = new MyEventCls();  
// 步骤5，用+=操作符将事件添加到队列中  
this.myevent += new MyEventHandler(myecls.MyEventFunc);  
}  
// 步骤6，以调用delegate的方式写事件触发函数  
protected void OnMyEvent(System.EventArgs e)  
{  
if(myevent != null)  
myevent(this, e);  
}  
 
public void RaiseEvent()  
{  
EventArgs e = new EventArgs();  
// 步骤7，触发事件  
OnMyEvent(e);  
}  
 
public static void Main()  
{  
EventTest et = new EventTest();  
Console.Write("Please input a:");  
string s = Console.ReadLine();  
if(s == "a")  
{  
et.RaiseEvent();  
}  
else 
{  
Console.WriteLine("Error");  
}  
}  
} 
{% endcodeblock %}
<p>
输出结果如下，黑体为用户的输入：
Please input ‘a’: a
My event is ok!
</p>

####小结
<p>
通过上面的讨论，我们大体上明白了C# delegate和C# event的概念，以及如何在C#中使用它们。我个人认为，delegate在C#中是一个相当重要的概念，合理运用的话，可以使一些相当复杂的问题变得 很简单。有时我甚至觉得，delegate甚至能够有指针的效果，除了不能直接访问物理地址。而且事件也是完全基于delegate来实现的。由于能力有 限，本文只是对delegate和event的应用作了一个浅显的讨论，并不深入，我希望本文能够起到抛砖引玉的作用。真正想要对这两个概念有更深入的了 解的话，还是推荐大家看MSDN。
</p>