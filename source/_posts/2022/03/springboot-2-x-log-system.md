---
title: springboot-2.x-log-system
date: 2022-03-03 13:06:58
permalink:
tags:
    - springboot
    - log
    - log4j
    - sl4j
    - 日志
categories:
    - 技术生活
      - 编程技术
        - 后端技术
---
# springboot日志系统的理解
## 日志归一系统
写代码的时候突然对log感兴趣，想到了springboot之前了解的时候是依赖于sl4j实现的统一api，在编译的时候在决定用哪一个日志框架。但是翻了一下网上的文章。得出了**Spring Boot 可以自动的适配日志框架，而且底层使用 SLF4 + LogBack**的结论。
![20220303131251](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220303131251.png)

从图中可以看出几点
1. 所有其他日志系统例如，jul ，log4j ，logback 都是实现了和sl4j的桥接。
2. 其中只有logback实现了sl4j，这个也是得出默认springboot支持logback的原因。


## 在springboot中，各个日志框架究竟是如何和平相处，并且相安无事的
我们写代码，免不了使用第三方的库，假设第三方库没有实现sl4j的api，那么这个日志岂不是不受到springboot的统一管理了？？？不是这样的，就类似上面的spring-logging 这个库，一些第三方的流行的库，会有一些工具包来替换接管这些第三方系统的日志。
大致原理，如下，下面是一个sl4j官方的一个图。
![20220303132843](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220303132843.png)


# 如何替换默认的logback的实现
```
<dependency>    
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>    
    <exclusions>           
        <exclusion>            
           <artifactId>spring-boot-starter-logging</artifactId>            							    
           <groupId>org.springframework.boot</groupId>        
        </exclusion>   
    </exclusions> 
</dependency>

<dependency>    
    <groupId>org.springframework.boot</groupId>    
    <artifactId>spring-boot-starter-log4j2</artifactId> 
</dependency>
```
