---
layout: note
title: HTML、CSS、JavaScript的总结
author: 陈继淼
category: 2020暑假第五周
date: 2020-08-09
---



#MyBatis

MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis，是一个基于Java的持久层框架。


* 持久层： 可以将业务数据存储到磁盘，具备长期存储能力，只要磁盘不损坏，在断电或者其他情况下，重新开启系统仍然可以读取到这些数据。
* 优点： 可以使用巨大的磁盘空间存储相当量的数据，并且很廉价
* 缺点：慢（相对于内存而言）



在我们传统的 JDBC 中，我们除了需要自己提供 SQL 外，还必须操作 Connection、Statment、ResultSet，不仅如此，为了访问不同的表，不同字段的数据，我们需要些很多雷同模板化的代码，闲的繁琐又枯燥。

而我们在使用了 MyBatis 之后，只需要提供 SQL 语句就好了，其余的诸如：建立连接、操作 Statment、ResultSet，处理 JDBC 相关异常等等都可以交给 MyBatis 去处理，我们的关注点于是可以就此集中在 SQL 语句上，关注在增删改查这些操作层面上。

并且 MyBatis 支持使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。


##第一个 MyBatis 程序

###第一步：准备数据库
首先我们创建一个数据库【mybatis】，编码方式设置为 UTF-8，然后再创建一个名为【student】的表，插入几行数据：

	DROP DATABASE IF EXISTS mybatis;
	CREATE DATABASE mybatis DEFAULT CHARACTER SET utf8;
	
	use mybatis;
	CREATE TABLE student(
	  id int(11) NOT NULL AUTO_INCREMENT,
	  studentID int(11) NOT NULL UNIQUE,
	  name varchar(255) NOT NULL,
	  PRIMARY KEY (id)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
	INSERT INTO student VALUES(1,1,'我没有三颗心脏');
	INSERT INTO student VALUES(2,2,'我没有三颗心脏');
	INSERT INTO student VALUES(3,3,'我没有三颗心脏');

###第二步：创建工程

在 IDEA 中新建一个 Java 工程，并命名为【HelloMybatis】，然后导入必要的 jar 包：

* mybatis-3.4.6.jar
* mysql-connector-java-5.1.21-bin.jar

###第三步：创建实体类

在 Package【pojo】下新建实体类【Student】，用于映射表 student：

	package pojo;
	
	public class Student {
	
	    int id;
	    int studentID;
	    String name;
	
	    /* getter and setter */
	}


###第四步：配置文件 mybatis-config.xml

在【src】目录下创建 MyBaits 的主配置文件 mybatis-config.xml ，其主要作用是提供连接数据库用的驱动，数据名称，编码方式，账号密码等，我们在后面说明：

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	
	    <!-- 别名 -->
	    <typeAliases>
	        <package name="pojo"/>
	    </typeAliases>
	    <!-- 数据库环境 -->
	    <environments default="development">
	        <environment id="development">
	            <transactionManager type="JDBC"/>
	            <dataSource type="POOLED">
	                <property name="driver" value="com.mysql.jdbc.Driver"/>
	                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=UTF-8"/>
	                <property name="username" value="root"/>
	                <property name="password" value="root"/>
	            </dataSource>
	        </environment>
	    </environments>
	    <!-- 映射文件 -->
	    <mappers>
	        <mapper resource="pojo/Student.xml"/>
	    </mappers>
	
	</configuration>


###第五步：配置文件 Student.xml
在 Package【pojo】下新建一个【Student.xml】文件：


	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE mapper
	        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	
	<mapper namespace="pojo">
	    <select id="listStudent" resultType="Student">
	        select * from  student
	    </select>
	</mapper>


* 由于上面配置了 <typeAliases> 别名，所以在这里的 resultType 可以直接写 Student，而不用写类的全限定名 pojo.Student
* namespace 属性其实就是对 SQL 进行分类管理，实现不同业务的 SQL 隔离
* SQL 语句的增删改查对应的标签有：



###第六步：编写测试类

在 Package【test】小创建测试类【TestMyBatis】：


	package test;
	
	import org.apache.ibatis.io.Resources;
	import org.apache.ibatis.session.SqlSession;
	import org.apache.ibatis.session.SqlSessionFactory;
	import org.apache.ibatis.session.SqlSessionFactoryBuilder;
	import pojo.Student;
	
	import java.io.IOException;
	import java.io.InputStream;
	import java.util.List;
	
	public class TestMyBatis {
	
	    public static void main(String[] args) throws IOException {
	        // 根据 mybatis-config.xml 配置的信息得到 sqlSessionFactory
	        String resource = "mybatis-config.xml";
	        InputStream inputStream = Resources.getResourceAsStream(resource);
	        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
	        // 然后根据 sqlSessionFactory 得到 session
	        SqlSession session = sqlSessionFactory.openSession();
	        // 最后通过 session 的 selectList() 方法调用 sql 语句 listStudent
	        List<Student> listStudent = session.selectList("listStudent");
	        for (Student student : listStudent) {
	            System.out.println("ID:" + student.getId() + ",NAME:" + student.getName());
	        }
	
	    }
	}




###基本原理
* 应用程序找 MyBatis 要数据
* MyBatis 从数据库中找来数据
	* 1.通过 mybatis-config.xml 定位哪个数据库
	* 2.通过 Student.xml 执行对应的 sql 语句
	* 3.基于 Student.xml 把返回的数据库封装在 Student 对象中
	* 4.把多个 Student 对象装载一个 Student 集合中
* 返回一个 Student 集合



