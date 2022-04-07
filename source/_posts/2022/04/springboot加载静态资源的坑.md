---
title: springboot加载静态资源的坑
date: 2022-04-04 09:37:44
permalink:
tags:
    - SpringBoot
    - 问题解决
categories:
    - 技术生活
      - 编程技术
        - 后端技术
---

## 预备知识
我们知道springboot中，已经预定义了一些配置，这些配置，指定了静态资源的位置和url访问方式。即使不作任何设置，放在resources下面的public static 等文件夹的静态资源也可以被访问到。也可以在application.yml/application.properties文件内更改配置。具体有两个配置项目。
- spring.mvc.static-path-pattern：根据官网的描述和实际效果，可以理解为静态文件URL匹配头，也就是静态文件的URL地址开头。Springboot默认为：/**。
- spring.web.resources.static-locations：根据官网的描述和实际效果，可以理解为实际静态文件地址，也就是静态文件URL后，匹配的实际静态文件。Springboot默认为：classpath:/META-INF/resources/,classpath:/resources/,classpath:/static/,classpath:/public/


## 坑
我做的时候，给用成了spring.resources.static-locations，结果始终不生效！！！原来这个是老版本的配置。记录一下，高版本springboot用这个配置 ***spring.web.resources.static-locations***