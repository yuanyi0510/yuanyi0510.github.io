---
layout: post
title: "使用DateFormat需设置语言环境"
date: 2018-01-08   
tag: Java 
---

转载：<http://iyiguo.net/blog/2014/05/19/java-dateformat-instance/>

先看段代码：
```
String sDate = "2014-12-20";
try {
    DateFormat df = DateFormat.getDateInstance();
    Date date = df.parse(sDate);
} catch (Exception e) {
    e.printStackTrace();
}
```
这段代码在中文语言环境下是可以测试通过。但在其他语言环境中则会抛出异常。原因是DateFormat.getDateInstance()初始化时会跟据当前语言环境设置日期格式。

1. DateFormat.getDateInstance() 根据当前语言环境设置日期模式：
```
 zh_CN : yyyy-MM-dd : 2014-05-19
 en_us : MMM d, yyyy : May 19, 2014
```
2. DateFormat.getDateInstance(int style) 根据当前语言环境设置指定的日期模式：
```
// Locale.SIMPLIFIED_CHINESE | Locale.CHINA | Locale.CHINESE
 // DateFormat.DEFAULT == DateFormat.MEDIUM
 DateFormat.getDateInstance(DateFormat.DEFAULT); // yyyy-M-d
 DateFormat.getDateInstance(DateFormat.FULL);// yyyy'年'M'月'd'日' EEEE
 DateFormat.getDateInstance(DateFormat.LONG);// yyyy'年'M'月'd'日'
 DateFormat.getDateInstance(DateFormat.MEDIUM);// yyyy-M-d
 DateFormat.getDateInstance(DateFormat.SHORT);// yy-M-d
 // Locale.US
 DateFormat.getDateInstance(DateFormat.DEFAULT, Locale.CHINESE); // MMM d, yyyy
 DateFormat.getDateInstance(DateFormat.LONG);// MMMM d, yyyy
 DateFormat.getDateInstance(DateFormat.SHORT);// M/d/yy
 DateFormat.getDateInstance(DateFormat.MEDIUM);// MMM d, yyyy
 DateFormat.getDateInstance(DateFormat.FULL);// EEEE, MMMM d, yyyy
```
3. DateFormat.getDateInstance(int style, Locale aLocale) 指定语言环境及相应日期模式
```
DateFormat.getDateInstance(DateFormat.DEFAULT, Locale.CHINESE);
 // 上述代码等价于
 Locale.setDefault(Locale.CHINESE);
 DateFormat.getDateInstance(DateFormat.DEFAULT)
```
因此，在使用DateFormat时，尽量不要使用无参的getDateInstance()方法，因其依赖具体的环境，有可能会造成不同环境运行异常的情况。如果确实不知道具体的语言环境极其格式模式的，那么使用SimpleDateFormat直接指定日期格式即可.这样可以避免因不同语言环境导致的日期格式错误。
```
String sDate = "2014-12-20";
try {
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
    Date date = sdf.parse(sDate);
} catch (Exception e) {
    e.printStackTrace();
}
```

