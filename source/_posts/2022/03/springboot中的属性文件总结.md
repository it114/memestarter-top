---
title: springboot中的属性文件总结
date: 2022-03-22 22:14:35
permalink:
tags:
    - springboot
    - 配置文件
    - 优先级
categories:
- 技术生活
    - 编程技术
      - 后端技术
---

## 属性文件放在哪里
我们知道springboot的属性文件一般是以.properties结尾或者以ymal结尾。至于这两种配置方式的优劣，仁者见仁智者见智了。但是，他们在某些场景的特点还是要注意以下的。那就是yaml中配置的属性是有顺序的，properties中配置的文件是没有顺序的，这个时候如果您的应用程序对属性的加载顺序有要求，这个时候使用yaml配置文件为佳。

一般情况下，大概有4个地方可以放置属性文件。
1. 项目根目录下的 config 目录下
2. 项目的根目录下的application.properties文件
3. 项目src下面的 resources 目录下的 config 目录下
4. 项目src下面的 resources 目录下的application.properties文件


## 属性文件的优先级
如下图，这几个位置的优先级，1>2>3>4 ，所以，如果您有属性需要全局覆盖，就可以在项目根目录下面的 config文件夹下面设置
![20220322222034](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220322222034.png)

## 属性文件的依赖注入
由于springboot 包含了spring的特性。回顾一下，spring中怎么注入属性的。假设我们有一个server.properties的配置文件需要配置到项目的中。
属性文件server.properties
```
    site.name=测试站点
    site.url=http://localhost/
```

java接受属性文件的bean
```
@Component
@PropertySource("classpath:site.properties")
class Site {
    @Value("${site.name}")
    String name;
    @Value("${site.url}")
    String url;
}
```

注意：这里
1. Site类使用了 Component注解，这个代表这个类要交个容器托管，这样，属性才可以注入。
2. 使用了@Value注解，使用了 server.properties 中的字段的全拼去引用 属性
3. 使用了PropertySource引用自己定义的属性名字的属性文件。


没问题，启动springboot在别的地方调用site即可获取属性

## 属性文件的安全性

上面的例子中，每个属性都要调用全拼属性的名字来引用，如果有非常多的属性怎么办？？？？？别着急springboot也想到了

````
@Component
@PropertySource("classpath:site.properties")
@ConfigurationProperties(prefix = "site")
class Site {
    String name;
    String url;
}
```

这里和上面的例子比起来，没有使用@Value注解来获取属性，极大降低了工作量和出错的概率。而这一期都是 @ConfigurationProperties的 功劳。