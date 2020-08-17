---
layout: note
title: ssm的继续学习
author: hanhansun233
category: 2020暑假第四周
date: 2020-08-03
---

## SpringMVC

### 概述

Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。

### 环境配置

springMVC基于servlet,故需要导入servlet依赖包

### 工作流程

客户端请求提交到 DispatcherServlet。
由 DispatcherServlet 控制器寻找一个或多个 HandlerMapping，找到处理请求的 Controller。
DispatcherServlet 将请求提交到 Controller。
Controller 调用业务逻辑处理后返回 ModelAndView。
DispatcherServlet 寻找一个或多个 ViewResolver 视图解析器，找到 ModelAndView 指定的视图。
视图负责将结果显示到客户端。

### RESTful风格接口的实现

创建一个控制层

@Controller
public class RestfulController {
    @GetMapping("/add/{a}/{b}")
    public String add(@PathVariable int a, @PathVariable int b, Model model) {
        model.addAttribute("msg", "结果为" + (a + b));
        //这个return的字符串,会被视图解析器拼接成完整字符串
        return "test";
    }
}

在浏览器栏访问即可

### 去除视图解析器的整法

当我们不需要视图解析器时,我们直接在return的字符串上面操作

方式一:正常跑法

@RequestMapping("/hello")
public String hello(Model model){
    model.addAttribute("msg","Hello,SpringMVC!");
    return "forward:/WEB-INF/jsp/hello.jsp";
}
方式二:重定向跑法

@RequestMapping("/hello")
public String hello(Model model){
    model.addAttribute("msg","Hello,SpringMVC!");
    return "redirect:/index.html";
}

