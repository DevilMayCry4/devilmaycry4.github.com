---
layout: post
title: "C#abstrace跟virtua的区别"
date: 2013-04-26 09:30
comments: true
categories: windowsphone
---
#####一、Virtual方法（虚方法）

<p>
virtual 关键字用于在基类中修饰方法。virtual的使用会有两种情况：

     情况1：在基类中定义了virtual方法，但在派生类中没有重写该虚方法。那么在对派生类实例的调用中，该虚方法使用的是基类定义的方法。

     情况2：在基类中定义了virtual方法，然后在派生类中使用override重写该方法。那么在对派生类实例的调用中，该虚方法使用的是派生重写的方法。
</p>
#####二、Abstract方法（抽象方法）
<p>
 abstract关键字只能用在抽象类中修饰方法，并且没有具体的实现。抽象方法的实现必须在派生类中使用override关键字来实现。

接口和抽象类:

最本质的区别：抽象类是一个不完全的类，是对对象的抽象，而接口是一种行为规范。

 C# 是面向对象的程序设计语言，每一个函数都属于一个类。
Static：当一个方法被声明为Static时，这个方法是一个静态方法，编译器会在编译时保留这个方法的实现。也就是说，这个方法属于类，但是不属于任何成员，不管这个类的实例是否存在，它们都会存在。就像入口函数Static void Main，因为它是静态函数，所以可以直接被调用。</p>
<p>
Virtua：当一个方法被声明为Virtual时，它是一个虚拟方法，直到你使用ClassName variable = new ClassName();声明一个类的实例之前，它都不存在于真实的内存空间中。这个关键字在类的继承中非常常用，用来提供类方法的多态性支持。
overrride：表示重写 这个类是继承于Shape类
public override double Area 这个属性再shape中肯定存在 但是这里我们不想用shape中的 所以要重写
virtual，abstract是告诉其它想继承于他的类 你可以重写我的这个方法或属性，否则不允许。
一个生动的例子 :老爸表示基类（被继承的类） 儿子表示子类（继承的类）
老爸用virtual告诉儿子:"孩子,你要继承我的事业,在这块上面可以自己继续发展你自己的"
儿子用override告诉全世界:"这个我可不是直接拿我爸的,他只是指个路给我,是我自己奋斗出来的"
</p>
<p>
abstract：抽象方法声明使用，是必须被派生类覆写的方法，抽象类就是用来被继承的；可以看成是没有实现体的虚方法；如果类中包含抽象方法，那么类就必须定义为抽象类，不论是否还包含其他一般方法；抽象类不能有实体的。
实例解答:
interface:用来声明接口
</p>

<p>
1.只提供一些方法规约，不提供方法主体. 如:
</p>

{% codeblock lang:csharp %}
public interface IPerson
{
void getName();//不包含方法主体
}
{% endcodeblock %}

<p>
2.方法不能用public abstract等修饰,无字段变量，无构造函数。
</p>
 

<p>
3.方法可包含参数。 如:
</p>

{% codeblock lang:csharp %}
 public interface IPerson
{
void getAge(string s);
}
{% endcodeblock %}


<p>
一个例子(例1)：
</p>

{% codeblock lang:csharp %}
public interface IPerson
{
IPerson(); //错误
string name; //错误
public void getIDcard();//错误
void getName(); //right
void getAge(string s); //right
}
{% endcodeblock %}


<p>
实现interface的类
1.与继承类的格式一致，如 public class Chinese:IPerson{}
2.必须实现 interface 中的各个方法
例2，继承例1
</p>


{% codeblock lang:csharp %}
public class Chinese:IPerson
{
public Chinese(){} //添加构造
public void getName(){} //实现getName()
public void getAge(string s){} //实现getAge()
}
{% endcodeblock %}
<!--more-->

<p>
abstract:声明抽象类、抽象方法
</p>
<p>
1.抽象方法所在类必须为抽象类
</p>
<p>
2.抽象类不能直接实例化，必须由其派生类实现。
</p>
<p>
3.抽象方法不包含方法主体，必须由派生类以override方式实现此方法,这点跟interface中的方法类似
如
</p>


{% codeblock lang:csharp %}
public abstract class Book
{
public Book()
{
}
public abstract void getPrice(); //抽象方法，不含主体
public virtual void getName() //虚方法，可覆盖
{
Console.WriteLine("this is a test:virtual getName()");
}
public virtual void getContent() //虚方法，可覆盖
{
Console.WriteLine("this is a test:virtual getContent()");
}
public void getDate() //一般方法，若在派生类中重写，须使用new关键字
{
Console.WriteLine("this is a test: void getDate()");
}
}
public class JavaBook:Book
{
public override void getPrice() //实现抽象方法，必须实现
{
Console.WriteLine("this is a test:JavaBook override abstract getPrice()");
}
public override void getName() //覆盖原方法，不是必须的
{
Console.WriteLine("this is a test:JavaBook override virtual getName()");
}
}
{% endcodeblock %}


<p>
测试如下：
</p>

{% codeblock lang:csharp %}
public class test
{
public test()
{
JavaBook jbook=new JavaBook();
jbook.getPrice(); //将调用JavaBook中getPrice()
jbook.getName(); //将调用JavaBook中getName()
jbook.getContent(); //将调用Book中getContent()
jbook.getDate(); //将调用Book中getDate()
}
public static void Main()
{
test t=new test();
}
}
{% endcodeblock %}


<p>
virtual:标记方法为虚方法
1.可在派生类中以override覆盖此方法
2.不覆盖也可由对象调用
3.无此标记的方法(也无其他标记)，重写时需用new隐藏原方法
abstract 与virtual : 方法重写时都使用 override 关键字
接口定义以大写字母I开头。方法只定义其名称,在C#中，方法默认是公有方法；用public修饰方法是不允许的,否则会出现编译错误；接口可以从别的接口继承，如果是继承多个接口，则父接口列表用逗号间隔。
接口可以通过类来实现，当类的基列表同时包含基类和接口时，列表中首先出现的是基类；类必须要实现其抽象方法；
接口使用：见代码（转）
interface使用
interface使用（实例一）
</p>


{% codeblock lang:csharp %}
using System;
namespace Dage.Interface
{
//打印机接口
public interface IPrint
{
string returnPrintName();
}
}
//--------------------------------------------
using System;
using Dage.Interface;
namespace Dage.Print
{
//HP牌打印机类
public class HP: IPrint
{
public string returnPrintName()
{
return "这是HP牌打印机";
}
}
}
//--------------------------------------------
using System;
namespace Dage.Print
{
//Eps牌打印机类
public class Eps: IPrint
{
public string returnPrintName()
{
return "这是Eps牌打印机";
}
}
}
//--------------------------------------------
using System;
using Dage.Interface;
namespace Dage
{
//打印类
public class Printer
{
public Printer()
{}
public string PrintName(IPrint iPrint)
{
return iPrint.returnPrintName();
}
}
}
//--------------------------------------------
--WinFrom中调用代码:
private void button1_Click(object sender, System.EventArgs e)
{
Printer p= new Printer();
switch (this.comboBox1.Text)
{
case "HP":
MessageBox.Show(p.PrintName(new HP()));
break;
case "Eps":
MessageBox.Show(p.PrintName(new Eps()));
break;
default:
MessageBox.Show("没有发现这个品牌！");
break;
}
}
{% endcodeblock %}