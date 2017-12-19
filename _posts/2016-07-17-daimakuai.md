---
layout: post
title: "代码块介绍"
date: 2017-07-17   
tag: Java 
---

### 代码块的种类：静态代码块、构造代码块、普通代码块<br>

<br>
<font style="background-color:Yellow">执行顺序：静态代码块---》构造代码块---》构造方法-----》普通方法-----》普通代码块</font><br>
<br>
静态代码块：<br>
   功能：一般完成静态成员变量的初始化。<br>
&nbsp; &nbsp;特点：1.一般不能使用非静态的变量<br>
&nbsp; &nbsp;&nbsp; &nbsp;2.静态的代码块只会加载一次，执行时机只有创建第一个对象的时候才会被执行，而且仅执行一次<br>
&nbsp; &nbsp;&nbsp; &nbsp; 如果不创建对象也不会执行<br>
&nbsp; &nbsp;使用场景：如果想实现对静态成员变量的初始化赋值，可以通过静态代码块实现<br>
<br>
构造代码块：<br>
&nbsp; &nbsp;特点：1.在构造方法之前执行<br>
&nbsp; &nbsp;&nbsp; &nbsp;2.直接定义在类体中的代码块<br>
&nbsp; &nbsp;&nbsp; &nbsp;3.每创建一个对象都会被执行一次<br>
<br>
<br>
方法代码块：<br>
&nbsp; &nbsp;特点：1.方法中的代码块<br>
&nbsp; &nbsp;&nbsp; &nbsp;2.只有调用方法的时候才会被执行的代码块<br>
&nbsp; &nbsp;&nbsp; &nbsp;<font style="background-color:Yellow">3.代码块中声明的变量外界无法访问</font><br>
<br>