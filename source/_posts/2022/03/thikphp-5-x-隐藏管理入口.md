---
title: thikphp-5.x-隐藏管理入口
date: 2022-03-25 10:37:23
permalink:
tags:
    - thinkphp
    - 问题解决
categories:
- 技术生活
    - 编程技术
      - php
---

# 面临黑客的突然袭击

在后台看日志，发现有人不停的扫描服务器可用的url，如果一旦被发现admin这个后台管理入口，就后果很严重了。

# 应对策略
隐藏后台服务器管理的 url 入口。

# 设置隐藏后台管理地址
- 打开conig/app.php,app.php中设置拒绝直接通过模块名访问模块
```
'deny_module_list'       => ['common', 'admin']

```

- 新增后台入口文件，绑定模块
```
Container::get('app')->bind('admin')->run()->send();
```

- public目录下新建一个入口文件
> 入口文件内容和index.php内容一致即可。文件名字随便起，越奇怪越好。例如*** 0o98dfsd-fffP.php *** 


OVER 
