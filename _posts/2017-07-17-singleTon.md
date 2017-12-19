---
layout: post
title: "设计模式——单例模式"
date: 2017-07-22
tag: Java 
---



### 设计模式——单例模式

<br>
单例模式：代码简单，包含了面象对象的所有知识点-------》面试常考<br>
&nbsp; &nbsp;饿汉式：<br>
&nbsp; &nbsp;&nbsp; &nbsp;<br>
<div class="blockcode"><div id="code_L4H"><ol><li>public class Singleton{<br>
</li><li><br>
</li><li>&nbsp; &nbsp; private Singleton(){<br>
</li><li>&nbsp; &nbsp;&nbsp;&nbsp;private static final Singleton singleObject=new Singleton();<br>
</li><li>&nbsp; &nbsp; public static Singleton getInstance(){//getInstance一般是获取单例模式的对象<br>
</li><li>&nbsp; &nbsp;&nbsp; &nbsp;return singleObject;<br>
</li><li>&nbsp; &nbsp; }<br>
</li><li><br>
</li><li>&nbsp; &nbsp;}<br>
</li><li>}</li></ol></div><em onclick="copycode($('code_L4H'));">复制代码</em></div>&nbsp;&nbsp;优点：线程安全<br>
&nbsp; &nbsp;缺点：如果长期不使用，就会一直占用内存空间<br>
<br>
<br>
&nbsp; &nbsp;懒汉式：<br>
&nbsp; &nbsp;<br>
&nbsp; &nbsp;&nbsp;&nbsp;<br>
<div class="blockcode"><div id="code_Q3Z"><ol><li>public class Singleton{<br>
</li><li>&nbsp; &nbsp;//1.私有化构造方法<br>
</li><li>&nbsp; &nbsp; private Singleton(){}<br>
</li><li>&nbsp; &nbsp;//声明变量<br>
</li><li>&nbsp; &nbsp; private static Singleton singleObject;<br>
</li><li>&nbsp;&nbsp;//定义公有的方法返回变量<br>
</li><li>&nbsp; &nbsp;public static Singleton getInstance(){//getInstance一般是获取单例模式的对象<br>
</li><li>&nbsp; &nbsp;&nbsp;&nbsp;if(singleObject==null){<br>
</li><li>&nbsp; &nbsp;&nbsp; &nbsp; singleObject=new Singleton();<br>
</li><li>&nbsp; &nbsp;&nbsp;&nbsp;}<br>
</li><li>&nbsp; &nbsp;&nbsp;&nbsp;return singleObject;<br>
</li><li>&nbsp; &nbsp;}<br>
</li><li>}</li></ol></div><em onclick="copycode($('code_Q3Z'));">复制代码</em></div>&nbsp;&nbsp;优点：不会过早的new空间<br>
&nbsp; &nbsp;缺点：多线程使用单例，会出现线程不安全问题<br>
<br>

