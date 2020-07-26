---
layout: note
title: Java基础知识和MySQL基础知识
author: hanhansun233
category: 2020暑假第二周
date: 2020-07-26
---

## 线程

### 概念

进程（process）表示程序的一次执行过程，程序运行的生命周期
线程（thread）表示程序内部的一条执行路径
1. 若一个程序同一时间并行执行多个进程，则表示支持多线程
2. 线程作为调度和执行的单位，每个线程拥有独立的运行栈和程序计数器
3. 多个线程共享方法区和堆
4. 一个java应用程序至少有三个线程，main方法主线程、垃圾回收线程、异常处理线程

并发与并行
并行(parallel)：指在同一时刻，有多条指令在多个处理器上同时执行。
并发(concurrency)：指在同一时刻只能有一条指令执行，但多个进程指令被快速的轮换执行。

在并发程序中可以同时拥有两个或者多个线程。这意味着，如果程序在单核处理器上运行，那么这两个线程将交替地换入或者换出内存。
这些线程是同时“存在”的——每个线程都处于执行过程中的某个状态。如果程序能够并行执行，那么就一定是运行在多核处理器上。
此时，程序中的每个线程都将分配到一个独立的处理器核上，因此可以同时运行。


“并行”概念是“并发”概念的一个子集。也就是说，你可以编写一个拥有多个线程或者进程的并发程序，
但如果没有多核处理器来执行这个程序，那么就不能以并行方式来运行代码。因此，凡是在求解单个问题时涉及多个执行流程的编程模式或者执行行为，
都属于并发编程的范畴。

摘自：《并发的艺术》 — 〔美〕布雷谢斯

### 线程的创建

#### 方法一：

创建一个继承于Thread的子类
重写Thread子类的run方法 -> 将执行的操作声明在run()中
创建子类对象
调用start方法

#### 方法二：

创建一个实现了Runnable接口的类
实现类实现Runnable中的抽象方法：run()
创建实现类的实例
将此对象作为参数传到Thread类的构造器中，创建Thread类的对象
通过Thread类的对象调用start()
此时如果想打印线程名，则只能使用currentThread()

### 线程的同步

#### 方法一：同步块

同步代码块synchronized(同步监视器){需要被同步的代码}

同步监视器俗称锁,Java中对锁的要求：任何一个类的对象都可以作为锁，但多个线程只能共用同一个锁

#### 方法二：同步方法

## 泛型

### 概念

本质是参数化类型，也就是说所操作的数据类型被指定为一个参数

### 使用

泛型不能使用基本数据类型，应使用包装类

ArrayList<Integer> list = new ArrayList<>():单定义方式

HashMap<Integer, String> hashMap = new HashMap<>():双定义

Set<Map.Entry<Integer, String>> entries = hashMap.entrySet():泛型的嵌套

同一实现类不同泛型的对象，不能相互赋值

静态方法不能用类的泛型

无法使用instanceof关键字或==判断泛型类的类型

泛型类可以继承其它泛型类

## IO流

### 概念

流：流动，流向，从一端到另一端，源头与目的地，程序与 文件|网络|数组|数据库.... 之间的联系，以程序为中心

### 分类

inputStream：字节输入流

outputStream：字节输出流

Reader：字符输入流

Writer：字符输出流

字节流没有缓冲区，是直接输出的，而字符流是输出到缓冲区的。因此在输出时，字节流不调用colse()方法时，信息已经输出了，而字符流只有在调用close()方法关闭缓冲区时，信息才输出。要想字符流在未关闭时输出信息，则需要手动调用flush()方法。

## 反射

### 概念

JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。

### Class类    

Class代表类的实体，在运行的Java应用程序中表示类和接口。

方式一:已知具体的类    Class<? extends User> clazz1 = User.class;

方式二:已知某个类的实例   User user = new User();   Class<? extends User> clazz2 = user.getClass();

方式三:基本数据类型使用类.Type    Class<Integer> type = Integer.TYPE;

获取父类类型   Class<? super Dog> superclass = Dog.class.getSuperclass();

### Field类

Field代表类的成员变量（成员变量也称为类的属性）。

Modifier and Type	Method and Description
boolean	            equals(Object obj)               将此 Field与指定对象进行比较。
Object	            get(Object obj)                  返回该所表示的字段的值 Field ，指定的对象上。
void                set(Object obj, Object value)    将指定对象参数上的此 Field对象表示的字段设置为指定的新值。

### Method类

Method代表类的方法。

Modifier and Type	Method and Description
boolean             equals(Object obj)                     将此 方法与指定对象进行比较。
Object              invoke(Object obj, Object... args)     在具有指定参数的 方法对象上调用此 方法对象表示的底层方法。

## MySQL基础部分

学生表部分：  
DROP TABLE IF EXISTS `student_table`;  
CREATE TABLE `student_table`  (  
  `stu_ID` int(11) NOT NULL,
  `stu_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  `stu_sex` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  `classID` int(11) NOT NULL,  
  `stu_hobby` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  `stu_address` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  PRIMARY KEY (`stu_ID`) USING BTREE
)   
ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

SET FOREIGN_KEY_CHECKS = 1;

班级表部分：  
DROP TABLE IF EXISTS `class_table`;  
CREATE TABLE `class_table`  (  
  `class_ID` int(11) NOT NULL,  
  `class_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  PRIMARY KEY (`class_ID`) USING BTREE
)   
ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

学院表部分：  
DROP TABLE IF EXISTS `dept_table`;  
CREATE TABLE `dept_table`  (  
  `dept_ID` int(11) NOT NULL,  
  `dept_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  `dept_add` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  `dept_res` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  PRIMARY KEY (`dept_ID`) USING BTREE
)   
ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

学生班级关联表：  
DROP TABLE IF EXISTS `stu&class`;  
CREATE TABLE `stu&class`  (  
  `stu_ID` int(11) NOT NULL,  
  `class_ID` int(11) NOT NULL,  
  PRIMARY KEY (`stu_ID`) USING BTREE,  
  INDEX `class_ID`(`class_ID`) USING BTREE,  
  CONSTRAINT `class_ID` FOREIGN KEY (`class_ID`) REFERENCES `class_table` (`class_ID`) ON DELETE RESTRICT ON UPDATE RESTRICT,  
  CONSTRAINT `stu_ID` FOREIGN KEY (`stu_ID`) REFERENCES `student_table` (`stu_ID`) ON DELETE RESTRICT ON UPDATE RESTRICT
)   
ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

班级学院关联表：  
DROP TABLE IF EXISTS `class_table`;  
CREATE TABLE `class_table`  (  
  `class_ID` int(11) NOT NULL,  
  `class_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,  
  PRIMARY KEY (`class_ID`) USING BTREE
)   
ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;


PS：由于本周都在学车，所以耽误了许多学习时间（当然没有打游戏耽误得多，下周少打一点23333），主要是对之前学习的Java和MySQL知识的复习以及继续学习Java，师兄们就当看个小学读物233333.