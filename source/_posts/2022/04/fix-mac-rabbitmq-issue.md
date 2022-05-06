---
title: fix-mac-rabbitmq-issue
date: 2022-04-19 20:27:21
permalink:
tags:
    - rabbmitmq
categories:
- 技术生活
  - 编程技术
    - 后端技术
---


## 问题描述
今天在mac 下面配置 rabbitmq的环境，按照官方文档 brew install rabbitmq 竟然报错了，如下。
核心错误 *** No formulae found in taps *** 


```
yyy@xxx-MacBook-Pro ~ % brew install rabbitmq

Running `brew update --preinstall`...
fatal: Could not resolve HEAD to a revision
==> Tapping homebrew/cask
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask'...
remote: Enumerating objects: 633392, done.
remote: Counting objects: 100% (14/14), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 633392 (delta 6), reused 2 (delta 0), pack-reused 633378
Receiving objects: 100% (633392/633392), 298.02 MiB | 9.22 MiB/s, done.
Resolving deltas: 100% (448299/448299), done.
Updating files: 100% (4055/4055), done.
Tapped 3996 casks (4,066 files, 318.3MB).
Warning: No available formula with the name "rabbitmq".
==> Searching for similarly named formulae...
Error: No similarly named formulae found.
==> Searching for a previously deleted formula (in the last month)...
Error: No previously deleted formula found.
==> Searching taps on GitHub...
Error: No formulae found in taps.
jim@zhangs-MacBook-Pro ~ % brew install rabbitmq

Running `brew update --preinstall`...
fatal: Could not resolve HEAD to a revision
Warning: No available formula with the name "rabbitmq".
==> Searching for similarly named formulae...
Error: No similarly named formulae found.
==> Searching for a previously deleted formula (in the last month)...
Error: No previously deleted formula found.
==> Searching taps on GitHub...
Error: No formulae found in taps.
```

## 解决方案
经过一番思考和查阅发现可以这样解决

- 定位到homebrew目录
```
cd /usr/local/Homebrew/Library/Taps/homebrew/
```
- 然后rm -rf homebrew-core
- 重新git clone https://github.com/Homebrew/homebrew-core.git
- 最后重新执行brew install


## 问题原因
可能是安装brew的过程中由于网络原因或者其他原因导致homebrew安装出现问题

## rabbitmq 其他
- rabbitmq常用命令：
```
brew services info   rabbitmq
brew services stop   rabbitmq
brew services restart   rabbitmq
brew services kill   rabbitmq
```
- Web界面管理RabbitMQ
默认是可以本地登录localhost:15672，用户名：guest；密码：guest；端口默认15672
