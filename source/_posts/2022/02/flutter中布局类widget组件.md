---
title: flutter中布局类widget组件
date: 2022-02-11 14:46:28
permalink:
tags:
  - flutter
  - 布局
  - widget
categories:
  - 技术生活
      - 编程技术
        - 移动端研发
---

# widget布局组件原理
flutter中布局从布局树的角度来说，大致分为三类：完全没有子节点，有且只有一个子节点的widget ，可以有多个子节点的widget。只有后两者才能成为布局widget。下班是这三类的说明和举例。
|widget名称        |  说明      |  举例      |
|:-------|:-------|:-------|
|  LeafRenderObjectWidget	      |  非容器类组件通常继承自这个      | Widget树的叶子节点，用于没有子节点的widget，通常基础组件都属于这一类，如Image。       |
| SingleChildRenderObjectWidget       |  只能容纳单个组件的widget的基类      |   只能有一个child Widget，如：ConstrainedBox、DecoratedBox等 MultiChildRenderObjectWidget    |包含多个组件

> 所有的widget都继承自 RenderObjectWidget，这个类中定义了创建、更新RenderObject的方法，子类必须实现他们，它是最终布局、渲染UI界面的对象的方法，其布局算法都是通过对应的RenderObject对象来实现的。


# widget布局组件的两个形式
Flutter 中的整体渲染流程是 Widget -> Element -> RenderObejct -> Layer 这样的过程，而 「Flutter 里的布局和绘制逻辑都在 RenderObejct」,而其中的布局，就是这里要说的。<Br>
Flutter 中有两种布局模型：

- 基于 RenderBox 的盒模型布局。
- 基于 Sliver ( RenderSliver ) 按需加载列表布局。
两种布局方式在细节上略有差异，但大体流程相同，布局流程如下：

1. 上层组件向下层组件传递约束（constraints）条件。
2. 下层组件确定自己的大小，然后告诉上层组件。注意下层组件的大小必须符合父组件的约束。
2. 上层组件确定下层组件相对于自身的偏移和确定自身的大小（大多数情况下会根据子组件的大小来确定自身的大小）。
   


## 盒模型布局组件
盒模型布局组件有两个特点：
1. 组件对应的渲染对象都继承自 RenderBox 类。
2. 在布局过程中父级传递给子级的Constraints（约束） BoxConstraints 描述。

ConstrainedBox用于对子组件添加额外的约束。被ConstrainedBox约束的组件，子组件的宽高以ConstrainedBox中设置的为准
例如以下代码
```
    ConstrainedBox(
                  constraints:
                      const BoxConstraints(maxWidth: 300, minHeight: 40),
                  child: Container(
                    height: 1,
                    decoration: const BoxDecoration(color: Colors.red),
                  )),
```

![20220211234015](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220211234015.png)

container的高度设置为1实际绘制的高度是40；设置height为100 ，绘制高度也是100 

SizedBox用于给子元素指定固定的宽高。

### Row和Column
这两个组件比较简单，可以类比安卓开发的时候有一个线性布局。
构造方法：
```
Row({
  ...  
  TextDirection textDirection,    
  MainAxisSize mainAxisSize = MainAxisSize.max,    
  MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,
  VerticalDirection verticalDirection = VerticalDirection.down,  
  CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,
  List<Widget> children = const <Widget>[],
})

```
- TextDirection 组件排列方向
- mainAxisSize 子元素的尺寸。mainAxisSize.max 尽可能的暂用空间。mainAxisSize.min尽可能的小的去占用。
- mainAxisAlignment ：水平空间的对齐方式。MainAxisAlignment.start表示沿textDirection的初始方向对齐，如textDirection取值为TextDirection.ltr时，则MainAxisAlignment.start表示左对齐，textDirection取值为TextDirection.rtl时表示从右对齐。而MainAxisAlignment.end和MainAxisAlignment.start正好相反；MainAxisAlignment.center表示居中对齐。读者可以这么理解：textDirection是mainAxisAlignment的参考系。
- verticalDirection：表示Row纵轴（垂直）的对齐方向，默认是VerticalDirection.down，表示从上到下。
- crossAxisAlignment：表示子组件在纵轴方向的对齐方式，Row的高度等于子组件中最高的子元素高度，它的取值和MainAxisAlignment一样(包含start、end、 center三个值)，不同的是crossAxisAlignment的参考系是verticalDirection，即verticalDirection值为VerticalDirection.down时crossAxisAlignment.start指顶部对齐，verticalDirection值为VerticalDirection.up时，crossAxisAlignment.start指底部对齐；而crossAxisAlignment.end和crossAxisAlignment.start正好相反；


### 柔性布局Flex
柔性布局和Expand结合可以把元素按照比例分割布局。

### 流式布局
超出屏幕显示范围会自动折行的布局称为流式布局。Flutter中通过Wrap和Flow来支持流式布局
wrap组件重要的属性
- spacing：主轴方向子widget的间距
- runSpacing：次轴方向的间距
- runAlignment：次轴方向的对齐方式
  
### 绝对布局Stack和Positioned
绝对布局、Android 中的 Frame 布局是相似的。子组件可以根据距父容器四个角的位置来确定自身的位置。Stack允许子组件堆叠，而Positioned用于根据Stack的四个角来确定子组件的位置。

stack重要属性
- alignment:Alignment.center , //指定未定位或部分定位widget的对齐方式；这个决定原点
- fit: StackFit.expand, //未定位widget占满Stack整个空间

Positioned构造函数
```
const Positioned({
  Key? key,
  this.left, 
  this.top,
  this.right,
  this.bottom,
  this.width,
  this.height,
  required Widget child,
})

```

left、top 、right、 bottom分别代表离Stack左、上、右、底四边的距离。width和height用于指定需要定位元素的宽度和高度。


## 容器类组件
Padding
可以给其子节点添加填充（留白），和边距效果类似。
EdgeInsets
**EdgeInsets提供的api** 
- fromLTRB(double left, double top, double right, double bottom)：分别指定四个方向的填充。
- all(double value) : 所有方向均使用相同数值的填充。
- only({left, top, right ,bottom })：可以设置具体某个方向的填充(可以同时指定多个方向)。
- symmetric({ vertical, horizontal })：用于设置对称方向的填充，vertical指top和bottom，horizontal指left和right。

### DecoratedBox
可以在其子组件绘制前(或后)绘制一些装饰（Decoration），如背景、边框、渐变等。


### Container
Container是一个组合类容器，它本身不对应具体的RenderObject，它是DecoratedBox、ConstrainedBox、Transform、Padding、Align等组件组合的一个多功能容器，所以我们只需通过一个Container组件可以实现同时需要装饰、变换、限制的场景。
```
Container({
  this.alignment,
  this.padding, //容器内补白，属于decoration的装饰范围
  Color color, // 背景色
  Decoration decoration, // 背景装饰
  Decoration foregroundDecoration, //前景装饰
  double width,//容器的宽度
  double height, //容器的高度
  BoxConstraints constraints, //容器大小的限制条件
  this.margin,//容器外补白，不属于decoration的装饰范围
  this.transform, //变换
  this.child,
  ...
})
```









#




## Sliver(薄片布局)
通常可滚动组件子组件比较多，如果一次性渲染和加载所有组件，需要消耗比较大的系统资源，严重影响系统性能。并且随着手势滑动的时候，需要不停的计算。Flutter中提出一个Sliver（中文为“薄片”的意思）概念，Sliver 可以包含一个或多个子组件。Sliver 的主要作用是配合：加载子组件并确定每一个子组件的布局和绘制信息，如果 Sliver 可以包含多个子组件时，通常会实现按需加载模型。<br>
通过这个机制，只有当 Sliver 出现在视口中时才会去构建它，这种模型也称为“基于Sliver的列表按需加载模型”。可滚动组件中有很多都支持基于Sliver的按需加载模型，如ListView、GridView，但是也有不支持该模型的，如SingleChildScrollView。
>Flutter 中的可滚动主要由三个角色组成：Scrollable、Viewport 和 Sliver：
>
>1. Scrollable ：用于处理滑动手势，确定滑动偏移，滑动偏移变化时构建 Viewport 。
2. Viewport：显示的视窗，即列表的可视区域；
3. Sliver：视窗里显示的元素。

这几个的关系大致如下
![20220211162027](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220211162027.png)
说明：<br>
1.  Scrollable 、 Viewport 和 Sliver 所占用的空间都是白色区域，也就是说，这部分是重合的。
2.  从上到下的层级关系是scrollview ，viewPort  ，silver 
3.  按需加载需要最下层的silver来实现
4.  图中的cacheExtent区域是为了让滑动更丝滑而存在的，这部分不显示在可见区域，只是在即将出现在可见区域的提前绘制。默认值是250大小。可以被改变

绘制过程
具体布局过程：
1. Scrollable 监听到用户滑动行为后，根据最新的滑动偏移构建 Viewport 。
2. Viewport 将当前视口信息和配置信息通过 SliverConstraints 传递给 Sliver。
3. Sliver 中对子组件（RenderBox）按需进行构建和布局，然后确认自身的位置、绘制等信息，保存在 geometry 中（一个 SliverGeometry 类型的对象）。
### SingleChildScrollView 
SingleChildScrollView类似安卓的ScrollView ，只有一个子组件。需要注意的是SingleChildScrollView不支持silver的按需加载。如果有比较多的待滑动内容的时候，不要使用这个组件。

# 滑动组件
## ListView
### ListView是一个列表展示组件。它的构造方法可以传入一组widget 
如下
```
ListView(
                  shrinkWrap: true,
                  padding: const EdgeInsets.all(10),
                  children: const [
                    Text("data1"),
                    Text("data2"),
                    Text("data3"),
                    Text("data4"),
                    Text("data5"),
                    Text("data6"),
                    Text("data7"),
                    Text("data8"),
                    Text("data9"),
                    Text("data10"),
                  ])
            ]),
```
通常如果列表数量比较少，可以这么样做。如果列表数量比较多，这样就会消耗性能。

![20220211170603](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220211170603.png)

### ListView.builder适合批量创建列表元素


```
Expanded(
                  child: ListView.builder(
                      itemCount: 100,
                      itemExtent: 60,
                      itemBuilder: (BuildContext context, int index) {
                        return ListTile(title: Text("ListViwe $index"));
                      }))
 ```
 ![20220211171356](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220211171356.png)                     

### ListView.separated
ListView.separated可以在生成的列表项之间添加一个分割组件，它比ListView.builder多了一个separatorBuilder参数，该参数是一个分割组件生成器。使用场景，例如需要在每一个item之间添加分隔条的时候。

### ListVIew的性能
1. 如果列表数据多，尽量使用listVIew.build或者listVIew.separated来构建列表。
2. 尽量给列表指定 itemExtent 或 prototypeItem ，这样会减少引擎的计算时间

## GridVIew
GridView 有着非常有用的应用场景。如下
![20220211174838](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220211174838.png)

GridView构造方法
```
GridView({
  Key key,
  Axis scrollDirection = Axis.vertical,
  bool reverse = false,
  ScrollController controller,
  ScrollPhysics physics,
  bool shrinkWrap = false,
  EdgeInsetsGeometry padding,
  @required this.gridDelegate,
  double cacheExtent,
  List<Widget> children = const <Widget>[],
})
```
这么多参数中，重点需要关注的是gridDelegate这个参数。它其实是GridView组件如何控制排列子元素的一个委托
他的的类型是SliverGridDelegate。fultter中主要由两个实现类：

1. SliverGridDelegateWithFixedCrossAxisCount：用于固定列数的场景；
2. SliverGridDelegateWithMaxCrossAxisExtent：用于子元素有最大宽度限制的场景；

### SliverGridDelegateWithFixedCrossAxisCount
主要用于横轴方向有固定元素的场景，构造方法
```
SliverGridDelegateWithFixedCrossAxisCount({
  @required this.crossAxisCount,
  this.mainAxisSpacing = 0.0,
  this.crossAxisSpacing = 0.0,
  this.childAspectRatio = 1.0,
})
```
参数解释
- crossAxisCount：列数，即一行有几个子元素；
- mainAxisSpacing：主轴方向上的空隙间距；这里主轴是x轴
- crossAxisSpacing：次轴方向上的空隙间距；这里次轴是y轴
- childAspectRatio：子元素的宽高比例。

下图说的比较清楚
![20220211175521](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220211175521.png)



### 其他
GridView使用方法主要有以下几种
- GridView默认构造函数可以类比于ListView默认构造函数，适用于有限个数子元素的场景，因为GridView组件会一次性全部渲染children中的子元素组件；
- GridView.builder构造函数可以类比于ListView.builder构造函数，适用于长列表的场景，因为GridView组件会根据子元素是否出现在屏幕内而动态创建销毁，减少内存消耗，更高效渲染；
- GridView.count构造函数是GrdiView使用SliverGridDelegateWithFixedCrossAxisCount的简写（语法糖），效果完全一致；
- GridView.extent构造函数式GridView使用SliverGridDelegateWithMaxCrossAxisExtent的简写（语法糖），效果完全一致。

