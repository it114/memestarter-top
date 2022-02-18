---
title: flutter组件wdidget系列-概述
date: 2022-02-06 16:32:01
permalink:
tags:
    - flutter 
    - mac
    - android
    - widget
    - 组件 
categories:
    - 技术生活
      - 编程技术
        - 移动端研发
---

# Flutter 中万物皆为Widget
在Flutter中几乎所有的对象都是一个 widget ，Flutter 中的 widget 的概念很广泛，它不仅可以表示UI元素，也可以表示一些功能性的组件如：用于手势检测的 GestureDetector 、用于APP主题数据传递的 Theme，用于对其的center  等等。如果有web前端开发经验，对于理解这些还稍微好点。html前端开发中诸如div span p hr 等也都是标签。标签也就是描述UI 如何布局的，在flutter中，可以借鉴这个去理解。其实，在flutter中，这些widget就是一个描述和标签类似，真正的渲染的时候，flutter引擎会自动根据widget描述去绘制UI 。

## 一些要点
1. Key: 这个key属性类似于 React/Vue 中的key
   它的主要的作用是决定是否在下一次build时复用旧的 widget ，决定的条件在canUpdate()方法中。

## flutter中UI如何绘制出来的？
记清楚4棵树就好了
既然 Widget 只是描述一个UI元素的配置信息，那么真正的布局、绘制是由谁来完成的呢？Flutter 框架的的处理流程是这样的：

- 根据 Widget 树生成一个 Element 树，Element 树中的节点都继承自 Element 类。
- 根据 Element 树生成 Render 树（渲染树），渲染树中的节点都继承自RenderObject 类。
- 根据渲染树生成 Layer 树，然后上屏显示，Layer 树中的节点都继承自 Layer 类。
- 真正的布局和渲染逻辑在 Render 树中，Element 是 Widget 和 RenderObject 的粘合剂，可以理解为一个中间代理。 
  如下所示，屏幕真正显示出来的是layer树
  ![20220210134418](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220210134418.png)

## StatefulWidget 和 StatelessWidget
StatelessWidget ，普通的无关，不改变的UI界面描述。由于它不需要维护场景，所以，它的build方法直接返回widget
StatefulWidget，表示widget运行过程中可能会变化的UI页面描述。它有一个state对象。它有一个createState方法，用于创建和 StatefulWidget 相关的状态，它在StatefulWidget 的生命周期中可能会被多次调用。例如，当一个 StatefulWidget 同时插入到 widget 树的多个位置时，Flutter 框架就会调用该方法为每一个位置生成一个独立的State实例，其实，本质上就是一个StatefulElement对应一个State实例。
### state 
一个 StatefulWidget 类会对应一个 State 类，State表示与其对应的 StatefulWidget 要维护的状态，State 中的保存的状态信息可以：
- 在 widget 构建时可以被同步读取。
- 在 widget 生命周期中可以被改变，当State被改变时，可以手动调用其setState()方法通知Flutter 框架状态发生改变，
- Flutter 框架在收到消息后，会重新调用其build方法重新构建 widget树，从而达到更新UI的目的。

### state的声明周期方法
- initState
  当 widget 第一次插入到 widget 树时会被调用，对于每一个State对象，Flutter 框架只会调用一次该回调，所以，通常在该回调中做一些一次性的操作，如状态初始化、订阅子树的事件通知等
- didChangeDependencies()
  当State对象的依赖发生变化时会被调用；例如：在之前build() 中包含了一个InheritedWidget （第七章介绍），然后在之后的build() 中Inherited widget发生了变化，那么此时InheritedWidget的子 widget 的didChangeDependencies()回调都会被调用。典型的场景是当系统语言 Locale 或应用主题改变时，Flutter 框架会通知 widget 调用此回调。需要注意，组件第一次被创建后挂载的时候（包括重创建）对应的didChangeDependencies也会被调用。
- build()
  此回调读者现在应该已经相当熟悉了，它主要是用于构建 widget 子树的，会在如下场景被调用：
1.在调用initState()之后。
2.在调用didUpdateWidget()之后。
3.在调用setState()之后。
4.在调用didChangeDependencies()之后。
5.在State对象从树中一个位置移除后（会调用deactivate）又重新插入到树的其它位置之后。
- reassemble()
  此回调是专门为了开发调试而提供的，在热重载(hot reload)时会被调用，此回调在Release模式下永远不会被调用。
- didUpdateWidget ()
在 widget 重新构建时，Flutter 框架会调用widget.canUpdate来检测 widget 树中同一位置的新旧节点，然后决定是否需要更新，如果widget.canUpdate返回true则会调用此回调。正如之前所述，widget.canUpdate会在新旧 widget 的 key 和 runtimeType 同时相等时会返回true，也就是说在在新旧 widget 的key和runtimeType同时相等时didUpdateWidget()就会被调用
- deactivate()
当 State 对象从树中被移除时，会调用此回调。在一些场景下，Flutter 框架会将 State 对象重新插到树中，如包含此 State 对象的子树在树的一个位置移动到另一个位置时（可以通过GlobalKey 来实现）。如果移除后没有重新插入到树中则紧接着会调用dispose()方法。
- dispose()
当 State 对象从树中被永久移除时调用；通常在此回调中释放资源。
完整生命周期
![20220209211735](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220209211735.png)

###  自定义widget
StatelessWidget 和 StatefulWidget 都是用于组合其它组件的，它们本身没有对应的 RenderObject；StatelessWidget 和 StatefulWidget 相当于把其他带有 RenderObject的对象组织起来，真正的渲染对象完成UI的绘制。所以自定义widget就需要继承自 RenderObject。例如Text Row ...

如果组件不会包含子组件，则我们可以直接继承自 LeafRenderObjectWidget ，它是 RenderObjectWidget 的子类，而 RenderObjectWidget 继承自 Widget ，它的实现大致如下：
```
abstract class LeafRenderObjectWidget extends RenderObjectWidget {
  const LeafRenderObjectWidget({ Key? key }) : super(key: key);

  @override
  LeafRenderObjectElement createElement() => LeafRenderObjectElement(this);
}
```
其实，就是帮 widget 实现了createElement 方法，它会为组件创建一个 类型为 LeafRenderObjectElement 的 Element对象。如果自定义的 widget 可以包含子组件，则可以根据子组件的数量来选择继承SingleChildRenderObjectWidget 或 MultiChildRenderObjectWidget，它们也实现了createElement() 方法，返回不同类型的 Element 对象。


### flutter内置组件
Flutter 提供了一套丰富、强大的基础组件，在基础组件库之上 Flutter 又提供了一套 Material 风格（ Android 默认的视觉风格）和一套 Cupertino 风格（iOS视觉风格）的组件库。要使用基础组件库，需要先导入：
```
  import 'package:flutter/widgets.dart';
```
这些基础组件包括<Br>
- Text ：该组件可让您创建一个带格式的文本。
- Row 、 Column ： 这些具有弹性空间的布局类 widget 可让您在水平（Row）和垂直（Column）方向上创建灵活的布局。其设计是基于 Web 开发中的 Flexbox 布局模型。
- Stack  ： 取代线性布局 (译者语：和 Android 中的FrameLayout相似)，[Stack](https://docs.flutter.io/flutter/ widgets/Stack-class.html)允许子 widget 堆叠， 你可以使用 Positioned  来定位他们相对于Stack的上下左右四条边的位置。Stacks是基于Web开发中的绝对定位（absolute positioning )布局模型设计的。
- Container  ： Container 可让您创建矩形视觉元素。Container 可以装饰一个BoxDecoration  , 如 background、一个边框、或者一个阴影。 Container 也可以具有边距（margins）、填充(padding)和应用于其大小的约束(constraints)。另外， Container 可以使用矩阵在三维空间中对其进行变换。




