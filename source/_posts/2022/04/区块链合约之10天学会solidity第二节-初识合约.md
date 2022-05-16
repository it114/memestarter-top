---
title: 区块链合约之10天学会solidity第二节-初识合约
date: 2022-04-30 18:44:32
permalink:
tags:
- 合约
- 区块链
- solidity
- 教程
categories:
- 技术生活
  - 编程技术
    - 区块链
---

# 前言
学solidity已经2年了，平日除了偶尔写一点点合约喝读一点合约之外，几乎没有用武之地（工作中也用不到）。导致目前渐渐的有些生疏了。现在捡起来，希望有人一起再次一起学习。学习学习，有输入和输出才能学的更好。所以，我计划，没两天更新一波学习记录。我会把学习记录写成系列文章，把代码放到github，期待一起进步。也请大家到我的GitHub 给我star呀！也欢迎给我留言加个好友啥的。如果给我star的，联系我我会拉个共同学习的群v（jingyiwei）。
GitHub：git@github.com:it114/learn-solidity-30day.git
本文原文发表在张智进博客：https://www.memestarter.top/

# 合约结构
## 最简单的合约
下面来看一段最简单的合约

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.4.0;
contract SimpleStorage {
    string message;
}
```
分开来看
- 第一行，代表合约所使用的许可证。不写也无所谓，不会报错；同时使用//符号注释这一行，代表这一行不会被编译器编译
- 第二行，pragma 编译器指令 告诉编译器 合约使用 solidity编程语言，最低只是的solidity语言版本是 0.4.0。需要注意的是，pragma指令只对当前的文件起作用。^0.4.0 表示可以使用的版本为 0.4.0-0.4.9  之间的任意版本，但不能超过 0.5.0 版本。0.4.0-0.4.9 之间的小版本改动通常不会有破坏性更改，源代码应该都是兼容的。
- 第三行，创建了一个名字为 "SimpleStorage"的合约。
- 第四行，合约中，定义了一个变量message

## 合约编译
![20220506180910](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220506180910.png)

## 合约部署
![20220506181410](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220506181410.png)


