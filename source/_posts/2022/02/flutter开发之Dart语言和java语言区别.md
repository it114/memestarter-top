---
title: flutter开发之Dart语言和java语言区别
date: 2022-02-08 19:08:00
permalink:
tags:
    - flutter 
    - mac
    - android
    - 语法
    - dart 
categories:
    - 技术生活
      - 编程技术
        - 移动端研发
---

# 为何要说java语法
当然了，编程语言都是互通的；如果会java或者其他语法学起来会相对轻松。由于之前对java语法比较熟悉。这里就和java做一个对比


# java语法和dart语法的对比和区别

1. 主函数
   - 没有public static 
   - 命令参数List<String> args
   函数体类似
   ```
    void main() {
    }
    ```
2. 没有public, private, protected关键字
3. 创建对象，new可选
4. class中属性默认public，若声明私有，只需在属性名前加_
5. getter/setter方法
java中是有函数定义getter和setter的，dart通过关键字*get*  *set* 来声明，如下
```
//返回值类型/get/外部可访问属性/方法体
int get speed => _speed
```
6. 变量必须初始化，未初始化的变量值均为null
7. 字符串可用单引号或者双引号，这点和JavaScript又有点类似
8. dart中没有interface关键字，每个类都可以做接口
9. 函数无需声明可能抛出的异常类型，java中需要用关键字*throws*声明
10. 捕获异常的时候dart有一个*on*关键字
```
try {
  // ···
} on Exception catch (e) {
  print('Exception details:\n $e');
} catch (e, s) {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
```
11. dart语言是前面加一个下划线来实现变量或者方法为私有属性或者私有方法。
12. dart所独有的特征:mixins <br>
mixins(混入)的定义是
> Mixins are a way of reusing a class’s code in multiple class hierarchies.
从目的看Mixins要解决的就是代码复用的问题
维基百科的解释
> 在面向对象的语言中,mixins类是一个可以把自己的方法提供给其他类使用，但却不需要成为其他类的父类。
```
**注意**
- mixins类只能继承自object
- mixins类不能有构造函数
- 一个类可以mixins多个mixins类
- 可以mixins多个类，不破坏Flutter的单继承
```
1.  变量的定义
    1. var用来定义类型不变的变量。
    ```
    var x = 10;//x是整数，之后x就不能赋值其他类型，这个和java一样
    ```
    1. dynamic和object
DART语言中 所有对象包括Function和null 都是object的子类型。这两个声明的变量，在后边可以改变类型。
二者的区别在于，dynamic声明的变量在编写代码的时候，可以动态的去写，编译器会默认你有这个变量或者方法，而不去报错。基于这个特点，就需要小心使用。如下
```
dynamic a;
 Object b = "";
 main() {
   a = "";
   printLengths();
 }   

 printLengths() {
   // 正常
    print(a.length);
   // 报错 The getter 'length' is not defined for the class 'Object'
   print(b.length);
 }
```
>例子引用：`https://book.flutterchina.club/chapter1/dart.html#_1-4-1-%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E`

13. final 和const 修饰的变量
两个都可以修饰变量，而且修饰的变量在运行时不可以改变。区别在于final修饰的值在定义变量的时候已经知道了变量的值。但是，const修饰的值在编译器运行的时候是可以计算的。例如
```
const bar = 1000000;       // 定义常量值
// bar =13;   // 出现异常，const修饰的变量不能调用setter方法，即：不能设值，只能在声明处设值
const atm = 1.01325 * bar; // 值的表达式中的变量必须是编译时常量（bar）;

```
14. dart中的函数
    1. 包装一组函数参数，用[]标记为可选的位置参数，并放在参数列表的最后面。例如
    ```
        String say(String from, String msg, [String device]) {
            ///
        }
    ```
    2. 可选的命名参数
    函数定义：
    ```
        void testFunction({string params1, bool params2}) {
            // ... 
        }
    ```
    函数使用,必须指定变量名称
    ```
        testFunction(params1: "1111", params2: true);
    ```

      