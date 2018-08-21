---
layout: page
title: Flutter中的盒约束
permalink: /layout/
summary: 本文介绍了Flutter布局中的盒子模型及约束
keywords: Flutter约束
---


盒约束是指widget可以按照指定限制条件来决定自身如何占用布局空间，所谓的“盒”即指自身的渲染框。

* Placeholder for TOC
{:toc}

## 介绍

在Flutter中，widget由其底层的[`RenderBox`](https://docs.flutter.io/flutter/rendering/RenderBox-class.html)对象渲染。
渲染框由它们的父级给出约束，并且在这些约束下调整自身大小。约束由最小宽度、最大宽度和高度组成; 尺寸由特定的宽度和高度组成。

通常，按照widget如何处理他们的约束来看，有三种类型的盒子：

- 尽可能大。
  例如 [`Center`](https://docs.flutter.io/flutter/widgets/Center-class.html) 和 [`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html) 的渲染盒
- 跟随子widget大小。
  例如, [`Transform`](https://docs.flutter.io/flutter/widgets/Transform-class.html) 和 [`Opacity`](https://docs.flutter.io/flutter/widgets/Opacity-class.html) 的渲染盒。
- 指定尺寸。
  例如, [`Image`](https://docs.flutter.io/flutter/dart-ui/Image-class.html) 和 [`Text`](https://docs.flutter.io/flutter/widgets/Text-class.html)的渲染盒

一些widget，例如[`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)，
会根据构造函数参数的不同而不同。[`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)默认是尽可能大的占用空间，
但是如果你给它指定一个width，那它就会采用指定的值。

其他一些，例如[`Row`](https://docs.flutter.io/flutter/widgets/Row-class.html) 和 [`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html) (flex boxes)
会根据给定它们的约束的不同而不同，如下面的“Flex”部分所述。

这些约束有时是“tight”，这意味着它们没有留下让渲染框自己决定大小的空间（例如，如果最小和最大宽度相同，也就是说有一个tight宽度）。
其中的主要例子是`App` widget, 它是RenderView类包含的一个widget ：由应用程序build函数返回的子widget的渲染框被指定了一个约束，强制它精确填充应用程序的内容区域（通常是整个屏幕）。
Flutter中的许多盒子，特别是那些只包含一个子widget的，都会将其约束传递给他们的孩子。
这意味着如果您在应用程序的渲染树的根部嵌套一些盒子，那么子节点都要受到这些渲染盒的约束。

有些箱子放松了约束，有“最大”约束，但没有“最小”约束。例如，[`Center`](https://docs.flutter.io/flutter/widgets/Center-class.html)。

无边界约束
---------------------

在某些情况下，渲染盒的约束将是无边界（unbounded）的或无限的。这意味着最大宽度或最大高度会设置为`double.INFINITY`。

一个本身试图占用尽可能大的渲染盒在给定无边界约束时不会有用，在检查模式下（译者语：指Dart的checked模式），会抛出异常。

渲染盒具有无边界约束的最常见情况是在自身处于弹性盒（[`Row`](https://docs.flutter.io/flutter/widgets/Row-class.html)
和 [`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html)）内以及可滚动区域 （[`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html) 和其他[`ScrollView`](https://docs.flutter.io/flutter/widgets/ScrollView-class.html)的子类）内。

特别是，[`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html)试图扩充以适应其横向可用空间（即，如果它是一个垂直滚动块，它将尝试与其父项一样宽）。
如果您ListView在水平滚动的情况下嵌套垂直滚动的ListView，则内部滚动区域会尽可能宽，这是无限宽的，因为外部滚动区域可以在水平方向上一直滚动。

Flex
----

弹性盒自身（[`Row`](https://docs.flutter.io/flutter/widgets/Row-class.html)和[`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html)）
的行为有所不同，这取决于它们在给定的方向上是处于有边界的限制还是无边界的限制下。

在有边界的限制下，他们在这个方向上会尽可能大。

在无边界的限制下，他们试图让自己的子节点在这个方向自适应。
在这种情况下，您不能将子节点的`flex`属性设置为0以外的任何值（默认值为0）。
在widget库中，这意味着一个弹性盒位于另一个弹性盒或可滚动的盒子内部时，你不能使用[`Expanded`](https://docs.flutter.io/flutter/widgets/Expanded-class.html)。
如果你这样做，你会得到一个异常消息。

在 _交叉_ 方向上, 例如在[`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html)的宽度和在[`Row`](https://docs.flutter.io/flutter/widgets/Row-class.html)的高度上，它们绝不能是无界的，否则它们将无法合理地对齐他们的子节点。
