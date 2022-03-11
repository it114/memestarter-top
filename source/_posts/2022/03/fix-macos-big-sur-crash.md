---
title: fix-macos-big-sur-crash
date: 2022-03-02 21:03:20
permalink:
tags:
    - maos
    - big sur
    - crash 
    - idea
    - intelj
categories:
    - 技术生活
      - 编程技术
        - bug-fix
---

# 问题描述
今天本来是用vscode写一个spring的小程序，编译的时候一直提示没有compile provider，百思不得其解。初步怀疑是maven和vscode的配置有关系。但是多番尝试依然无果。为了不浪费时间，还是决定intelj来开发，好久没有打开过intelj了，谁知道一打开就crash....今天真的是见了鬼。
  
# 解决思路
    1. 一番观察进程之后，排除了我下载的破解插件的原因。
    2. 软件兼容性方面，电脑自从上次没有用idea 大概没有安装太多第三方软件，初步排除
    3. 查看是否是因为系统资源不够等原因的时候，查看到mac os 系统版本，最近曾经升级过....

茅塞顿开！！！

# 解决方案
参考intelj 官方的答案：https://www.jetbrains.com/help/idea/directories-used-by-the-ide-to-store-settings-caches-plugins-and-logs.html#config-directory

注意mac上如果直接删除参考如下
```
ls -al ~/Library/Application\ Support/JetBrains   
```
先查看自己的版本，然后决定是否备份一下配置文件。之后执行删除 **JetBrains** 下面的相关所有文件即可