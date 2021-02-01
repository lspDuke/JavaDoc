# Spring-Boot 整合 thymeleaf

> Thymeleaf 是一个跟 Velocity、FreeMarker 类似的模板引擎，它可以完全替代 JSP 。相较与其他的模板引擎，它有如下三个极吸引人的特点
>
> Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外
> 的属性来达到模板 + 数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。
>
> Thymeleaf 开箱即用的特性。它提供标准和 Spring 标准两种方言，可以直接套用模板实现 JSTL、 OGNL 表达式效果，避免每天套模板、改 JSTL、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。
>
> Thymeleaf 提供 Spring 标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。

## 引用

```java
 <!--引入thymeleaf依赖-->
<dependency>  		 	 	
     <groupId>org.springframework.boot</groupId>
	 <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

## 模板

```html
<!doctype html>

<!--注意：引入thymeleaf的名称空间-->
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <p th:text="'hello SpringBoot'">hello thymeleaf</p>
</body>
</html>
```

## yml 配置

```yml
spring:
  thymeleaf:
    enabled: true  #开启thymeleaf视图解析
    encoding: utf-8  #编码
    prefix: classpath:/templates/  #前缀
    cache: false  #是否使用缓存
    mode: HTML  #严格的HTML语法模式
    suffix: .html  #后缀名
```

