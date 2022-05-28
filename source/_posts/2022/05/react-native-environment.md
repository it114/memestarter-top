---
title: react-native-environment
date: 2022-05-23 14:23:16
permalink:
tags:
    - react 
categories:
- 技术生活
  - 编程技术
    - 前端
---

## 前置条件
由于react本质上是一套前端的开发流程，所以类似node ，npm等是必不可少的
```
brew install node
brew install watchman
```
其中 watchman是用来监测文件变化的。



## 安装 
这里区分ios和安卓开发环境。最简单的，安卓安装android studio，ios就安装xcode ~ 如果在遇到其他问题，搜一下，一下子就出来了。


## 创建项目
```
npx react-native init AwesomeProject
```

## 运行项目
在项目根目录运行
```
npx react-native start
```
这个命令把项目用到的所有依赖和代码合并成一个js文件。

### 安卓平台
运行
```
npx react-native run-android
```
安卓平台还可以打开Android studio，然后导入代码目录下面的android文件夹编译，安装。

如果报错，在命令行输入命令
```
adb reverset tcp:8081 tcp:8081 
```
这句的意思是把电脑的8081端口的内容转发给手机端8081端口

### ios平台
打开ios目录下的workspace，运行就很简单了，这里提一下签名。
1. 必须使用苹果官方数据线。否则无法识别设备
2. 需要一个mobileprovision文件，这是一个描述文件。他包含的信息说明使用什么证书打包、AppID是什么、打包的App包含了哪些功能、可以在哪些设备上安装，则是通过Provisioning Profile 描述文件。
 
 


