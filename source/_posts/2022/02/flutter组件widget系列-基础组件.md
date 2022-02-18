---
title: flutter组件widget系列-基础组件
date: 2022-02-10 17:10:43
permalink:
tags:
  - widget
  - flutter
  - Button 
  - Image
  - 组件
categories:
  - 技术生活
      - 编程技术
        - 移动端研发    
---

# 文本组件
## 文本组件基本属性
Text是最基本的文本组件，它的构造方法如下
```
const Text(
    String this.data, {
    Key? key,
    this.style,
    this.strutStyle,
    this.textAlign,
    this.textDirection,
    this.locale,
    this.softWrap,
    this.overflow,
    this.textScaleFactor,
    this.maxLines,
    this.semanticsLabel,
    this.textWidthBasis,
    this.textHeightBehavior,
  })
  ```
  可以看出，有几个属性可以直接设置，例如：textAlign，textDirection，locale，overflow等。下边是一个例子
  ```
  const Text(
              "You have pushed  many timesYou have pushed  many timesYou have pushed  many timesYou have pushed  many times:",
              textAlign: TextAlign.left, //对其
              overflow: TextOverflow
                  .ellipsis, //截取部分展示：clip：直接截取 fade：渐隐 ellipsis：省略号，省略的部分是以单词为单位，而不是字母
              textScaleFactor: 1, //设置字体大小的一种快捷方式
              maxLines: 2, //最多允许几行
              textDirection: TextDirection.ltr
  )
  ```

  textStyle主要核心是设置文字的属性，粒度更加细

  ```
  const Text(
              "You have pushed  many timesYou have pushed  many timesYou have pushed  many timesYou have pushed  many times:",
              textAlign: TextAlign.left, //对其
              overflow: TextOverflow
                  .ellipsis, //截取部分展示：clip：直接截取 fade：渐隐 ellipsis：省略号，省略的部分是以单词为单位，而不是字母
              textScaleFactor: 1, //设置字体大小的一种快捷方式
              maxLines: 2, //最多允许几行
              textDirection: TextDirection.ltr,
              style: TextStyle(
                  color: Colors.blue,
                  fontSize: 28.0,
                  fontWeight: FontWeight
                      .bold, //字体粗细 一般使用的属性：FontWeight normal（默认） 、FontWeight bold（粗体）
                  letterSpacing: -1, //字母间距，默认0，负数间距越小，正数 间距越大
                  wordSpacing:
                      1, //单词 间距，默认0，负数间距越小，正数 间距越大，注意和letterSpacing的区别，比如hello，h、e、l、l、o各是一个字母，hello是一个单词
                  height: 2, //会乘以fontSize做为行高
                  //阴影shadows://
                  fontFamily: "Courier",
                  decorationColor: Colors.red, //划线颜色
                  decoration: TextDecoration.underline, //文字划线：下划线、上划线、中划线
                  decorationStyle: TextDecorationStyle.dotted),
            ),
  ```

  运行效果
![20220210181013](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220210181013.png)

# TextSpan 
如果要实现同一段文字的不同部分有不同的样式，就要用到TextSpan了<Br>
看一下它的定义
```
const TextSpan({
  TextStyle style, 
  Sting text,
  List<TextSpan> children,
  GestureRecognizer recognizer,
});

```
在使用的时候，可以分别使用Text.rice和RichText两种方法

```
children: <Widget>[
            const Text(
              "You have many times:",
              textAlign: TextAlign.left, //对其
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
            const Text.rich(
              TextSpan(text: "hello Text.rich ", children: [
                TextSpan(
                  text: "red text span",
                  style: TextStyle(fontSize: 16.0, color: Colors.red),
                ),
                TextSpan(
                  text: "red text span",
                  style: TextStyle(fontSize: 16.0, color: Colors.green),
                ),
              ]),
            ),
            RichText(
              text: const TextSpan(
                  text: "hello  RichText TextSpan ",
                  style: TextStyle(fontSize: 16.0, color: Colors.black),
                  children: [
                    TextSpan(
                      text: "red text span",
                      style: TextStyle(fontSize: 16.0, color: Colors.red),
                    ),
                    TextSpan(
                      text: "yello text span",
                      style: TextStyle(fontSize: 16.0, color: Colors.yellow),
                    )
                  ]),
            ),
          ],
        ),

```
运行效果如下

![20220210184244](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220210184244.png)

# 按钮
Material 组件库中提供了多种按钮组件如ElevatedButton、TextButton、OutlineButton等，它们都是直接或间接对RawMaterialButton组件的包装定制，所以他们大多数属性都和RawMaterialButton一样。在介绍各个按钮时我们先介绍其默认外观，而按钮的外观大都可以通过属性来自定义，我们在后面统一介绍这些属性。另外，所有 Material 库中的按钮都有如下相同点：

按下时都会有“水波动画”（又称“涟漪动画”，就是点击时按钮上会出现水波扩散的动画）。
有一个onPressed属性来设置点击回调，当按钮按下时会执行该回调，如果不提供该回调则按钮会处于禁用状态，禁用状态不响应用户点击。
- 按下时都会有“水波动画”（又称“涟漪动画”，就是点击时按钮上会出现水波扩散的动画）。
- 有一个onPressed属性来设置点击回调，当按钮按下时会执行该回调，如果不提供该回调则按钮会处于禁用状态，禁用状态不响应用户点击。

## 各种按钮
- ElevatedButton
即"漂浮"按钮，它默认带有阴影和灰色背景。按下后，阴影会变大。

- TextButton即文本按钮，默认背景透明并不带阴影。按下后，会有背景色。

- OutlineButton默认有一个边框，不带阴影且背景透明。按下后，边框颜色会变亮、同时出现背景和阴影(较弱)。

- IconButton是一个可点击的Icon，不包括文字，默认没有背景，点击后会出现背景。

```
children: <Widget>[
              const Text(
                "You have many times:",
                textAlign: TextAlign.left, //对其
              ),
              Text(
                '$_counter',
                style: Theme.of(context).textTheme.headline4,
              ),
              ElevatedButton(
                  onPressed: () {}, child: const Text("ElevatedButton")),
              TextButton(onPressed: () {}, child: const Text("TextButton")),
              OutlinedButton(
                  onPressed: () {}, child: const Text("OutlinedButton")),
              IconButton(onPressed: () {}, icon: const Icon(Icons.abc))
            ]),

```
效果如下
![20220210185953](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220210185953.png)

## 带图标的按钮
ElevatedButton、TextButton、OutlineButton都有一个icon 构造函数，通过它可以轻松创建带图标的按钮。
```
   TextButton.icon(
                icon: const Icon(Icons.add_a_photo),
                label: const Text(""),
                onPressed: () {},
              ),
            ]),
```

 如下
 ![20220210190540](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220210190540.png)




# Image图像显示组件
Image在显示图片时定义了一系列参数，通过这些参数我们可以控制图片的显示外观、大小、混合效果等。 Image 的主要参数：
```
const Image({
    Key? key,
    required this.image,
    this.frameBuilder,
    this.loadingBuilder,
    this.errorBuilder,
    this.semanticLabel,
    this.excludeFromSemantics = false,
    this.width,
    this.height,
    this.color,//图片的混合色值
    this.opacity,//透明度
    this.colorBlendMode,//混合模式
    this.fit,//缩放模式
    this.alignment = Alignment.center,
    this.repeat = ImageRepeat.noRepeat,
    this.centerSlice,
    this.matchTextDirection = false,
    this.gaplessPlayback = false,
    this.isAntiAlias = false,
    this.filterQuality = FilterQuality.low,
  })
```
- width、height
  用于设置图片的宽、高，当不指定宽高时，图片会根据当前父容器的限制，尽可能的显示其原始大小，如果只设置width、height的其中一个，那么另一个属性默认会按比例缩放，但可以通过下面介绍的fit属性来指定适应规则。
- fit
该属性用于在图片的显示空间和图片本身大小不同时指定图片的适应模式。适应模式是在BoxFit中定义，它是一个枚举类型，有如下值：
1. fill：会拉伸填充满显示空间，图片本身长宽比会发生变化，图片会变形。
2. cover：会按图片的长宽比放大后居中填满显示空间，图片不会变形，超出显示空间部分会被剪裁。
3. contain：这是图片的默认适应规则，图片会在保证图片本身长宽比不变的情况下缩放以适应当前显示空间，图片不会变形。
4. fitWidth：图片的宽度会缩放到显示空间的宽度，高度会按比例缩放，然后居中显示，图片不会变形，超出显示空间部分会被剪裁。
5. fitHeight：图片的高度会缩放到显示空间的高度，宽度会按比例缩放，然后居中显示，图片不会变形，超出显示空间部分会被剪裁。
6. none：图片没有适应策略，会在显示空间内显示图片，如果图片比显示空间大，则显示空间只会显示图片中间部分。

## 图片数据源
在Image的构造函数中有一个this.image ,这个就是一个 ImageProvider 的实现。<br>
### 加载assets下面的图片
在pubspec.yml中添加图片
```
  assets:
    - images/ic_launcher.png
```
在代码中添加如下代码
```
              const Image(image: AssetImage('images/ic_launcher.png')),
```
效果如下
![20220210192802](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220210192802.png)


### 从网络上加载图片
```
 const Image(
                  image: NetworkImage(
                      "https://docs.flutter.dev/assets/images/dash/dash-fainting.gif")),
```                      

效果如下，gif图也很好的显示
![20220210193559](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220210193559.png)

