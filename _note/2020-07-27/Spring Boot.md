---
layout: note
title: sprng boot的学习
author: 陈继淼
category: 2020暑假第三周
date: 2020-07-26
---



#sprng boot的学习
>Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。通过这种方式，Spring Boot致力于在蓬勃发展的快速应用开发领域成为领导者。

SpringBoot所具备的特征有：

* 可以创建独立的Spring应用程序，并且基于其Maven或Gradle插件，可以创建可执行的JARs和WARs；

* 内嵌Tomcat或Jetty等Servlet容器；
* 提供自动配置的“starter”项目对象模型（POMS）以简化Maven配置；
* 尽可能自动配置Spring容器；
* 提供准备好的特性，如指标、健康检查和外部化配置；
* 绝对没有代码生成，不需要XML配置。


***从最根本上来讲，Spring Boot就是一些库的集合*** 

##Maven依赖

使用Spring创建Web应用程序所需的最小依赖项：
	<pre style="-webkit-tap-highlight-color: transparent; box-sizing: border-box; font-family: Consolas, Menlo, Courier, monospace; font-size: 16px; white-space: pre-wrap; position: relative; line-height: 1.5; color: rgb(153, 153, 153); margin: 1em 0px; padding: 12px 10px; background: rgb(244, 245, 246); border: 1px solid rgb(232, 232, 232); font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;"><dependency>
	 <groupId>org.springframework</groupId>
	 <artifactId>spring-web</artifactId>
	 <version>5.1.0.RELEASE</version>
	</dependency>
	<dependency>
	 <groupId>org.springframework</groupId>
	 <artifactId>spring-webmvc</artifactId>
	 <version>5.1.0.RELEASE</version>
	</dependency>
	</pre>


**与Spring不同，Spring Boot只需要一个依赖项来启动和运行Web应用程序：**

	<pre style="-webkit-tap-highlight-color: transparent; box-sizing: border-box; font-family: Consolas, Menlo, Courier, monospace; font-size: 16px; white-space: pre-wrap; position: relative; line-height: 1.5; color: rgb(153, 153, 153); margin: 1em 0px; padding: 12px 10px; background: rgb(244, 245, 246); border: 1px solid rgb(232, 232, 232); font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;"><dependency>
	 <groupId>org.springframework.boot</groupId>
	 <artifactId>spring-boot-starter-web</artifactId>
	 <version>2.0.5.RELEASE</version>
	</dependency>
	</pre>


在构建期间，所有其他依赖项将自动添加到最终归档中。

另一个很好的例子是测试库。我们通常使用Spring Test，JUnit，Hamcrest和Mockito库集。在Spring项目中，我们应该将所有这些库添加为依赖项。

但是在Spring Boot中，我们只需要用于测试的启动器依赖项来自动包含这些库。

Spring Boot为不同的Spring模块提供了许多入门依赖项。一些最常用的是：

spring-boot-starter-data-jpa
spring-boot-starter-security
spring-boot-starter-test
spring-boot-starter-web
spring-boot-starter-thymeleaf


##配置模板引擎

在Spring中，我们需要为视图解析器添加 thymeleaf-spring5依赖项和一些配置：


	<pre style="-webkit-tap-highlight-color: transparent; box-sizing: border-box; font-family: Consolas, Menlo, Courier, monospace; font-size: 16px; white-space: pre-wrap; position: relative; line-height: 1.5; color: rgb(153, 153, 153); margin: 1em 0px; padding: 12px 10px; background: rgb(244, 245, 246); border: 1px solid rgb(232, 232, 232); font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">@Configuration
	@EnableWebMvc
	public class MvcWebConfig implements WebMvcConfigurer {
	 @Autowired
	 private ApplicationContext applicationContext;
	 @Bean
	 public SpringResourceTemplateResolver templateResolver() {
	 SpringResourceTemplateResolver templateResolver = new SpringResourceTemplateResolver();
	 templateResolver.setApplicationContext(applicationContext);
	 templateResolver.setPrefix("/WEB-INF/views/");
	 templateResolver.setSuffix(".html");
	 return templateResolver;
	 }
	 @Bean
	 public SpringTemplateEngine templateEngine() {
	 SpringTemplateEngine templateEngine = new SpringTemplateEngine();
	 templateEngine.setTemplateResolver(templateResolver());
	 templateEngine.setEnableSpringELCompiler(true);
	 return templateEngine;
	 }
	 @Override
	 public void configureViewResolvers(ViewResolverRegistry registry) {
	 ThymeleafViewResolver resolver = new ThymeleafViewResolver();
	 resolver.setTemplateEngine(templateEngine());
	 registry.viewResolver(resolver);
	 }
	}
	</pre>


**Spring Boot 只需要spring-boot-starter-thymeleaf的依赖项 来启用Web应用程序中的Thymeleaf支持。**

**一旦依赖关系添加成功后，我们就可以将模板添加到src / main / resources / templates文件夹中，Spring Boot将自动显示它们。**








