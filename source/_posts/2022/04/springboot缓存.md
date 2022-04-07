---
title: springboot缓存
date: 2022-04-07 11:27:21
permalink:
tags:
    - 技术
    - 编程
    - springboot
    - 缓存
categories:
  - 技术生活
      - 编程技术
        - 后端技术
---

## springboot缓存框架
springboot的缓存框架对市面上的缓存进行了抽象，使我们可以很方便的集成和使用。它的架构大致如下
![20220407123350](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220407123350.png)
Springboot实现了jsr的标准。
> Java Caching（JSR-107）定义了5个核心接口，分别是CachingProvider, CacheManager, Cache, Entry和 Expiry。
CachingProvider：创建、配置、获取、管理和控制多个CacheManager
CacheManager：创建、配置、获取、管理和控制多个唯一命名的Cache，Cache存在于CacheManager的上下文中。一个CacheManager仅对应一个CachingProvider
Cache：是由CacheManager管理的，CacheManager管理Cache的生命周期，Cache存在于CacheManager的上下文中，是一个类似map的数据结构，并临时存储以key为索引的值。一个Cache仅被一个CacheManager所拥有
Entry：是一个存储在Cache中的key-value对
Expiry：每一个存储在Cache中的条目都有一个定义的有效期。一旦超过这个时间，条目就自动过期，过期后，条目将不可以访问、更新和删除操作。缓存有效期可以通过ExpiryPolicy设置

**这样做的好处是做到了统一管理缓存，是的框架的接口和实现分离。如果要切换缓存实现，成本也很低。**

## 重要的接口以及注解
###  重要接口
- Cache：缓存抽象的规范接口，缓存实现有：RedisCache、EhCacheCache、ConcurrentMapCache等
- CacheManager：缓存管理器，管理Cache的生命周期

### 重要注解
- @Cacheable
添加在方法上,根据条件添加缓存

value:  缓存的名称,不能为空
cacheNames:  缓存的名称,和value二选一
key:  缓存的key,默认为空,表示使用方法的参数类型及参数值作为key,支持SpEL
keyGenerator:  指定key的生成策略(和key不能共存)
cacheManager:  指定缓存管理器
cacheResolver:  指定获取解析器
condition:  条件符合则缓存
unless:  条件符合则不缓存
sync:  是否使用异步模式 默认false

- @CachePut
添加在方法上,根据条件添加缓存
@CachePut作用和@Cacheable类似,区别是 ***@Cacheable没有缓存数据会执行方法后,把结果缓存起来,第二次调用方法不执行方法,直接从缓存中获取数据并返回.@CachePut每次都会执行方法,不管缓存中有没有数据,都会把结果缓存.*** 

- @CacheEvict
添加在方法上,根据条件清空缓存.
value:  缓存名称,不能为空
cacheNames: 缓存的名称,与value二选一
keyGenerator:  key的生成器。key/keyGenerator二选一使用
condition:  触发条件,支持SpEL
allEntries:  true表示清除value中的全部缓存,默认为false
beforeInvocation:  是否在方法执行前就清空 默认false
cacheManager:  指定缓存管理器
cacheResolver:  或者指定获取解析器

- @CacheConfig
作用在类上,为本类的缓存注解配置全局属性
cacheNames:  缓存名称
keyGenerator:  key的生成器
cacheManager:  缓存管理器
cacheResolver:  获取解析器


- @EnableCaching： 开启缓存

## 注意点
1. Spring boot默认使用的是SimpleCacheConfiguration，即使用ConcurrentMapCacheManager来实现缓存，ConcurrentMapCache实质是一个ConcurrentHashMap集合对象java内置，所以无需引入其他依赖，也没有额外的配置
2. Caffeine是使用Java8对Guava缓存的重写版本，在Spring Boot 2.0中将取代，基于LRU算法实现，支持多种缓存过期策略
3. StringRedisTemplate默认采用的是String的序列化策略，保存的key和value都是采用此策略序列化保存的。（JdkSerializationRedisSerializer）RedisTemplate默认采用的是JDK的序列化策略，保存的key和value都是采用此策略序列化保存的。（StringRedisSerializer)
4. 如果使用redis可以配置如下依赖
```
<!-- redis starter -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!-- lettuce pool 缓存连接池 -->
<dependency>
	<groupId>org.apache.commons</groupId>
	<artifactId>commons-pool2</artifactId>
</dependency>
```

>不再需要引入其他依赖,SpringBoot导入spring-boot-starter-data-redis时, CacheManager默认使用RedisCache.


否则可以配置如下依赖
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```
5. 在不指定缓存 key 属性的情况下，默认使用 SimpleKeyGenerator 生成 key。除此之外，我们也可以自定义实现 KeyGenerator 接口，生成自己的 key 名称策略。