---
layout: post
title: "条件判断"
date: 2018-03-13   
tag: "python" 
---
### 条件判断

1. if
  语法：
```
if 条件 :
	执行语句1
	执行语句2
	........
```
2. if，then
  语法：
```
if 条件 :
	执行语句1
	执行语句2
	........（以上代码为if的代码块如果if条件不满足则直接执行下面的代码）
执行语句
```
3. if，else
  语法：
```
if 条件 :
	执行语句1
	执行语句2
	........
else：
	执行语句
```
4. elif
  语法：
```
if 条件 :
	执行语句1
	........
elif 条件：
	执行语句
elif 条件：
	执行语句
.......(可以有多个elif,最后结束判断用else)
else：
	执行语句
```
5. and/or
  ![boolean operators](//img-blog.csdn.net/20180313165308219?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3l1YW55aTA1MDE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

6. 获取输入input()
+ input('statement')——括号内的参数：输出在控制台上的，等待用户输入的提示性语言
+ 可以用变量接收输入的值。day=input("enter the day of the week")
