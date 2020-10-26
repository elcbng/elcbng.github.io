---
layout: note
title: Thymeleaf的总结
author: 陈继淼
category: 2020暑假第六周
date: 2020-08-23
---

# Thymeleaf

**简单说， Thymeleaf 是一个跟 Velocity、FreeMarker 类似的模板引擎，它可以完全替代 JSP 。**


1. Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。
2. Thymeleaf 开箱即用的特性。它提供标准和spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、该jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。
3. Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。

## 1.引入依赖

springboot直接引入：
```html
	　　<dependency>
	      <groupId>org.springframework.boot</groupId>
	      <artifactId>spring-boot-starter-thymeleaf</artifactId>
	    </dependency>
```

　springboot1.4之后，可以使用thymeleaf3来提高效率，并且解决标签闭合问题，配置方式：

```html
	<properties>
	    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	    <!-- set thymeleaf version -->
	    <thymeleaf.version>3.0.0.RELEASE</thymeleaf.version>
	    <thymeleaf-layout-dialect.version>2.0.0</thymeleaf-layout-dialect.version>
	    <!--set java version-->
	    <java.version>1.8</java.version>
	</properties>
```


## 2.配置thymeleaf视图解析器
```
	#thymeleaf start
	spring.thymeleaf.mode=HTML5
	spring.thymeleaf.encoding=UTF-8
	spring.thymeleaf.content-type=text/html
	#开发时关闭缓存,不然没法看到实时页面
	spring.thymeleaf.cache=false
	#thymeleaf end
```

## 3。编写控制器
```
	/**
	 * 测试demo的controller
	 *
	 * @author zcc ON 2018/2/8
	 **/
	@Controller
	public class HelloController {
	    private static final Logger log = LoggerFactory.getLogger(HelloController.class);
	
	    @GetMapping(value = "/hello")
	    public String hello(Model model) {
	        String name = "jiangbei";
	        model.addAttribute("name", name);
	        return "hello";
	    }
	}
```

## 4.编写模板html
```html
	<!DOCTYPE HTML>
	<html xmlns:th="http://www.thymeleaf.org">
	<head>
	    <title>hello</title>
	    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
	</head>
	<body>
	    <!--/*@thymesVar id="name" type="java.lang.String"*/-->
	    <p th:text="'Hello！, ' + ${name} + '!'">3333</p>
	</body>
	</html>
```