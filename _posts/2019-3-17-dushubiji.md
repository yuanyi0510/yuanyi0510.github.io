---
layout: post
title: "猫眼电影"
date: 2019-3-14
tag: 面试总结 
---

# 一面

### 自我介绍

### 介绍一下项目

### 项目中的单点登陆怎么实现的，为什么会携带token

http无状态

### 项目中为什么使用zookeeper

1. 配置信息同步
2. 集群内节点状态快速感知

### 项目中使用redis做了什么

1. 使用redis存储用户登录信息
2. 使用redis的list结构存储用户订阅的币种信息

……

### 说一下Java的特征

面向对象，封装、继承、多态

#### 封装

利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体。数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，只保留一些对外接口使之与外部发生联系。用户无需知道对象内部的细节，但可以通过对象对外提供的接口来访问该对象。

优点：

- 减少耦合：可以独立地开发、测试、优化、使用、理解和修改
- 减轻维护的负担：可以更容易被程序员理解，并且在调试的时候可以不影响其他模块
- 有效地调节性能：可以通过剖析确定哪些模块影响了系统的性能
- 提高软件的可重用性
- 降低了构建大型系统的风险：即使整个系统不可用，但是这些独立的模块却有可能是可用的

#### 继承

继承实现了 **IS-A** 关系，例如 Cat 和 Animal 就是一种 IS-A 关系，因此 Cat 可以继承自 Animal，从而获得 Animal 非 private 的属性和方法。

继承应该遵循里氏替换原则，子类对象必须能够替换掉所有父类对象。

#### 多态

多态有三个条件：

- 继承
- 覆盖（重写）
- 向上转型

### 了解线程么，讲述一下

这个范围太宽泛了，我就大概讲述了一下线程池的原理

### 知道锁么，阐述一下

锁也是一样的，讲述了一下synchronized和volatitle关键字

# 二面

### 自我介绍

### 讲述项目

### 给一个数组N，求最大的M个整数

```java
public class Maoyan {
    public static void main(String[] args) {
        int[] arr={1,2,3,4,5};
        int [] sortedList=sort(arr);
        int M=2;
        for (int i = 0; i < M; i++) {
            System.out.println(sortedList[i]);
        }
    }

    public  static  int [] sort(int[]  arr){
        for (int i=0;i<arr.length-1;i++){
            for (int j=0;j<arr.length-1-i;j++){
                if (arr[j]<arr[j+1]){
                    int temp=arr[j+1];
                    arr[j+1]=arr[j];
                    arr[j]=temp;
                }
            }
        }
        return arr;
    }
}

```

### 查找一个整数是否在二叉排序树中

```java
class TreeNode {
    int key;
    int value;
    TreeNode left;
    TreeNode right;

    TreeNode(int key, int value) {
        this.key = key;
        this.value = value;
    }

}

public class SortedTree {
    public static void main(String[] args) {

    }

    public static boolean find(int key, TreeNode root) {
        TreeNode currentNode = root;
        while (currentNode != null && currentNode.key != key) {
            if (key < currentNode.key) {
                currentNode = currentNode.left;
            } else if (key > currentNode.key) {
                currentNode = currentNode.right;
            } else {
                return true;
            }
        }
        return false;
    }
}

```

