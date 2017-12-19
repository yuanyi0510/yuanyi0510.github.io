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
      <br>

```
public class Singleton{

private Singleton(){

    private static final Singleton singleObject=new Singleton();

    public static Singleton getInstance(){//getInstance一般是获取单例模式的对象     return singleObject;

 }

}

}  

```

优点：线程安全&lt;br&gt;


&nbsp; &nbsp;缺点：如果长期不使用，就会一直占用内存空间<br>
<br>
<br>
&nbsp; &nbsp;懒汉式：<br>
&nbsp; &nbsp;<br>
&nbsp; &nbsp;&nbsp;&nbsp;<br>
```
public class Singleton{
//1.私有化构造方法
 private Singleton(){}
//声明变量
 private static Singleton singleObject;
//定义公有的方法返回变量
public static Singleton getInstance(){//getInstance一般是获取单例模式的对象if(singleObject==null){
singleObject=new Singleton();
}
return singleObject;
}
}
```

优点：不会过早的new空间<br>
&nbsp; &nbsp;缺点：多线程使用单例，会出现线程不安全问题<br>
<br>

