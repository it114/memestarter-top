---
title: https-ssl-403-error
date: 2022-06-24 00:55:31
permalink:
tags:
- 问题记录
categories:
- 技术生活
  - 编程技术
    - 运维
---

# 问题描述
由于用到小程序，需要配置ssl服务器；因为之前配置了好多次，以为这次这个事情也就10分钟的事情，谁知道花费了几个小时。按照正常流程申了ssl证书配置了域名和nginx ，当访问https://打开网站的的时候，莫名的打不开了。。。就以为是nginx配置问题。疯狂的找资料。因为服务器面板和云服务器443端口都开了，所以我就没往这里想。


# 问题解决

充分尝试之后，还是不行，之后，我一个一个的排查，先关闭防火墙；结果。。。。有反应了，https 出现了 403 ，巨大的进步。但是这一步怎么也解决不了。后来发现nginx文件没有配置默认的入口文件。


# 经验

遇到问题多方思考。最熟悉的地方👄容易忽略。
分享一个nginx配置文件生成网址，还挺有用的，可以根据nginx版本 ，ssl版本和其他一些方面，动态生成nginx配置还挺有用。
https://ssl-config.mozilla.org/#server=nginx&version=1.20.2&config=intermediate&openssl=1.1.1k&hsts=false&ocsp=false&guideline=5.6

