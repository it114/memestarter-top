---
title: update-ganache-from-less-than-v6.x.x-to-v7.x.x
date: 2022-06-01 02:19:01
permalink:
tags:
- 区块链
- blockchain
- ganache
categories:
- 技术生活
  - 编程技术
    - 区块链
---

# ganache不保存数据
每次重启，账户都被情况，数据，交易全部都没了，每次重启reset了db。不想要这个效果，从文档看传入-db参数可以解决这个问题。无奈，这个V6.x.x有bug ，需要v7.x.x才行。


# 升级
v7.x.x的版本变化比较大
```
The Ganache npm packages have been combined and renamed from ganache-cli and ganache-core to ganache. You may need to uninstall the old packages before installing the new ganache.
```

如果升级需要删除老版本
```
npm uninstall --global ganache-cli ganache-core
npm install --global ganache
```

命令行参数也改了
```
x@y-MacBook-Pro ~ % ganache
ganache v7.2.0 (@ganache/cli: 0.3.0, @ganache/core: 0.3.0)
Starting RPC server
```

参考：https://github.com/trufflesuite/ganache/blob/develop/UPGRADE-GUIDE.md

# 最好的解决方案
升级之后，还是有问题。可能是我配置有问题，ganache命令行应该就是为了内存设计。如果为了缓存数据，可以用ganache 的 UI 版本。
![20220601024732](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220601024732.png)
Remix 添加ganache的网络
![20220601024705](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220601024705.png)

ganache UI 版本有一个workspace概念，可以很好的隔离保存每次不同的数据区块