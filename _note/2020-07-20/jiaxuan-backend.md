﻿---
layout: note
title: Java
category: 2020暑假第二周
author: jiaxuan.yu
date: 2020-07-20
---

[Java](https://www.liaoxuefeng.com/wiki/1252599548343744)

**数据类型**

byte, short, int, long, float, double, char, string, boolean

强制类型转换：（类型）+ 数据

转义：\

英文对应ASCII码，其他语言对应Unicode码
___
___
**Java语言特点**

* 面向对象
* 跨平台
* 简单
___
___
**类***class*

* 一个源文件可以定义多个类
* 一个类只能有一个主函数
* public修饰公开类，类名与文件名完全相同
* 一个源文件只能有一个公开类
___
___
**Package***包*

类似文件夹
___
___
**控制台输入**

* 引入包

 * `import java.util.Scanner;`
* 声明Scanner类型的变量
 
 * `Scanner + 名称 = new Scanner（System.in）`
* 使用Scanner类中的对应类型
 * `.nextInt();     //整数
      .nextDouble();   //小数
      .next();   //字符串
      .next().charAt(0);  //单个字符` 

*字符串的比较*：**equals**
___
___
**if**

**switch**

**while**

**dowhile**

**for**

增强的for：**for(循环变量 ：数组）

`int[] array;
 for(int i : array){}`
___
___
**随机数**

`Math.random()*n;`
___
___
**递归**

*注意条件*
___
___
**排序**

*冒泡排序*：相邻位置比较

*选择排序*：固定值与其他值依次比较

*JDK排序*：引用包（升序）

  `java.util.Arrays.sort(数组)`

  *降序时使用元素倒置*
___
___
**面向对象**

*创建对象*：**new**

  `Dog mydog = new Dog();`

*实例变量*：*this的应用*

*局部变量*

*变量修饰词*：

* public
* protected
* default
* private

*构造方法*： new对象时自动调用，与类方法同名

*方法的重载*：方法名相同，参数不同/实现方法不同

*面向对象三特性*：**封装 继承 多态**

**继承**：子类继承父类全部功能，并增加完善

* class 子类 extends 父类
* 一个子类只能有一个父类
* 一个父类可以有多个子类

**多态**：定义公共部分，具体行为放到子类，子类重写父类产生各自功能就是多态程序设计

* class 父类｛｝

* class 子类1 extends 父类｛｝

* class 子类2 extends 父类 ｛｝

**接口**：

* *接口声明*：interface  接口名称｛｝

* *接口实现*：class 类名 implements 接口名｛｝

* 只声明变量和方法，方法体留给实现接口的类来完成

**异常处理**：分为*运行时刻异常*和*非运行时刻异常*，
代码出错，系统抛出异常，码农捕获异常。

**异常**是Throwable型，有Errror和Exception两个子类

**异常处理器**：try块，catch块，finally块。
___
___
##集合
**集合**：若干个确定的元素所构成的整体

*集合类*：java.until包自带集合类：**Collection**，包括**List**、**Set**、**Map**

（还有好多好多，未完待续……555）
