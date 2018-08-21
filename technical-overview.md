---
title: Flutter框架概览
layout: page
permalink: /technical-overview/
summary: Flutter是一款移动应用程序SDK，一份代码可以同时生成iOS和Android两个高性能、高保真的应用程序。目标是使开发人员能够交付在不同平台上都感觉自然流畅的高性能应用程序
keywords: Flutter框架
---

## Flutter是什么?

Flutter是一款移动应用程序SDK，一份代码可以同时生成iOS和Android两个高性能、高保真的应用程序。

Flutter目标是使开发人员能够交付在不同平台上都感觉自然流畅的高性能应用程序。我们兼容滚动行为、排版、图标等方面的差异。

<object type="image/svg+xml" data="/images/whatisflutter/hero-shrine.svg" style="width: 100%; height: 100%;"></object>

这是一个来自[Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery/lib/demo)的演示应用程序，
您可以在安装Flutter并设置好环境后运行Flutter示例应用程序。“Shrine”示例拥有高质量的滚动图片、互动卡片、按钮、下拉列表和购物车页面。
要查看这个和更多示例的代码，请访问我们的[GitHub](https://github.com/flutter/flutter/tree/master/examples)。


无需移动开发经验即可开始使用。应用程序是用[Dart](https://dartlang.org/)语言编写的，如果您使用过Java或JavaScript之类的语言，则该应用程序看起来很熟悉。
使用面向对象语言的经验绝对有帮助，但一些Flutter应用程序甚至是没有编程经验的人写的！

## 为什么要使用Flutter?

Flutter有什么优势？它可以帮助你：

*   提高开发效率
    *   同一份代码开发iOS和Android
    *   用更少的代码做更多的事情
    *   轻松迭代
        *   在应用程序运行时更改代码并重新加载（通过热重载）
        *   修复崩溃并继续从应用程序停止的地方进行调试
*   创建美观，高度定制的用户体验
    *   受益于使用Flutter框架提供的丰富的Material Design和Cupertino（iOS风格）的widget
    *   实现定制、美观、品牌驱动的设计，而不受原生控件的限制

## 核心原则

Flutter包括一个现代的响应式框架、一个2D渲染引擎、现成的widget和开发工具。这些组件可以帮助您快速地设计、构建、测试和调试应用程序。

### 一切皆为widget

Widget是Flutter应用程序用户界面的基本构建块。每个Widget都是用户界面一部分的不可变声明。
与其他将视图、控制器、布局和其他属性分离的框架不同，Flutter具有一致的统一对象模型：widget。

Widget可以被定义为:

*   一个结构元素（如按钮或菜单）
*   一个文本样式元素（如字体或颜色方案）
*   布局的一个方面（如填充）
*   等等...

Widget根据布局形成一个层次结构。每个widget嵌入其中，并继承其父项的属性。没有单独的“应用程序”对象，相反，根widget扮演着这个角色。

您可以通过告诉框架使用另一个widget替换层次结构中的widget来响应事件，例如用户交互，替换后框架会比较新的和旧的widget，并高效地更新用户界面。

#### 组合 > 集成

Widget本身通常由许多更小的、单一用途widget组成，这些widget结合起来产生强大的效果。例如，[Container](https://github.com/flutter/flutter/blob/master/packages/flutter/lib/src/widgets/container.dart)是一个常用的widget，
由多个widget组成，这些widget负责布局、绘制、定位和调整大小。具体来说，Container由
[LimitedBox](https://docs.flutter.io/flutter/widgets/LimitedBox-class.html)、
[ConstrainedBox](https://docs.flutter.io/flutter/widgets/ConstrainedBox-class.html)、
[Align](https://docs.flutter.io/flutter/widgets/Align-class.html)、
[Padding](https://docs.flutter.io/flutter/widgets/Padding-class.html)、
[DecoratedBox](https://docs.flutter.io/flutter/widgets/DecoratedBox-class.html)、
和[Transform](https://docs.flutter.io/flutter/widgets/Transform-class.html)组成。
您可以用各种方式组合这些以及其他简单的widget，而不是继承容器。

类层次结构很浅且很宽，可以最大限度地增加可能的组合数量。

<object type="image/svg+xml" data="/images/whatisflutter/diagram-widgetclass.svg" style="width: 100%; height: 100%;"></object>

您还可以通过与其他widget组合来控制widget的布局。例如，要将widget居中，可以将其封装在Center widget中。有填充、对齐、行、列和网格的widget。
这些布局widget没有自己的可视化表示。相反，他们唯一的目的是控制另一个widget布局的某些方面。要理解widget以某种方式呈现的原因，检查相邻widget通常很有帮助。

#### 分层的框架

Flutter框架是一个分层的结构，每个层都建立在前一层之上。

<object type="image/svg+xml" data="/images/whatisflutter/diagram-layercake.svg" style="width: 85%; height: 85%"></object>

该图显示了框架的上层，它比下层的使用频率更高。有关构成Flutter分层框架的完整库，请参阅我们的[API文档](https://docs.flutter.io)。

这个设计的目标是帮助你用更少的代码做更多的事情。例如，Material层是通过组合来自Widget层的基本Widget来构建的，
并且Widgets层本身是通过较低级对象渲染层构建的。

层为构建应用程序提供了许多选项。选择一种自定义的方法来释放框架的全部表现力，或者使用构件层中的构建块，或混合搭配。
您可以实现Flutter提供的所有现成的widget，或者使用Flutter团队用于构建框架的相同工具和技术创建您自己的定制widget。

没有什么是隐藏的。您可以从高层次，统一的widget概念中获得开发效率优势，而不会牺牲您希望深入到下层的能力。

### 构建widget

您可以通过实现widget的[build](https://docs.flutter.io/flutter/widgets/StatelessWidget/build.html)返回widget树（或层次结构）来定义widget的独特特征 。
这棵树更具体地表示了用户界面的widget层次。例如，工具栏widget的build函数可能返回一个包含一些文本和各种按钮的[水平布局](https://docs.flutter.io/flutter/widgets/Row-class.html)。
然后，框架递归地构建widget，直到该所有widget构建完成，然后framework将他们一起添加到树中。

widget的构建函数一般没有副作用。每当它被要求构建时，widget应该返回一个新的widget树，无论widget以前返回的是什么。
Framework会将之前的构建与当前构建进行比较并确定需要对用户界面进行哪些修改。

这种自动比较非常有效，可以实现高性能的交互式应用程序。而构建函数的设计则着重于声明widget是由什么构成的，而不是将用户界面从一个状态更新到另一个状态的(这很复杂性)，从而简化了代码。

### 处理用户交互

如果widget需要根据用户交互或其他因素进行更改，则该widget是有状态的。例如，如果一个widget的计数器在用户点击一个按钮时递增，那么该计数器的值就是该widget的状态。
当该值发生变化时，需要重新构建widget以更新UI。

这些widget将继承[StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)（而不是[State](https://docs.flutter.io/flutter/widgets/State-class.html)）并将它们的可变状态存储在State的子类中。

<object type="image/svg+xml" data="/images/whatisflutter/diagram-state.svg" style="width: 85%; height: 85%"></object>

每当你改变一个State对象时（例如增加计数器），你必须调用`setState()`来通知框架，框架会再次调用State的构建方法来更新用户界面。
有关管理状态的示例，请参阅创建新Flutter项目时的[MyApp 模板](https://github.com/flutter/flutter/blob/master/packages/flutter_tools/templates/create/lib/main.dart.tmpl) 。


有了独立的状态和widget对象，其他widget可以以同样的方式处理无状态和有状态的widget，而不必担心丢失状态。
父widget可以自由地创造子widget的新实例且不会失去子widget的状态，而不是通过持有子widget来维持其状态。
框架在适当的时候完成查找和重用现有状态对象的所有工作。

## 试试!

现在您已经熟悉了Flutter框架的基本结构和原理，以及如何构建应用程序并和其交互，您就可以开始开发和迭代了。

接下来的步骤:

1.  查看[Flutter 入门指南](/get-started/).
1.  尝试[在Flutter中构建布局](/tutorials/layout/)和
    [给Flutter App添加交互](/tutorials/interactive/).
1.  查看[Widget框架概览](/widgets-intro/).

## 支持


Flutter是开源的，我们很乐意听到你的声音！

我们鼓励您以各种方式参与并加入对话。

- [询问可以用特定解决方案回答的HOWTO问题][so]
- [与Flutter工程师和用户进行实时聊天][gitter]
- [在我们的邮件列表中讨论Flutter、最佳实践、应用程序设计等等][mailinglist]
- [提交bugs并提出新功能和文档请求][issues]
- [关注我们的Twitter: @flutterio](https://twitter.com/flutterio/)


[issues]: https://github.com/flutter/flutter/issues
[apidocs]: https://docs.flutter.io
[so]: https://stackoverflow.com/tags/flutter
[mailinglist]: https://groups.google.com/d/forum/flutter-dev
[gitter]: https://gitter.im/flutter/flutter
