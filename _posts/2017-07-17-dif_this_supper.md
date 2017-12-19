---
layout: post
title: "this和super的区别"
date: 2017-07-20 
tag: Java 
---



<table cellspacing="0" cellpadding="0"><tbody><tr><td class="t_f" id="postmessage_177108">
<font size="5"><font size="4"> </font><br>
</font><table class="t_table" style="width:50%" cellspacing="0"><tbody><tr><td><div align="center">this</div></td><td><div align="center">super</div></td><td><div align="center">注意</div></td></tr><tr><td><div align="center">本类构造</div></td><td><div align="center">父类的构造方法</div></td><td><div align="center">this()与sup<br>
<br>
er（）不能同时存在</div></td></tr></tbody></table><br>
<font size="4">1.构造方法<br>
 this（）是调用一个类中的兄弟方法（一般为构造方法）<br>
  <br>
`public Student(itn sID)<br>`
`{<br>`
   `this();<br>`
   `this.sID.sID;<br>`
`}<br>`
`public Student(){<br>`
  System.out.println("无参数的构造方法");<br>
 `}</br>`

super（）调用 父类的构造方法<br>
&nbsp; &nbsp;<br>

`public Student(){<br>`

  `super(10);<br>`
  `System.out.println("子类的构造方法")<br>`
`}<br>`
2.属性<br>
 调用属性的顺序：先搜索本类中是否存在，不存在则去寻找父类<br>
<br>
 this.age   先搜索本类是否存在，不存在则去父类寻找<br>
 super.age   调用父类的age属性<br>
<br>
<br>

`public class A{<br>`

    `int x=0;<br>`
``}<br>``

`<br>`

`publib class B extends A{<br>`

    `public void setX(int x)<br>`

   {<br>

>        `this.x=x;//此时的x本类中不存在所以是父类的<br>`


>     }<br>

`}`

`public class A{<br>`
   int x=0;<br>
`}<br>`

`publib class B extends A{<br>`
   `public void setX(int x)<br>`
   {<br>
    this.x=x;//此时的x是本类的，想调用父类的x使用super.x<br>
   `}<br>`

`}<br>`
3.方法<br>
 <br>
<table class="t_table" style="width:50%" cellspacing="0"><tbody><tr><td> this.show()</td><td> super.show()</td></tr><tr><td> this.show()先搜索本类是否存在不存在则父类</td><td> 调用父类的show（）方法</td></tr></tbody></table><br>
`//重写父类的printA方法<br>`
`public void printA()<br>`
`{<br>`
  super.printA();//调用父类的<br>
  system.out.println("子类的")<br>
`}<br>`

> `<br>`

`运行结果：<br>`
  父类的<br>
  `子类的<br>`

<br>

