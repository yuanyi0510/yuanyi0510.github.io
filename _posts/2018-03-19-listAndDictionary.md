---
layout: post
title: "list and dictionary"
date: 2018-03-19  
tag: "python" 
---

  ### list
    1. 定义:python中的集合是一种容器，可以存储任何数据(在js中一个python的list就是一个array)
    2. 例如：
  + 一个空的集合：empty = [ ]
  + 数字集合：nums = [10, 20, 30, 40.4]
  + 字符串集合：str = ['w', 'o', 'r', 'd']
  + 混合类型的集合：anything = [10, 'hi', 'python', 12.4]
    3. 一个list的索引就是它的位置，通常索引是以0开始
    4. 可以通过索引获得指定的集合项，比如：nums[0]；也可以通过一个范围获取一个字列表，比如：anything[1:3]——>['hi', 'python']
    5. 可以创建一个有元素的list或者空的list ，eg：arr = []；
     可以通过append()方法像集合中添加元素，eg：arr.append('12')；
     可以通过remove()方法从集合中移除元素，通过元素的值，eg：arr.remove('12')；
     可以通过del 关键字删除元素，通过元素的索引，eg：del arr[0]；
    6. 两个结合必须有相同的元素以及顺序才能相等
    7. 二维集合
  ```
  menus = [ ['Spam n Eggs', 'Spam n Jam', 'Spam n Ham'], 
  		['SLT (Spam-Lettuce-Tomato)', 'PB&S (PB&Spam)'],
  		['Spalad', 'Spamghetti', 'Spam noodle soup']]
  menus[0][1]---'Spam n Jam'
  ```

  ### dictionary
    1. 定义：字典与列表类似，但可以使用键而不是索引查找值，eg：str={'name':'jim', 'sex':'man', 'age': 12}；
    2. 向字典中添加或者更新元素，str['add'] = '中国'
    3. 删除元素用del , eg： del str['add'] ——同时删除key和value
    4.如果使用name['key']这种方式获取值，当key不存在是会造成错误；可以使用get()方法，如果返回结果是none说明元素不存在
    4. none 表示没有值，并且在条件判断中表示false
    5. 因为dictionary是无序的，所以是要拥有相同的键值对就是相等的
    6. 可以在dictionary中嵌套使用list, 
  ```
  eg：menus = {'Breakfast': ['Spam n Eggs', 'Spam n Jam', 'Spam n Ham'],
  			'Lunch': ['SLT (Spam-Lettuce-Tomato)', 'PB&S (PB&Spam)'], 
  			'Dinner': ['Spalad', 'Spamghetti', 'Spam noodle soup'] }
     print('Breakfast Menu:\t', menus['Breakfast'])
  ```
    8. 如果想获取dictionary中的所有键，可以使用keys()方法；
      如果想获取dictionary中的所有值，可以使用values()方法

  ### 循环
    1. for name in list:
      执行体
    2. eg：
  ```
  prices = [2.50, 3.50, 4.50]
  for price in prices: //price是一个临时变量来存储每次循环是prices的值
  	print('Price is', price)

  menu_prices = {'Knackered Spam': 0.50, 'Pip pip Spam': 1.50, 'Squidgy Spam': 2.50, 'Smashing Spam': 3.50}
  for name, price in menu_prices.items(): 
  	print(name, ': $', price)
  ```
    3. random 的使用
  + random.random()   生成一个[0.0,1.0)的随机数
  + random.choice( [list] )   从list中随机选出一个值
  + random.randint(start, end)  在[start,end)中随机生成一个整数
  + range(num) 生成从[0, num-1]的列表
  + range(start, end, step)
    4. format 的使用
  + print（）默认有一个空格来分隔每个单词。我们可以通过设置sep =空字符串来覆盖它
  ```
  print(name, ': $', price, sep='')
  ```
  + 我们可以使用format（）函数将我们的price float格式化为两位小数
  ```
  print(name, ': $', format(price, '.2f'), sep='')
  ```
    5. while 循环
  ```
  while 条件:
    执行语句
  ```

