---
layout: post
title: "面向对象常见概念的区别"
date: 2017-07-20 
tag: JavaSE
---

[TOC]

### 类和对象得区别

类是对一类事物得抽象描述，而对象用于表示现实中该类事物的个体。

![1]()

从上图可以看出，可以将玩具模型看作一个类，将一个个的玩具看成对象。类用于描述多个对象的共同特征，是对象的模板。对象用于描述现实世界中的个体，它是类的实例。

### 局部变量和成员变量的区别

1. 定义的位置不同
   - 定义在类中的变量是成员变量
   - 定义在方法中或者代码块中的变量是局部变量
2. 在内存中的位置不同
   - 成员变量存储在内存的对象中
   - 局部变量存储在栈内存的方法中

3. 声明周期不同
   - 成员变量随着对象的出现而出现在堆中，随着对象的消失从堆中消失
   - 局部变量随着方法的运行出现在栈中，随着方法的消失而弹栈消失
4. 初始化不同
   - 成员变量在堆中，所以有默认的初始化值
   - 局部变量没有默认的初始化值，必须手动给其赋值才可以使用


### 基本类型和引用类型作为参数传递

​	基本类型作为参数传递时，其实就是将基本类型的变量复制了一份传递给方法，所以并不会影响其值的变化；引用类型作为传递参数时，是将引用变量空间中的内存地址复制了一份传递给方法，这时会有两个引用指向堆中的同一个对象，无论哪个引用改变了所指对象的值，原来的结果都会改变

###  this和super的区别

<table class="t_table" style="width:50%" cellspacing="0"><tbody><tr><td><div align="left">this</div></td><td><div align="center">super</div></td><td><div align="center">注意</div></td></tr><tr><td><div align="center">本类构造</div></td><td><div align="center">父类的构造方法</div></td><td><div align="center">this()与sup<br>
<br>
er（）不能同时存在</div></td></tr></tbody></table><br>


1.构造方法<br>
 this（）是调用一个类中的兄弟方法（一般为构造方法）

```java

public Student(itn sID)
{
  this();
  this.sID.sID;
}
public Student(){
  System.out.println("无参数的构造方法");
}
```

super（）调用 父类的构造方法

```java
public Student(){
 super(10);
  System.out.println("子类的构造方法");
}
```

2.属性<br>
 调用属性的顺序：先搜索本类中是否存在，不存在则去寻找父类<br>
<br>
 this.age   先搜索本类是否存在，不存在则去父类寻找<br>
 super.age   调用父类的age属性<br>

```java

public class A{

 int x=0;
}

publib class B extends A{

    public void setX(int x)

   {

      this.x=x;//此时的x本类中不存在所以是父类的



   }

}

public class A{
   int x=0;
}

publib class B extends A{
  public void setX(int x)
   {
    this.x=x;//此时的x是本类的，想调用父类的x使用super.x
  }

}
```



3.方法<br>
 <br>
<table class="t_table" style="width:50%" cellspacing="0"><tbody><tr><td> this.show()</td><td> super.show()</td></tr><tr><td> this.show()先搜索本类是否存在不存在则父类</td><td> 调用父类的show（）方法</td></tr></tbody></table><br>

```java

//重写父类的printA方法
public void printA()
{
  super.printA();//调用父类的
  system.out.println("子类的")
}

运行结果：
  父类的
  子类的
```

### 抽象类和接口的区别

**抽象类**：定义了抽象函数的类也必须被abstract关键字修饰，被abstract关键字修饰的类是抽象类。方法功能声明相同，但方法功能主体不同。那么这时也可以抽取，但只抽取方法声明，不抽取方法主体。那么此方法就是一个抽象方法。

抽象类中是否可以没有抽象方法？如果可以，那么，该类还定义成抽象类有意义吗？为什么？——可以没有抽象方法，有意义，不会让其他人直接创建该类对象

 **抽象类的特点**：       

    1. 抽象类与抽象方法都必须使用 abstract来修饰
    2. 抽象类不能直接创建对象
    3. 抽象类中可以有抽象方法，也可以没有抽象方法
    4. 抽象类的子类
           1. 实现了抽象方法的具体类
           2. 抽象类

**接口：**接口是功能的结合。接口只描述应该具备的方法，并没有具体的实现，将功能的定义与实现分离，优化了程序的设计。

**接口特点：**

1、接口中可以定义变量，但是变量必须有固定的修饰符修饰，public static final 所以接口中的变量也称之为常量，其值不能改变。后面我们会讲解static与final关键字

2、接口中可以定义方法，方法也有固定的修饰符，public abstract

3、接口不可以创建对象。

4、子类必须覆盖掉接口中所有的抽象方法后，子类才可以实例化。否则子类是一个抽象类。

**相同点:**

- 都位于继承的顶端,用于被其他类实现或继承;


- 都不能直接实例化对象;


- 都包含抽象方法,其子类都必须覆写这些抽象方法;

**区别:**

- 抽象类为部分方法提供实现,避免子类重复实现这些方法,提高代码重用性;接口只能包含抽象方法;


- 一个类只能继承一个直接父类(可能是抽象类),却可以实现多个接口;(接口弥补了Java的单继承)
- 抽象类是这个事物中应该具备的你内容, 继承体系是一种 is..a关系
- 接口是这个事物中的额外内容,继承体系是一种 like..a关系