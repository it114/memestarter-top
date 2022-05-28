---
title: 区块链合约之10天学会solidity第一节-remix ide
date: 2022-04-28 18:11:20
permalink:
tags:
    - 区块链
    - blockchain
    - ide
    - solidity教程
    - 教程
    - 合约
categories:
- 技术生活
  - 编程技术
    - 区块链
---

# 前言
学solidity已经2年了，平日除了偶尔写一点点合约喝读一点合约之外，几乎没有用武之地（工作中也用不到）。导致目前渐渐的有些生疏了。现在捡起来，希望有人一起再次一起学习。学习学习，有输入和输出才能学的更好。所以，我计划，没两天更新一波学习记录。我会把学习记录写成系列文章，把代码放到github，期待一起进步。也请大家到我的GitHub 给我star呀！也欢迎给我留言加个好友啥的。如果给我star的，联系我我会拉个共同学习的群v（jingyiwei）。
GitHub：git@github.com:it114/learn-solidity-30day.git
本文原文发表在张智进博客：https://www.memestarter.top/


# 合约ide
和很多编程语言一样，当你问学过的人该用什么ide来开发。清高的人活着xx的人会告诉你：记事本也可以。对于新手来说，一款好的编译器能帮助初学者少走弯路，建立信心。回忆起我的初学之后，从最初的vs code 到 https://remix.ethereum.org/ 在线ide，再到remix 桌面ide，最后还是觉得桌面ide要要用一些。说一下优缺点吧 
- VS code
需要安装插件，而且这个插件还有不同的开发者开发的插件，初学者要去选择。代码变现到编译到部署，中间要用到几个插件，都需要要初学者去探索。
- remix 在线ide 
打开网址就能编写代码，同一个ide，几乎啥都不配置，可以执行编写，编译和部署等诸多步骤。上手快，无需下载，用完即走
- remix 桌面ide
和在线的remix一样，不一样的是可以打开本地workspace还有一些高级功能待探索。


# remix
## 认识工作区
工作区大致如下，各个区域的功能
![20220430183656](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220430183656.png)

在编译页面，重要的配置项
![20220430183827](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220430183827.png)

部署页面重要配置项
![20220430184102](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220430184102.png)


第一段就到这了，需要多练多写