---
layout: post
title: "重载和重写"
date: 2017-07-18  
tag: Java 
---



### 重载

##### 重载：让类以统一的方式处理不同的数据类型的一种手段。是一个类中多态性的一种表现。也叫overload<br>

   特点：<br>

     1. 方法名一样，参数列表不同 <br>
        &nbsp; &nbsp;&nbsp;&nbsp;2.不能通过访问权限、返回类型、抛出的异常进行重载<br>
        &nbsp; &nbsp;&nbsp;&nbsp;3.方法的异常类型和数目不会对重载造成影响<br>
        &nbsp; &nbsp;场景：<br>
             一个方法有不同的参数类型<br>

### 重写

#####  重写：父类方法在子类中的实现。也叫覆盖override<br>

   特点：<br>
&nbsp; &nbsp; 1.发生在子类中<br>
&nbsp; &nbsp; 2.方法的定义与父类完全一致<br>
&nbsp; &nbsp; 3.改变方法体<br>
&nbsp; &nbsp; 4.大于等于父类访问修饰符&nbsp;&nbsp;public ---》子类：public <br>
&nbsp; &nbsp;&nbsp;&nbsp;大 ：public protected&nbsp;&nbsp;默认的&nbsp; &nbsp;private&nbsp;&nbsp;小<br>
&nbsp; &nbsp; 5.静态方法不能被重写为非静态方法<br>
    场景；<br>
&nbsp; &nbsp;&nbsp;&nbsp;子类继承的父类的方法不能满足当前需求。<br>
