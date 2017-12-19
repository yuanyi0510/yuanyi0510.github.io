---
layout: post
title: "封装"
date: 2017-07-18  
tag: Java 
---



### 封装

封装：顾名思义，隐藏对象的属性和实现细节，仅对外公开接口，控制在程序中属性的读和修改的访问级别；将抽象得到的数据和行为（或功能）相结合，形成一个有机的整体，也就是将数据与操作数据的源代码进行有机的结合，形成“类”，其中数据和函数都是类的成员。<br>
<br>
封装的目的是增强安全性和简化编程，使用者不必了解具体的实现细节，而只是要通过 外部接口，一特定的访问权限来使用类的成员。 <br>
<br>
封装的大致原则:<br>
1、把尽可能多的东西藏起来.对外提供简捷的接口. <br>
2、把所有的属性藏起来. <br>
3、封装好处：将变化隔离；便于使用；提高重用性；安全性。<br>
封装的意义：<br>
　　封装的意义在于保护或者防止代码（数据）被我们无意中破坏。在面向对象程序设计中数据被看作是一个中心的元素并且和使用它的函数结合的很密切，从而保护它不被其它的函数意外的修改。<br>
<br>

1. 保护数据成员，不让类以外的程序直接访问或修改，只能通过提供的公共的接口访问==&gt;数据封装。<br>
   <br>
2. 方法的细节对用户是隐藏的，只要接口不变，内容的修改不会影响到外部的调用者==&gt;方法封装。 <br>
   <br>
3. 当对象含有完整的属性和与之对应的方法时称为封装。<br>
   <br>
4. 从对象外面不能直接访问对象的属性，只能通过和该属性对应的方法访问。<br>
   <br>
5. 对象的方法可以接收对象外面的消息。<br>
   <br>
   类成员的访问修饰符：<br>
   &nbsp; &nbsp;即类的方法和成员变量的访问控制符，一个类作为整体对象不可见，并不代表他的所有域和方法也对程序其他部分不可见，需要有他们的访问修饰符判断。<br>
   <br>
   权限如下：<br>

<ignore_js_op>

<img id="aimg_72506" aid="72506" src="forum.php?mod=attachment&amp;aid=NzI1MDZ8ZjZhNTEyZTZ8MTUxMzY0ODMwOXw3MDA1fDEyNjQyMg%3D%3D&amp;noupdate=yes" zoomfile="forum.php?mod=attachment&amp;aid=NzI1MDZ8ZjZhNTEyZTZ8MTUxMzY0ODMwOXw3MDA1fDEyNjQyMg%3D%3D&amp;noupdate=yes&amp;nothumb=yes" file="forum.php?mod=attachment&amp;aid=NzI1MDZ8ZjZhNTEyZTZ8MTUxMzY0ODMwOXw3MDA1fDEyNjQyMg%3D%3D&amp;noupdate=yes" class="zoom" onclick="zoom(this, this.src, 0, 0, 0)" inpost="1" onmouseover="showMenu({'ctrlid':this.id,'pos':'12'})" data-bd-imgshare-binded="1" initialized="true" width="481">

<div class="tip tip_4 aimg_tip" id="aimg_72506_menu" style="position: absolute; z-index: 301; left: 214.5px; top: 952.5px; display: none;" disautofocus="true" initialized="true">
<div class="xs0">
<p><strong>QQ截图20170728082629.png</strong> <em class="xg1">(31.29 KB, 下载次数: 0)</em></p>
<p>
<a href="forum.php?mod=attachment&amp;aid=NzI1MDZ8ZjZhNTEyZTZ8MTUxMzY0ODMwOXw3MDA1fDEyNjQyMg%3D%3D&amp;nothumb=yes" target="_blank">下载附件</a>

</p>

<p class="xg1 y">2017-7-28 08:30 上传</p>

</div>
<div class="tip_horn"></div>
</div>

</ignore_js_op>
<br>
总结：<br>
 public 修饰的可以在同包或者不同包中被访问<br>
 protected 同包或者不同包的子类可以被访问<br>
 默认 只能在自己的包中被访问，跨包则无法访问<br>
 private 只能在本类中被访问<br>
<br>
<br>
