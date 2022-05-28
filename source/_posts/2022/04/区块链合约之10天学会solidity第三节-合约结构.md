---
title: 区块链合约之10天学会solidity第三节-合约结构
date: 2022-04-29 08:40:30
tags:
- 合约
- 区块链
- solidity
- blockchain
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
在上一篇中初步认识了合约，下面看一下在合约中都有哪些重要的构成。这一节的目的是的对合约有一个大致的印象。不会说太多的细节，只是我们可以在宏观上知道合约的构成，在后面的学习中做到心中有数。

solidity中合约其他面向对象编程语言中的类比较类似。在其他编程语言中类声明，变量声明等特性在合约中大致都可以找到相关的影子。只不过是换了个名字而已。

合约中可以包含大致有状态变量、 函数、 函数修饰器、事件、 结构类型、枚举类型，并合约也有继承机制

## 状态变量
状态变量存储在内存中，按照java或者别的面向对象的语言来理解，可以理解为类的成员变量。
```
 string message; // 状态变量 ，消息
```


## 函数
函数是为了完成一些具有一定功能的代码的集合，可以被合约内部或者外部调用（注意函数的修饰符）
例如,下边是一个名称是setMessage的函数，接受一个string类型的参数，函数的可见性是public意味着在其他合约中也可以被调用
```
// 设置消息函数
    function setMessage(string amsg) public  isOwner {  
        message = amsg;
        emit MessageChanged(msg.sender,amsg);
    }

```


## 修饰器
如上面的函数。函数的调用必须受到isOwner的限制。如果不是owener调用函数，就会报错。

```
 // 修饰器，这里判断交易是否是合约创建者发起交易
    modifier isOwner() {
        require(msg.sender == owner, "Caller is not owner");
        _;
    }
```


## 合约事件
合约事件可以把合约运行过程中的事情给抛出去。关注这个事件的使用方可以关注并且根据事件做出自己的响应。
```
 // 定义事件
    event MessageChanged(address addr,string message);
```



## 最后
其实，合约如果理解为一个java或者c#或者php等面向对象语言中的一个类，就会好很多。这篇文章只是简单的列举了合约中的一些成员，函数，事件等。没有涉及的内容太多，这里我们对合约结构有一个大致印象即可。

下面是一个完整的代码 


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.4.0;


contract SimpleStorage   {
    string message; // 状态变量 ，消息
    address private owner;  // 状态变量 ，合约部署者

    // 定义事件
    event MessageChanged(address addr,string message);

    // 构造方法，部署合约的时候调用
    constructor() public {
        owner = msg.sender;
    }
    
    // 修饰器，这里判断交易是否是合约创建者发起交易
    modifier isOwner() {
        require(msg.sender == owner, "Caller is not owner");
        _;
    }

    // 设置消息函数
    function setMessage(string amsg) public  isOwner {  
        message = amsg;
        emit MessageChanged(msg.sender,amsg);
    }

    // 读取消息函数
    function getMessage() external  view  returns (string){
        return message;
    }

}
```


部署合约之后，我们就可以在remix ide里面调用 setMessage，然后调用 getMessage  就会看到我们刚才设置的 message 
![20220508214200](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220508214200.png)