---
layout: page
title: "动画:技术概述"
permalink: /animations/overview.html
---

* TOC Placeholder
{:toc}

Flutter中的动画系统是基于类型化的动画[`Animation`](https://docs.flutter.io/flutter/animation/Animation-class.html)对象。
组件可以通过读取自身的当前值和监听它们的状态变化，将这些动画直接合并到它们的build函数中，
或者以这些动画作为复杂动画的一个基础的部分传递给其他组件使用

## Animation
动画系统的主要模块是[`Animation`](https://docs.flutter.io/flutter/animation/Animation-class.html)类。
一个Animation代表了特定值在动画生命周期内的可以不断的改变。
动画表示可以在动画的生命周期内更改的特定类型的值。
大多数执行动画的组件都会接收一个Animation对象作为参数，从中可以读取动画的当前值以及监听该值的改变。

### `addListener`

每当动画的值发生变化时，动画都会通知所有添加了[`addListener`](https://docs.flutter.io/flutter/animation/Animation/addListener.html)的侦听器。
通常来说，监听动画的[`State`](https://docs.flutter.io/flutter/widgets/State-class.html)对象
将在其侦听器的回调中调用setState方法，以通知组件使用新值来重新构建.

这种模式很常见，当动画的值改变时下面这两个组件都可以用来帮助组件重新构建：
[`AnimatedWidget`](https://docs.flutter.io/flutter/widgets/AnimatedWidget-class.html)
和
[`AnimatedBuilder`](https://docs.flutter.io/flutter/widgets/AnimatedBuilder-class.html).
第一个是AnimatedWidget，对于无状态动画组件最有用。
要使用AnimatedWidget，只需实现其子类并实现构建函数。
第二个是AnimatedBuilder，对于一些复杂的组件希望将动画作为它的大构造方法的一部分来说，AnimatedBuilder将会非常有用。
要使用AnimatedBuilder，你只需要简单的构造一个组件，然后通过`builder`方法来使用.

### `addStatusListener`

动画还提供了一个[`AnimationStatus`](https://docs.flutter.io/flutter/animation/AnimationStatus-class.html),它展示动画将如何随时间演变。
每当动画的状态发生变化时，动画都会通知addStatusListener添加的所有侦听器。
通常情况下，如果动画脱离`dismissed` 状态，这意味着动画进入指定的范围内开始运行了。
例如，当值为0.0时，从0.0到1.0的动画将处于`dismissed`状态。
一个动画可以正向执行（例如，从0.0到1.0）也可以反向执行（例如，从1.0到0.0）。
最终，如果动画到达其范围的末尾（例如，1.0），则动画进入`completed` 状态.

## AnimationController

想要创建一个动画，首先需要创建一个
[`AnimationController`](https://docs.flutter.io/flutter/animation/AnimationController-class.html).
除了动画自身，`AnimationController`也可以让你控制
动画。 例如，您可以告诉控制器控制动画正向播放
[`forward`](https://docs.flutter.io/flutter/animation/AnimationController/forward.html)
或者停止 [`stop`](https://docs.flutter.io/flutter/animation/AnimationController/stop.html)
动画. 你也可以使用 [`fling`](https://docs.flutter.io/flutter/animation/AnimationController/fling.html)
方法，他可以模拟物理环境下的速度变化，比如弹簧一样来控制动画的播放。 
一旦你创建了动画控制器，你就可以基于此开始构建其他的动画了.比如
[`ReverseAnimation`](https://docs.flutter.io/flutter/animation/ReverseAnimation-class.html)
它控制了一个原始动画，以相反的方向运行（例如，从1.0到0.0).
同样，你可以创建一个[`CurvedAnimation`](https://docs.flutter.io/flutter/animation/CurvedAnimation-class.html)
，它使用曲线[curve](https://docs.flutter.io/flutter/animation/Curves-class.html)的方式来控制value的变化.

## Tweens

如果你的动画取值间隔超过了0.0到1.0，你可以使用
[`Tween<T>`](https://docs.flutter.io/flutter/animation/Tween-class.html)来控制动画从
[`begin`](https://docs.flutter.io/flutter/animation/Tween/begin.html)
到[`end`](https://docs.flutter.io/flutter/animation/Tween/end.html)
进行插值运算。许多类型都有特定的`Tween`子类，它们提供特定类型的
插值。 例如，
[`ColorTween`](https://docs.flutter.io/flutter/animation/ColorTween-class.html)
可以在不同颜色间进行插值运算
[`RectTween`](https://docs.flutter.io/flutter/animation/RectTween-class.html)
可以在两个矩形间进行插值运算. 你甚至可以自己定义一个插值器通过构建一个`Tween`的子类，并且重写它的方法
[`lerp`](https://docs.flutter.io/flutter/animation/Tween/lerp.html).

补间本身只定义了如何在两个值之间进行插值。要得到动画的当前帧的具体值，您还需要一个动画确定当前状态。 
有两种方法可以将补间和动画组合在一起来获取动画当前的状态值:

1. 你可以使用[`evaluate`](https://docs.flutter.io/flutter/animation/Tween/evaluate.html)
   来获取补间动画的当前值。这个方式对已经在监听动画的组件来说非常有用，每当动画的值修改后组件都会执行重新构建.

2. 你可以使用 [`animate`](https://docs.flutter.io/flutter/animation/Animatable/animate.html)
   方法在基于一个动画对象，创建一个补间动画. 该方法将返回一个
   使用了补间动画的插值器的`Animation`动画对象.
   当你想要为其他组件创建一个新的动画，这是非常有效的实践能够让你读取插值器的当前值就像在监听
   补间动画值的改变一样。

# Architecture

实际上一个动画是由很多模块构建而成的

## Scheduler

这个调度绑定器
[`SchedulerBinding`](https://docs.flutter.io/flutter/scheduler/SchedulerBinding-class.html)
是一个单例模式，它暴露了一个flutter调度器的原函数.

在本次讨论中，关键原函数是帧回调函数。当每一帧在屏幕上渲染时，flutter引擎都会触发"begin frame"回调
函数，调度器使用方法[`scheduleFrameCallback()`](https://docs.flutter.io/flutter/scheduler/SchedulerBinding/scheduleFrameCallback.html)以多路复用的方式回调给所有注册了监听器的单位.

## Tickers

[`Ticker`](https://docs.flutter.io/flutter/scheduler/Ticker-class.html)
 类挂载了调度器的
[`scheduleFrameCallback()`](https://docs.flutter.io/flutter/scheduler/SchedulerBinding/scheduleFrameCallback.html)方法,每当帧变化时都会调用该回调.

`Ticker`可以被启动和停止，调用开始方法后会返回一个`Future`任务，该任务将在`Ticker`被停止后才会完成.

因为`Tickers`回调时提供的经过时间总是相对与第一个Ticker的启动时间，所以`Tickers`永远都是同步执行的.
假设如果你再两帧之间的不同时间内分别启动了3个Ticker，尽管如此他们仍然会以相同的启动时间同步执行，随后按照固定的步骤滴答.

## Simulations

[`Simulation`](https://docs.flutter.io/flutter/physics/Simulation-class.html)
是一个抽象类，它将一个相对的时间值（经过的时间）映射成一个double值，并且`Simulation`有一个完成的状态.

原则上，`Simulation`是无状态的，但在实践中是一些模拟(比如说,
[`BouncingScrollSimulation`](https://docs.flutter.io/flutter/widgets/BouncingScrollSimulation-class.html) 和
[`ClampingScrollSimulation`](https://docs.flutter.io/flutter/widgets/ClampingScrollSimulation-class.html))
当需要时，他们会不可逆的改变状态值

这里有一些关于`Simulation` 的[实践](https://docs.flutter.io/flutter/physics/physics-library.html)，实现了一些不同的物理特效.

## Animatables

[`Animatable`](https://docs.flutter.io/flutter/animation/Animatable-class.html)
是一个抽象类，将一个特定的值映射成一个double值

`Animatable`类是无状态和不可变的。

### Tweens

[`Tween`](https://docs.flutter.io/flutter/animation/Tween-class.html)
抽象类将[0.0-1.0]的范围值映射成一个名义上是double类型的特定值（比如`Color`值，或者其他double类型值）
它是一个`Animatable`.

它有一个输出类型（`T`），一个`begin`值和一个`end`的概念，
以及在输入的开始和结束值之间插入（`lerp`）的方法（double名义上是在[0.0-1.0】范围内）.

`Tween` 是一个无状态不可变的类.

### Composing animatables

将`Animatable <double>`（父类）传递给`Animatable`
`chain（）`方法来创建一个新的`Animatable`子类，它采用了
父类的映射然后是子类的映射.

## Curves

[`Curve`](https://docs.flutter.io/flutter/animation/Curve-class.html)
抽象类将[0.0-1.0]的范围内的double值映射成一个[0.0-1.0]范围内的double值.

`Curve` 是一个无状态不可变的类.

## Animations

[`Animation`](https://docs.flutter.io/flutter/animation/Animation-class.html)
抽象类提供了一个给定类型的值，一个动画方向和动画状态的概念，
以及一个监听器接口用来注册值或状态变化时调用的回调.

一些 `Animation`的子类持续有的值永远不会改变
([`kAlwaysCompleteAnimation`](https://docs.flutter.io/flutter/animation/kAlwaysCompleteAnimation-constant.html),
[`kAlwaysDismissedAnimation`](https://docs.flutter.io/flutter/animation/kAlwaysDismissedAnimation-constant.html),
[`AlwaysStoppedAnimation`](https://docs.flutter.io/flutter/animation/AlwaysStoppedAnimation-class.html));
在这些动画上注册回调将永远不会被触发.

`Animation<double>`是一个特殊的变体，因为他被用在输入值希望是在[0.0-1.0]范围内的double值的类上，
比如	`Curve` 和 `Tween` 类，以及其他一些`Animation`的子类.

一些`Animation`子类是无状态的，只是转发监听器给他们的父类. 有些是有状态的.

### Composable animations

大部分`Animation` 子类都有明确的"父类"`Animation<double>`.
他们被父类所驱动.

`CurvedAnimation`子类采用`Animation <double>`类（父类）和几个`Curve`类（正向和反向曲线）
作为输入，并使用父值作为输入曲线确定其输出。 `CurvedAnimation`是不可变的无状态的.

`ReverseAnimation`子类将`Animation <double>`类作为
它的父级并反转动画的所有值。 它假定父类使用名义上在0.0-1.0范围内的值并返回
值范围为1.0-0.0。 父类的状态和动画方向也是逆转的.
 `ReverseAnimation`是不可变的无状态的.

`ProxyAnimation`子类将`Animation <double>`类作为它的父类
只是转发父类的当前状态。但是，父类是可变的

`TrainHoppingAnimation`子类需要两个父类，当他们的值交叉时在两者之间进行切换.

### Animation Controllers

该[`AnimationController`](https://docs.flutter.io/flutter/animation/AnimationController-class.html)
是有状态的`Animation <double>`，它使用`Ticker`来给提供自己的生命周期。
它可以启动和停止. 每个滴答都需要花费时间，自从它启动后通过`Simulation`来获取值.
然后他将值报告出去。如果是`Simulation`报告值的时候`Ticker`停止了，那么控制器也会把它自己给停止.

动画控制器可以给出两者动画之间的下限和上限和持续时间.

在简单的情况下(使用`forward()`，`reverse()`，`play()`，或
`resume()`)，动画控制器只是根据上限和下限在给定的时间内做一个线性的插值（反向动画亦然）.

当使用`repeat())`时，动画控制器在给定时间内给定边界之间的使用线性插值，
并且不会停止.

使用`animateTo()`时，动画控制器会在给定时间内从当前值到目标值进行线性化的插值.
如果方法没有给出持续时间，则为默认使用控制器的持续时间和控制器描述的范围
这个时候，上限下限的范围将决定动画的速度.

当使用`fling()`时，`Force`用于创建特定的模拟器，然后用于驱动控制器.

当使用`animateWith()`时，给定的模拟被用于驱动控制器.

这些方法都返回了“Ticker”提供的Future，这些Future将在控制器下次停止或者改变模拟器时完成结束.

### Attaching animatables to animations

将`Animation <double>`（父类）传递给`Animatable`的`animate（）`方法创建一个新的`Animation`子类，
这种写法作用类似于`Animatable`，但是来是由给定父类来驱动动画.
