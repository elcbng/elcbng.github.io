---
layout: note
title: maven、Mybatis、Spring入门学习
author: hanhansun233
category: 2020暑假第三周
date: 2020-07-27
---

## maven

### 概念

maven是优秀的项目管理和构建工具，能让我们更为方便的来管理和构建项目。它包含了一个项目对象模型 (Project Object Model)，一组标准集合，一个项目生命周期(Project Lifecycle)，一个依赖管理系统(Dependency Management System)，和用来运行定义在生命周期阶段(phase)中插件(plugin)目标(goal)的逻辑。

### 什么是依赖管理

一个java项目需要外部的第三方jar包来进行支持。我们说这个java项目依赖了这些jar包。
依赖管理就是将项目所依赖的jar包按照一定规则进行规范化管理。

### maven常用的构建命令

mvn -v 查看maven版本
mvn compile 用来将src/main/java下的文件编译为class文件，并输出到target中。
mvn test test 用来将src/main/test下的文件进行编译，同时执行一次
mvn package 打包,将项目进行打包，如果是jar打包为jar，war打包为war。

mvn clean 删除编译产生的target文件夹
mvn install 安装jar包到本地仓库中

### 仓库之间的关系

本地项目需要jar包，先从本地仓库中获取，如果本地仓库中没有，则从私服中获取，如果私服没有，则从中央仓库获取。获取到后，本地仓库及远程仓库各存储一份。如果没有远程仓库，本地仓库则直接从中央仓库获取，然后在本地仓库存储一份。

### 实战

本周，噢不，上周已经使用 IDEA 构建一个 Maven 的极其简单的工程。

## Mybatis

### 概述

是一个持久层框架

### xml的使用

导入依赖，这里直接贴代码（太懒了2333）
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.5</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.21</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>3.8.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>

### MyBatis执行流程

Resources获取加载全局配置文件
实例化sqlSessionFactoryBuilder
解析文件流XMLConfigBuilder
Configuration所有的配置信息
sqlSessionFactory实例化
transaction事务管理
创建executor执行器
创建sqlSession

## Spring

### 概述

Spring是一个开源的免费的框架（容器）
Spring是一个轻量级的、非入侵式的框架
控制反转（IOC），面向切面编程（AOP）
支持事务处理，对框架整合的支持

### IOC

Inversion of Control  它是一个技术思想，不是一个技术实现。它描述的事情是Java开发领域对象的创建，管理问题。

尝试用自己的话说就是用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码。如果程序代码量十分大，修改一次的成本代价身份昂贵。我们使用一个set接口实现，使用set注入后，程序不再具有主动性，而是变成了被动。这种思想，从本质上解决了问题，我们不再管理对象的创建，系统耦合性大大降低，可以专注在业务的实现上。这是IOC的原型。

### AOP

AOP（Aspect Oriented Programming），意为：面向切面编程
通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

核心思想：是将业务逻辑中与类不相关的通用功能切面式的提取分离出来，让多个类共享一个行为，一旦这个行为发生改变，不必修改类，而只需要修改这个行为即可。