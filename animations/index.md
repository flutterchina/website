---
layout: page
title: Flutter中的动画
permalink: /animations/
summary: 精心设计的动画会让用户界面感觉更直观、流畅，能改善用户体验。本文系统性的介绍了Flutter中各种类型的动画。
keywords: Flutter动画教程
---

* TOC Placeholder
{:toc}

精心设计的动画会让用户界面感觉更直观、流畅，能改善用户体验。
Flutter的动画支持可以轻松实现各种动画类型。许多widget，特别是[Material Design widgets](https://flutter.io/widgets/material/)，
都带有在其设计规范中定义的标准动画效果，但也可以自定义这些效果。

以下资源是开始学习Flutter动画框架的好地方。这些文档都详细的展示了如何编写动画代码。
{% comment %}
More documentation is in the works on how to implement common design
patterns, such as shared element transitions,
and physics-based animations.
If you have a specific request, please
[file an issue](https://github.com/flutter/flutter/issues).
{% endcomment %}

* [教程: Flutter中的动画](/tutorials/animation/)<br>
介绍了Flutter动画包（控制器、动画、曲线、监听器）中的基础类，它引导您使用不同的动画API逐步制作补间动画。

* [构建漂亮的用户界面](https://codelabs.developers.google.com/codelabs/flutter/index.html#0)<br>
Codelab 演示如何构建简单的聊天应用程序. [Step 7 (Animate
your app)](https://codelabs.developers.google.com/codelabs/flutter/index.html#6)
展示了如何对新消息进行动画处理 - 将它从输入区域滑动到消息列表。

## 动画类型


动画分为两类：基于tween或基于物理的。以下部分解释了这些术语的含义，并列出了一些相关的资源。
在一些情况下，我们最好的文档就是Flutter gallery中的示例代码。

### 补间(Tween)动画

“介于两者之间”的简称。在补间动画中，定义了开始点和结束点、时间线以及定义转换时间和速度的曲线。然后由框架计算如何从开始点过渡到结束点。

上面列出的文档[Flutter动画教程](/tutorials/animation/) 并不是专门介绍补间动画的，但在其示例中使用了补间动画。

### 基于物理的动画

在基于物理的动画中，运动被模拟为与真实世界的行为相似。例如，当你掷球时，它在何处落地，取决于抛球速度有多快、球有多重、距离地面有多远。
类似地，将连接在弹簧上的球落下（并弹起）与连接到绳子上的球放下的方式也是不同。

* [Flutter Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)<br>
在 **Material Components** ,
[Grid](https://github.com/flutter/flutter/blob/master/examples/flutter_gallery/lib/demo/material/grid_list_demo.dart)
示例中演示了一个动画。从网格中选择其中一个图像并放大，然后您可以甩手或拖动平移图像。

* 另外请参阅
[AnimationController.animateWith](https://docs.flutter.io/flutter/animation/AnimationController/animateWith.html) 和
[SpringSimulation](https://docs.flutter.io/flutter/physics/SpringSimulation-class.html)的文档

## 常见的动画模式

大多数UX或交互设计师发现在设计UI时有一些会经常使用的动画模式。本节列出了一些常用的动画模式，并告诉您可以在哪里了解更多。

### 动画列表或网格

此模式涉及在网格或列表中添加或删除元素时应用动画。

* [AnimatedList 示例](/catalog/samples/animated-list/)<br>
此演示来自[示例程序目录](/catalog/samples)，演示如何将元素添加到列表或删除选定元素。
在用户使用加号（+）和减号（ - ）按钮时修该并同步列表。

### 共享元素转换

在这种模式中，用户从页面中选择一个元素（通常是一个图像），然后打开所选元素的详情页面，在打开详情页时使用动画。
在Flutter中，您可以使用Hero widget 轻松实现路由（页面）之间的共享元素过渡动画。

* [Hero 动画](/animations/hero-animations/)<br>
如何创建两种风格的 Hero 动画：
  * 在改变位置和大小的同时，hero从一页飞到另一页
  * hero的边界从一个圆形变成一个正方形，同时它从一个页面飞到另一个页面

* [Flutter Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)
您可以自己构建Gallery应用程序，也可以从Play商店下载(中国不行)。
其中 [Shrine](https://github.com/flutter/flutter/blob/master/examples/flutter_gallery/lib/demo/shrine_demo.dart)演示了包括hero动画的一个例子。

* 另外请参阅
[Hero,](https://docs.flutter.io/flutter/widgets/Hero-class.html)
[Navigator](https://docs.flutter.io/flutter/widgets/Navigator-class.html) 和
[PageRoute](https://docs.flutter.io/flutter/widgets/PageRoute-class.html)
类的API文档。

### 交错动画

动画被分解为较小的动作，其中一些动作被延迟。较小的动画可以是连续的，或者可以部分或完全重叠。

* [交错动画（Staggered Animations）](/animations/staggered-animations/) <img src="/images/ic_new_releases_black_24px.svg" alt="this doc is new!"> NEW<br>

## 其它资源

在以下链接中了解更多关于Flutter动画的信息：

* [动画: 技术概述](/animations/overview.html)<br>
查看动画库中的一些主要类，以及Flutter的动画架构。

* [动画和运动（Motion） Widgets](/widgets/animation/)<br>
Flutter提供的一些动画widget的目录

{% comment %}
Until the landing page for the animation library is reworked, leave this
link out.
* The [animation
library](https://docs.flutter.io/flutter/animation/animation-library.html)
in the [Flutter API documentation](https://docs.flutter.io/)<br>
The animation API for the Flutter framework.
{% endcomment %}




