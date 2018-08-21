---
layout: page
title: "教程：Flutter中的动画"
permalink: /tutorials/animation/
summary: Flutter动画教程，介绍了Flutter中的补间(Tween)动画、hero动画、交错动画等。
keywords: Flutter动画教程
---

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>你会学到什么：</b>

* 如何使用动画库中的基础类将动画添加到widget
* 何时使用AnimatedWidget与AnimatedBuilder.

</div>

本教程向您展示如何在Flutter中构建动画。在介绍动画库中的一些基本概念、类和方法之后，它会引导您完成5个动画示例，向您介绍动画库的不同内容。

* TOC Placeholder
{:toc}

## 基本的动画概念和类

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>重点是什么？</b>

* Animation对象是Flutter动画库中的一个核心类，它生成指导动画的值。
* Animation对象知道动画的当前状态（例如，它是开始、停止还是向前或向后移动），但它不知道屏幕上显示的内容。
* AnimationController管理Animation。
* CurvedAnimation 将过程抽象为一个非线性曲线.
* Tween在正在执行动画的对象所使用的数据范围之间生成值。例如，Tween可能会生成从红到蓝之间的色值，或者从0到255。
* 使用Listeners和StatusListeners监听动画状态改变。

</div>

Flutter中的动画系统基于[`Animation`](https://docs.flutter.io/flutter/animation/Animation-class.html)对象的，widget可以在`build`函数中读取[`Animation`](https://docs.flutter.io/flutter/animation/Animation-class.html)对象的当前值，
并且可以监听动画的状态改变。

<a name="animation-class"></a>
### Animation&lt;double&gt;


在Flutter中，Animation对象本身和UI渲染没有任何关系。Animation是一个抽象类，它拥有其当前值和状态（完成或停止）。其中一个比较常用的Animation类是Animation&lt;double&gt;。

Flutter中的Animation对象是一个在一段时间内依次生成一个区间之间值的类。Animation对象的输出可以是线性的、曲线的、一个步进函数或者任何其他可以设计的映射。
根据Animation对象的控制方式，动画可以反向运行，甚至可以在中间切换方向。

Animation还可以生成除double之外的其他类型值，如：Animation&lt;Color&gt; 或 Animation&lt;Size&gt;。

Animation对象有状态。可以通过访问其`value`属性获取动画的当前值。

Animation对象本身和UI渲染没有任何关系。

### CurvedAnimation

CurvedAnimation 将动画过程定义为一个非线性曲线.

<!-- skip -->
{% prettify dart %}
final CurvedAnimation curve =
    new CurvedAnimation(parent: controller, curve: Curves.easeIn);
{% endprettify %}

<aside class="alert alert-success" markdown="1">
**注:**
[Curves](https://docs.flutter.io/flutter/animation/Curves-class.html)
类类定义了许多常用的曲线，也可以创建自己的，例如：

<!-- skip -->
{% prettify dart %}
class ShakeCurve extends Curve {
  @override
  double transform(double t) {
    return math.sin(t * math.PI * 2);
  }
}
{% endprettify %}
</aside>

CurvedAnimation和AnimationController（在下一节中介绍）都是Animation&lt;double&gt;类型。CurvedAnimation包装它正在修改的对象 - 您不需要子类AnimationController来实现曲线

### AnimationController

AnimationController是一个特殊的Animation对象，在屏幕刷新的每一帧，就会生成一个新的值。默认情况下，AnimationController在给定的时间段内会线性的生成从0.0到1.0的数字。
例如，下面代码创建一个Animation对象，但不会启动它运行：

<!-- skip -->
{% prettify dart %}
final AnimationController controller = new AnimationController(
    duration: const Duration(milliseconds: 2000), vsync: this);
{% endprettify %}

AnimationController派生自Animation&lt;double&gt;，因此可以在需要Animation对象的任何地方使用。
但是，AnimationController具有控制动画的其他方法。例如，`.forward()`方法可以启动动画。数字的产生与屏幕刷新有关，因此每秒钟通常会产生60个数字，在生成每个数字后，每个Animation对象调用添加的Listener对象。

当创建一个AnimationController时，需要传递一个`vsync`参数，存在`vsync`时会防止屏幕外动画（译者语：动画的UI不在当前屏幕时）消耗不必要的资源。
通过将SingleTickerProviderStateMixin添加到类定义中，可以将stateful对象作为`vsync`的值。你可以在GitHub的[animate1](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate1/main.dart)中看到这个例子 。

> 译者语：`vsync`对象会绑定动画的定时器到一个可视的widget，所以当widget不显示时，动画定时器将会暂停，当widget再次显示时，动画定时器重新恢复执行，这样就可以避免动画相关UI不在当前屏幕时消耗资源。
> 如果要使用自定义的State对象作为`vsync`时，请包含`TickerProviderStateMixin`。


<aside class="alert alert-success" markdown="1">
**注意**：
在某些情况下，值(position，值动画的当前值)可能会超出AnimationController的0.0-1.0的范围。例如，`fling()`函数允许您提供速度(velocity)、力量(force)、position(通过Force对象)。位置(position)可以是任何东西，因此可以在0.0到1.0范围之外。
CurvedAnimation生成的值也可以超出0.0到1.0的范围。根据选择的曲线，CurvedAnimation的输出可以具有比输入更大的范围。例如，Curves.elasticIn等弹性曲线会生成大于或小于默认范围的值。
</aside>

### Tween

默认情况下，AnimationController对象的范围从0.0到1.0。如果您需要不同的范围或不同的数据类型，则可以使用Tween来配置动画以生成不同的范围或数据类型的值。例如，以下示例，Tween生成从-200.0到0.0的值：

<!-- skip -->
{% prettify dart %}
final Tween doubleTween = new Tween<double>(begin: -200.0, end: 0.0);
{% endprettify %}

Tween是一个无状态(stateless)对象，需要`begin`和`end`值。Tween的唯一职责就是定义从输入范围到输出范围的映射。输入范围通常为0.0到1.0，但这不是必须的。

Tween继承自Animatable&lt;T&gt;，而不是继承自Animation&lt;T&gt;。Animatable与Animation相似，不是必须输出double值。例如，ColorTween指定两种颜色之间的过渡。

<!--- skip -->
{% prettify dart %}
final Tween colorTween =
    new ColorTween(begin: Colors.transparent, end: Colors.black54);
{% endprettify %}

Tween对象不存储任何状态。相反，它提供了`evaluate(Animation<double> animation)`方法将映射函数应用于动画当前值。
Animation对象的当前值可以通过`value()`方法取到。`evaluate`函数还执行一些其它处理，例如分别确保在动画值为0.0和1.0时返回开始和结束状态。

#### Tween.animate

要使用Tween对象，请调用其`animate()`方法，传入一个控制器对象。例如，以下代码在500毫秒内生成从0到255的整数值。

<!-- skip -->
{% prettify dart %}
final AnimationController controller = new AnimationController(
    duration: const Duration(milliseconds: 500), vsync: this);
Animation<int> alpha = new IntTween(begin: 0, end: 255).animate(controller);
{% endprettify %}

注意`animate()`返回的是一个Animation，而不是一个Animatable。

以下示例构建了一个控制器、一条曲线和一个Tween：

<!-- skip -->
{% prettify dart %}
final AnimationController controller = new AnimationController(
    duration: const Duration(milliseconds: 500), vsync: this);
final Animation curve =
    new CurvedAnimation(parent: controller, curve: Curves.easeOut);
Animation<int> alpha = new IntTween(begin: 0, end: 255).animate(curve);
{% endprettify %}

<a name="monitoring"></a>
### 动画通知

一个Animation对象可以拥有Listeners和StatusListeners监听器，可以用`addListener()`和`addStatusListener()`来添加。
只要动画的值发生变化，就会调用监听器。一个Listener最常见的行为是调用setState()来触发UI重建。动画开始、结束、向前移动或向后移动（如AnimationStatus所定义）时会调用StatusListener。
下一节中有一个`addListener()`方法的例子。[监视动画的进度](#monitoring)展示了如何调用`addStatusListener()`。

---

## 动画示例

本节将向您介绍5个动画示例。每个部分都提供了该示例源代码的链接。


### 渲染动画

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>重点是什么?</b>

* 如何通过`addListener()`和`setState()`给widget添加基础的动画。
* 每次动画生成一个新数字时，监听函数都会调用`setState()`。
* 如何使用必需的`vsync`参数定义AnimatedController
* 了解Dart语言中的 `..`语法。

</div>

到目前为止，您已经学会了如何随着时间的推移生成一系列数字，但没有任何东西被渲染到屏幕上。
要使用Animation&lt;&gt;对象进行渲染，请将Animation对象存储为Widget的成员，然后使用其`value`值来决定如何绘制。

考虑下面的应用程序，它绘制Flutter logo时没有动画：

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => new _LogoAppState();
}

class _LogoAppState extends State<LogoApp> {
  Widget build(BuildContext context) {
    return new Center(
      child: new Container(
        margin: new EdgeInsets.symmetric(vertical: 10.0),
        height: 300.0,
        width: 300.0,
        child: new FlutterLogo(),
      ),
    );
  }
}

void main() {
  runApp(new LogoApp());
}
{% endprettify %}

修改以上代码，通过一个逐渐放大的动画显示logo。定义AnimationController时，必须传入一个`vsync`对象。该`vsync`参数在上面[AnimationController](#animationcontroller)部分有介绍了 。

高亮部分为修改的代码:

<!-- skip -->
{% prettify dart %}
[[highlight]]import 'package:flutter/animation.dart';[[/highlight]]
import 'package:flutter/material.dart';

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => new _LogoAppState();
}

class _LogoAppState extends State<LogoApp> [[highlight]]with SingleTickerProviderStateMixin[[/highlight]] {
  [[highlight]]Animation<double> animation;[[/highlight]]
  [[highlight]]AnimationController controller;[[/highlight]]

  [[highlight]]initState() {[[/highlight]]
    [[highlight]]super.initState();[[/highlight]]
    [[highlight]]controller = new AnimationController([[/highlight]]
        [[highlight]]duration: const Duration(milliseconds: 2000), vsync: this);[[/highlight]]
    [[highlight]]animation = new Tween(begin: 0.0, end: 300.0).animate(controller)[[/highlight]]
      [[highlight]]..addListener(() {[[/highlight]]
        [[highlight]]setState(() {[[/highlight]]
          [[highlight]]// the state that has changed here is the animation object’s value[[/highlight]]
        [[highlight]]});[[/highlight]]
      [[highlight]]});[[/highlight]]
    [[highlight]]controller.forward();[[/highlight]]
  [[highlight]]}[[/highlight]]

  Widget build(BuildContext context) {
    return new Center(
      child: new Container(
        margin: new EdgeInsets.symmetric(vertical: 10.0),
        height: [[highlight]]animation.value,[[/highlight]]
        width: [[highlight]]animation.value,[[/highlight]]
        child: new FlutterLogo(),
      ),
    );
  }

  [[highlight]]dispose() {[[/highlight]]
    [[highlight]]controller.dispose();[[/highlight]]
    [[highlight]]super.dispose();[[/highlight]]
  }
}

void main() {
  runApp(new LogoApp());
}
{% endprettify %}

该`addListener()`函数调用了`setState()`，所以每次动画生成一个新的数字时，当前帧被标记为脏(dirty)，这会导致widget的`build()`方法再次被调用。
在`build()`中，改变container大小，因为它的高度和宽度现在使用的是`animation.value`。动画完成时释放控制器(调用`dispose()`方法)以防止内存泄漏。


[animate1.](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate1/main.dart)
通过简单的修改，您已经在Flutter中创建了第一个动画！这个例子的源代码[animate1.](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate1/main.dart)
、
<aside class="alert alert-success" markdown="1">
**Dart语言技巧：**
您可能不熟悉Dart级联符号(两个点)，如`..addListener()`，这种语法意味着使用`animate()`的返回值来调用`addListener()`方法。考虑下面的例子：

<!-- skip -->
{% prettify dart %}
[[highlight]]animation = tween.animate(controller)[[/highlight]]
          [[highlight]]..addListener(()[[/highlight]] {
            setState(() {
              // the animation object’s value is the changed state
            });
          });
{% endprettify %}

上面的代码等价于:

<!-- skip -->
{% prettify dart %}
[[highlight]]animation = tween.animate(controller);[[/highlight]]
[[highlight]]animation.addListener(()[[/highlight]] {
            setState(() {
              // the animation object’s value is the changed state
            });
          });
{% endprettify %}

您可以在[Dart语言之旅](https://www.dartlang.org/guides/language/language-tour) 中了解更多关于级联符号的信息。
</aside>

### 用AnimatedWidget简化

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>重点是什么？</b>

* 如何使用AnimatedWidget助手类（而不是`addListener()`和`setState()`）来给widget添加动画
* 使用AnimatedWidget创建一个可重用动画的widget。要从widget中分离出动画过渡，请使用[AnimatedBuilder](#refactoring-with-animatedbuilder)。
* Flutter API提供的关于AnimatedWidget的示例包括：AnimatedBuilder、AnimatedModalBarrier、DecoratedBoxTransition、FadeTransition、PositionedTransition、RelativePositionedTransition、RotationTransition、ScaleTransition、SizeTransition、SlideTransition。

</div>

AnimatedWidget类允许您从`setState()`调用中的动画代码中分离出widget代码。AnimatedWidget不需要维护一个State对象来保存动画。

在下面的重构示例中，LogoApp现在继承自AnimatedWidget而不是StatefulWidget。AnimatedWidget在绘制时使用动画的当前值。LogoApp仍然管理着AnimationController和Tween。

<!-- skip -->
{% prettify dart %}
// Demonstrate a simple animation with AnimatedWidget

import 'package:flutter/animation.dart';
import 'package:flutter/material.dart';

class AnimatedLogo extends AnimatedWidget {
  AnimatedLogo({Key key, Animation<double> animation})
      : super(key: key, listenable: animation);

  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return new Center(
      child: new Container(
        margin: new EdgeInsets.symmetric(vertical: 10.0),
        height: animation.value,
        width: animation.value,
        child: new FlutterLogo(),
      ),
    );
  }
}

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => new _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = new AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = new Tween(begin: 0.0, end: 300.0).animate(controller);
    controller.forward();
  }

  Widget build(BuildContext context) {
    return new AnimatedLogo(animation: animation);
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}

void main() {
  runApp(new LogoApp());
}
{% endprettify %}

LogoApp将Animation对象传递给基类并用`animation.value`设置容器的高度和宽度，因此它的工作原理与之前完全相同。

> 译者语：和animate1中不同的是，AnimatedWidget(基类)中会自动调用`addListener()`和`setState()`。

你可以 在GitHub上找到这个例子的源代码[animate2](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate2/main.dart)。


### 监视动画的过程

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>重点是什么？</b>

* 使用`addStatusListener`来处理动画状态更改的通知，例如启动、停止或反转方向。
* 当动画完成或返回其开始状态时，通过反转方向来无限循环运行动画

</div>

知道动画何时改变状态通常很有用的，如完成、前进或倒退。你可以通过`addStatusListener()`来得到这个通知。
以下代码修改[animate1](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate1/main.dart)示例，以便它监听动态状态更改并打印更新。
下面高亮显示的部分为修改的：

<!-- skip -->
{% prettify dart %}
class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = new AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = new Tween(begin: 0.0, end: 300.0).animate(controller)
      [[highlight]]..addStatusListener((state) => print("$state"));[[/highlight]]
    controller.forward();
  }
  //...
}
{% endprettify %}

运行此代码将输出以下内容：

<!-- skip -->
{% prettify sh %}
AnimationStatus.forward
AnimationStatus.completed
{% endprettify %}


接下来，使用`addStatusListener()`在开始或结束时反转动画。这产生了循环效果：

<!-- skip -->
{% prettify dart %}
class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = new AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = new Tween(begin: 0.0, end: 300.0).animate(controller);

    animation.addStatusListener((status) {
      if (status == AnimationStatus.completed) {
        controller.reverse();
      } else if (status == AnimationStatus.dismissed) {
        controller.forward();
      }
    });
    controller.forward();
  }
  //...
}
{% endprettify %}

你可以在GitHub上找到这个例子的源代码[animate3](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate3/main.dart)。

### 用AnimatedBuilder重构

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>重点是什么？</b>

* AnimatedBuilder了解如何渲染过渡.
* An AnimatedBuilder 不知道如何渲染widget，也不知道如何管理Animation对象。
* 使用AnimatedBuilder将动画描述为另一个widget的`build`方法的一部分。如果你只是想用可复用的动画定义一个widget，请使用AnimatedWidget。
* Flutter API中AnimatedBuilder的示例包括: BottomSheet、ExpansionTile、
  PopupMenu、ProgressIndicator、RefreshIndicator、Scaffold、SnackBar、TabBar、TextField。

</div>

[animate3](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate3/main.dart)示例中的代码存在的一个问题：
更改动画需要更改显示logo的widget。更好的解决方案是将职责分离：

* 显示logo
* 定义Animation对象
* 渲染过渡效果

您可以借助AnimatedBuilder类完成此分离。AnimatedBuilder是渲染树中的一个独立的类。
与AnimatedWidget类似，AnimatedBuilder自动监听来自Animation对象的通知，并根据需要将该控件树标记为脏(dirty)，因此不需要手动调用`addListener()`。

[animate5](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate5/main.dart)示例的widget树如下所示：

<img src="images/AnimatedBuilder-WidgetTree.png" alt="A widget tree with Container pointing to ContainerTransition, pointing to AnimatedBuilder, pointing to (AnonymousBuilder), pointing to LogoWidget.">

从widget树的底部开始，渲染logo的代码直接明了：

<!-- skip -->
{% prettify dart %}
class LogoWidget extends StatelessWidget {
  // Leave out the height and width so it fills the animating parent
  build(BuildContext context) {
    return new Container(
      margin: new EdgeInsets.symmetric(vertical: 10.0),
      child: new FlutterLogo(),
    );
  }
}
{% endprettify %}

图中的中间三个块都在GrowTransition的`build()`方法中创建。GrowTransition本身是无状态的，并拥有定义过渡动画所需的最终变量集合。
`build()`函数创建并返回AnimatedBuilder，它将（匿名构建器）方法和LogoWidget对象作为参数。渲染转换的工作实际上发生在（匿名构建器）方法中，
该方法创建一个适当大小的Container来强制缩放LogoWidget。

下面的代码中有一个的问迷惑的题是，`child`看起来像被指定了两次。但实际发生的事情是，将外部引用child传递给AnimatedBuilder，AnimatedBuilder将其传递给匿名构造器，
然后将该对象用作其子对象。最终的结果是AnimatedBuilder插入到渲染树中的两个widget之间。

<!-- skip -->
{% prettify dart %}
class GrowTransition extends StatelessWidget {
  GrowTransition({this.child, this.animation});

  final Widget child;
  final Animation<double> animation;

  Widget build(BuildContext context) {
    return new Center(
      child: new AnimatedBuilder(
          animation: animation,
          builder: (BuildContext context, Widget child) {
            return new Container(
                height: animation.value, width: animation.value, child: child);
          },
          child: child),
    );
  }
}
{% endprettify %}

最后，初始化动画的代码与第一个示例[animate1.](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate1/main.dart)非常相似。
`initState()`方法创建一个AnimationController和一个Tween，然后通过`animate()`绑定它们。魔术发生在`build()`方法中，该方法返回一个带有LogoWidget作为子对象的GrowTransition对象，以及一个用于驱动过渡的动画对象。


<!-- skip -->
{% prettify dart %}
class LogoApp extends StatefulWidget {
  _LogoAppState createState() => new _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with TickerProviderStateMixin {
  Animation animation;
  AnimationController controller;

  initState() {
    super.initState();
    controller = new AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    final CurvedAnimation curve =
        new CurvedAnimation(parent: controller, curve: Curves.easeIn);
    animation = new Tween(begin: 0.0, end: 300.0).animate(curve);
    controller.forward();
  }

  Widget build(BuildContext context) {
    return new GrowTransition(child: new LogoWidget(), animation: animation);
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}

void main() {
  runApp(new LogoApp());
}
{% endprettify %}

你可以 在GitHub上找到这个例子的源代码[animate4](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate4/main.dart)。

### 并行动画

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>重点是什么？</b>

* [Curves](https://docs.flutter.io/flutter/animation/Curves-class.html)
  类定义常用动画曲线的数组，你可以通过[CurvedAnimation](https://docs.flutter.io/flutter/animation/CurvedAnimation-class.html) 使用。

</div>

在本节中，您将基于[监视动画过程](#monitoring]([animate3](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate3/main.dart)中的示例进行构建，
该示例使用AnimatedWidget循环的进行放大和缩小，现在考虑一下如何再添加一个在透明和不透明之间循环的动画。

<aside class="alert alert-success" markdown="1">
**注意：**此示例展示了如何在同一个动画控制器上使用多个Tween，其中每个Tween管理动画中的不同效果。
本示例仅用于示例，如果您在生产代码中需要使用不透明度和大小变化的Tween，则可能会使用[FadeTransition](https://docs.flutter.io/flutter/widgets/FadeTransition-class.html)和[SizeTransition](https://docs.flutter.io/flutter/widgets/SizeTransition-class.html)。
</aside>

每一个Tween管理动画的一种效果。例如：

<!-- skip -->
{% prettify dart %}
final AnimationController controller =
    new AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
final Animation<double> sizeAnimation =
    new Tween(begin: 0.0, end: 300.0).animate(controller);
final Animation<double> opacityAnimation =
    new Tween(begin: 0.1, end: 1.0).animate(controller);
{% endprettify %}

你可以通过`sizeAnimation.value`来获取大小，通过`opacityAnimation.value`来获取不透明度，但AnimatedWidget的构造函数只接受一个动画对象。
为了解决这个问题，该示例创建了自己的Tween对象并显式计算了这些值。

其`build`方法`.evaluate()`在父级的动画对象上调用Tween函数以计算所需的`size`和`opacity`值。

下面高亮部分即修改的代码:

<!-- skip -->
{% prettify dart %}
import 'package:flutter/animation.dart';
import 'package:flutter/material.dart';

class AnimatedLogo extends AnimatedWidget {
  // The Tweens are static because they don't change.
  [[highlight]]static final _opacityTween = new Tween<double>(begin: 0.1, end: 1.0);[[/highlight]]
  [[highlight]]static final _sizeTween = new Tween<double>(begin: 0.0, end: 300.0);[[/highlight]]

  AnimatedLogo({Key key, Animation<double> animation})
      : super(key: key, listenable: animation);

  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return new Center(
      [[highlight]]child: new Opacity([[/highlight]]
        [[highlight]]opacity: _opacityTween.evaluate(animation),[[/highlight]]
        child: new Container(
          margin: new EdgeInsets.symmetric(vertical: 10.0),
          height: [[highlight]]_sizeTween.evaluate(animation)[[/highlight]],
          width: [[highlight]]_sizeTween.evaluate(animation)[[/highlight]],
          child: new FlutterLogo(),
        ),
      ),
    );
  }
}

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => new _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with TickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = new AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = [[highlight]]new CurvedAnimation(parent: controller, curve: Curves.easeIn);[[/highlight]]

    animation.addStatusListener((status) {
      if (status == AnimationStatus.completed) {
        controller.reverse();
      } else if (status == AnimationStatus.dismissed) {
        controller.forward();
      }
    });

    controller.forward();
  }

  Widget build(BuildContext context) {
    return new AnimatedLogo(animation: animation);
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}

void main() {
  runApp(new LogoApp());
}
{% endprettify %}

你可以在GitHub上找到这个例子的源代码[animate5](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/animate5/main.dart)。

## 下一步

本教程为您展示了如何使用Tweens在Flutter中创建动画的基础，但还有很多其他类需要探索。您可以研究一下特定的Tween类、Material Design中特定的动画、ReverseAnimation、共享元素过渡(也称为hero动画)、物理模拟和`fling()`方法的动画 。查看[动画首页](/animations/)获取最新的可用文档和示例。
