---
layout: page
title: "交错动画"
permalink: /animations/staggered-animations/
---

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>你将学到什么:</b>

* For each property being animated, create a Tween.

* 交错动画由一个动画序列或重叠的动画组成。
* 要创建交错动画，需要使用多个动画对象。
* 一个AnimationController控制所有动画。
* 每个动画对象在间隔(Interval)期间指定动画。
* 对于要执行动画的每个属性都要创建一个Tween。

</div>

<aside class="alert alert-info" markdown="1">
**Terminology:**
如果您还不清楚“补间”的概念，请参考
[Flutter动画教程](/tutorials/animation/)
</aside>

交错动画的概念：由一系列操作引发的视觉变化，而不是一次性的操作。动画可能是单纯的顺序执行，比如一个动画跟在另一个动画后面执行，也可能是一部分或者所有动画都重叠起来一块执行.动画之间也可能有间隔，在这个时候什么都不会发生。

下面展示了如何在flutter上构建一个交错动画
<aside class="alert alert-info" markdown="1">
**Examples**<br>

这里展示了一个基本的交错动画的案例，当然你也可以参考更复杂的案例，staggered_pic_selection。

[basic_staggered_animation](https://github.com/flutter/website/tree/master/examples/_animation/basic_staggered_animation)
: 在这个组件里我们展示了一组顺序执行和重叠执行的动画。点击屏幕后，动画会开始改变它的透明度，尺寸，形状，颜色和间距。
  
  

[staggered_pic_selection](https://github.com/flutter/website/tree/master/_includes/code/animation/staggered_pic_selection)
: 该案例展示了从图片列表中删除一张图片，图片的尺寸属于三种尺寸中的一种.
  该案例使用了两个 [animation controllers](https://docs.flutter.io/flutter/animation/AnimationController-class.html):
 一个用于图像选择/取消选择，一个用于图像删除。
 选择/取消选择动画是交错动画。（为了看到这种效果，
 您可能需要增加`timeDilation`值。）
 选择一个最大的图像&mdash；当它显示选中标记时会收缩
 在蓝色圆圈内。接下来，选择一个最小的图像&mdash;
 大图像会随着选中标记的消失而扩展。在大图像
 已完成扩展之前，小图像将缩小以显示其选中标记。
 这种交错的行为类似于你在谷歌照片中看到的行为。
</aside>

* TOC Placeholder
{:toc}

接下来的视频演示了一个基本的交错动画：

<!--
  Use this instead of the default YouTube embed code so that the embed
  is responsive.
-->
<div class="embed-container"><iframe src="https://www.youtube.com/embed/0fFvnZemmh8?rel=0" frameborder="0" allowfullscreen></iframe></div>

在视频中你可以看到，随后的动画变换都在同一个组件中执行，一开始是一个蓝色椭圆边框的矩形。
该矩形按照以下顺序开始变换：

1. 淡入
1. 变宽
1. 向上移动时变高
1. 变成带边框的圆形
1. 变成橙色

交错动画正向执行后，我们又反向执行了一遍

<aside class="alert alert-info" markdown="1">
**Flutter新手？**<br>
本页假定您知道如何使用Flutter的小部件创建布局。
有关更多信息，请参阅在Flutter中构建布局。 [Building Layouts in
Flutter](/tutorials/layout/).
</aside>

## 交错动画的基本构成

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>有哪些重点?</b>

* 所有的动画都使用同一个
  [AnimationController](https://docs.flutter.io/flutter/animation/AnimationController-class.html).
* 无论动画最后的实际运行时间有多长，controller的输入值都控制在0.0到1.0之间.
* 每个动画的
  [Interval](https://docs.flutter.io/flutter/animation/Interval-class.html)
  输入值都在0.0到1.0之间.
* 想要为每个属性定义动画过程中的值，使用
  [Tween.](https://docs.flutter.io/flutter/animation/Tween-class.html)
  Tween将决定该属性在动画过程中的起始值和结束值
* Tween 可以生成一个
  [Animation](https://docs.flutter.io/flutter/animation/Animation-class.html)
  对象，通过传入当前负责的controller来生成.
</div>

{% comment %}
The app is essentially animating a Container whose decoration and size are
animated. The Container is within another Container whose padding moves the
inner container around and an Opacity widget that's used to fade everything
in and out.
{% endcomment %}

下图显示了
[basic_staggered_animation](https://github.com/flutter/website/tree/master/_includes/code/animation/basic_staggered_animation)
示例中使用的Intervals。
您可能会注意到以下特征：:

* 不透明度在时间轴的前10％期间发生变化.
* 在不透明度变化和宽度变化的过程中，有一点小小的间隙.
* 在最后25％的时间线中没有任何动画.
* 通过增加padding使组件向上出现上升.
* 将borderRadius变化成0.5，使得矩形变换成了圆形.
* The padding and border radius changes occur during the same exact interval,
  but they don't have to.
* padding和borderRadius变化时interval是相同的，但是他们并不是必要的.

<img src="images/StaggeredAnimationIntervals.png" alt="Diagram showing the interval specified for each motion." />

设置动画:

* 创建一个AnimationController管理所有的动画.
* 为每个属性创建一个Tween.
  * 为Tween定义一个范围值.
  * Tween的`animate` 方法要求传入 `parent` controller, 然后才能生成一个属性动画.
* 为动画的 `curve` 属性指定一个interval.

当控制过程中动画的值发生改变，或者新创建动画的值发生改变的时候, 就会触发UI的更新.

下面的代码为“宽度”属性创建了一个tween, 它构建一个
[CurvedAnimation](https://docs.flutter.io/flutter/animation/CurvedAnimation-class.html),
指定了一条宽松的动画曲线.
查看 [Curves](https://docs.flutter.io/flutter/animation/Curves-class.html)
可以找到一些系统已经预设好的动画曲线.

<!-- skip -->
{% prettify dart %}
width = new Tween<double>(
  begin: 50.0,
  end: 150.0,
).animate(
  new CurvedAnimation(
    parent: controller,
    curve: new Interval(
      0.125, 0.250,
      curve: Curves.ease,
    ),
  ),
),
{% endprettify %}

`begin` 和 `end` 值没有必要翻倍.接下来的代码将为`borderRadius`属性构建一个tween(将矩形的角变成圆角),这里使用`BorderRadius.circular()`.

{% prettify dart %}
borderRadius = new BorderRadiusTween(
  begin: new BorderRadius.circular(4.0),
  end: new BorderRadius.circular(75.0),
).animate(
  new CurvedAnimation(
    parent: controller,
    curve: new Interval(
      0.375, 0.500,
      curve: Curves.ease,
    ),
  ),
),
{% endprettify %}

### 完整的交错动画

就像所有的交互组件一样，一个完整的动画是由无状态和有状态组件构成.

无状态组件指定了Tweens，定义了动画对象，并且提供了`build()` 方法来构建组件的动画部分.

有状态组件创建了一个controller控制器，用来控制播放动画，并且负责构建了组件的无动画部分。
当屏幕的任何地方被点击后，动画就会开始执行。

[Full code for basic_staggered_animation's main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/code/animation/basic_staggered_animation/main.dart)

### 无状态组件: StaggerAnimation

AnimatedBuilder--一个用于构建动画的通用小部件。
[AnimatedBuilder](https://docs.flutter.io/flutter/widgets/AnimatedBuilder-class.html)&mdash;
AnimatedBuilder构建一个小部件并使用Tweens的当前值配置它。
该示例创建一个名为_buildAnimation（）的函数（执行实际的UI更新），并将其分配给其构建器属性。
AnimatedBuilder监听来自动画控制器的通知，在值发生变化时将组件树标记为dirty。
对于动画的每个刻度，值都会更新，从而调用_buildAnimation（）。

<!-- skip -->
{% prettify dart %}
[[highlight]]class StaggerAnimation extends StatelessWidget[[/highlight]] {
  StaggerAnimation({ Key key, this.controller }) :

    // Each animation defined here transforms its value during the subset
    // of the controller's duration defined by the animation's interval.
    // For example the opacity animation transforms its value during
    // the first 10% of the controller's duration.

    [[highlight]]opacity = new Tween<double>[[/highlight]](
      begin: 0.0,
      end: 1.0,
    ).animate(
      new CurvedAnimation(
        parent: controller,
        curve: new Interval(
          0.0, 0.100,
          curve: Curves.ease,
        ),
      ),
    ),

    // ... Other tween definitions ...

    super(key: key);

  [[highlight]]final Animation<double> controller;[[/highlight]]
  [[highlight]]final Animation<double> opacity;[[/highlight]]
  [[highlight]]final Animation<double> width;[[/highlight]]
  [[highlight]]final Animation<double> height;[[/highlight]]
  [[highlight]]final Animation<EdgeInsets> padding;[[/highlight]]
  [[highlight]]final Animation<BorderRadius> borderRadius;[[/highlight]]
  [[highlight]]final Animation<Color> color;[[/highlight]]

  // This function is called each the controller "ticks" a new frame.
  // When it runs, all of the animation's values will have been
  // updated to reflect the controller's current value.
  [[highlight]]Widget _buildAnimation(BuildContext context, Widget child)[[/highlight]] {
    return new Container(
      padding: padding.value,
      alignment: Alignment.bottomCenter,
      child: new Opacity(
        opacity: opacity.value,
        child: new Container(
          width: width.value,
          height: height.value,
          decoration: new BoxDecoration(
            color: color.value,
            border: new Border.all(
              color: Colors.indigo[300],
              width: 3.0,
            ),
            borderRadius: borderRadius.value,
          ),
        ),
      ),
    );
  }

  @override
  [[highlight]]Widget build(BuildContext context)[[/highlight]] {
    return [[highlight]]new AnimatedBuilder[[/highlight]](
      [[highlight]]builder: _buildAnimation[[/highlight]],
      animation: controller,
    );
  }
}
{% endprettify %}

### Stateful widget: StaggerDemo

有状态小部件StaggerDemo创建了AnimationController（唯一的控制器用来控制组件内的动画），指定了动画续时间为2000毫秒。
它不但负责播放动画，并构建了组件的非动画部分。
在屏幕中检测到点击时动画开始。
动画先正向播放，然后再反向播放。

<!-- skip -->
{% prettify dart %}
[[highlight]]class StaggerDemo extends StatefulWidget[[/highlight]] {
  @override
  _StaggerDemoState createState() => new _StaggerDemoState();
}

class _StaggerDemoState extends State<StaggerDemo> with TickerProviderStateMixin {
  AnimationController _controller;

  @override
  void initState() {
    super.initState();

    _controller = new AnimationController(
      duration: const Duration(milliseconds: 2000),
      vsync: this
    );
  }

  // ...Boilerplate...

  [[highlight]]Future<Null> _playAnimation() async[[/highlight]] {
    try {
      [[highlight]]await _controller.forward().orCancel;[[/highlight]]
      [[highlight]]await _controller.reverse().orCancel;[[/highlight]]
    } on TickerCanceled {
      // the animation got canceled, probably because we were disposed
    }
  }

  @override
  [[highlight]]Widget build(BuildContext context)[[/highlight]] {
    timeDilation = 10.0; // 1.0 is normal animation speed.
    return new Scaffold(
      appBar: new AppBar(
        title: const Text('Staggered Animation'),
      ),
      body: new GestureDetector(
        behavior: HitTestBehavior.opaque,
        onTap: () {
          _playAnimation();
        },
        child: new Center(
          child: new Container(
            width: 300.0,
            height: 300.0,
            decoration: new BoxDecoration(
              color: Colors.black.withOpacity(0.1),
              border: new Border.all(
                color:  Colors.black.withOpacity(0.5),
              ),
            ),
            child: new StaggerAnimation(
              controller: _controller.view
            ),
          ),
        ),
      ),
    );
  }
}
{% endprettify %}

## Resources

当你编写动画时以下资源可能对你有所帮助:

[Animations landing page](/animations/)
: Lists the available documentation for Flutter animations.
  If tweens are new to you, check out the
  [Animations tutorial](/tutorials/animation/).

[Flutter API documentation](https://docs.flutter.io/)
: Reference documentation for all of the Flutter libraries.
  In particular, see the [animation
  library](https://docs.flutter.io/flutter/animation/animation-library.html)
  documentation.

[Flutter Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)
: Demo app showcasing many Material Components and other Flutter
  features.  The [Shrine
  demo](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery/lib/demo/shrine)
  implements a hero animation.

[Material motion spec](https://material.io/guidelines/motion/)
: Describes motion for Material apps.
