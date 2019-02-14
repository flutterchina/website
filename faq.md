---
layout: page
title: Flutter FAQ
permalink: /faq/
summary: 关于Flutter的常见问题
keywords: Flutter常见问题
---

* Placeholder for TOC
{:toc}

## 介绍

### Flutter是什么?

Flutter是一款移动应用程序SDK，包含框架、widget和工具，为开发人员提供了一种在Android和iOS上构建和部署精美移动应用程序的简单高效的方式。

### Flutter能做什么?

对于用户来说，Flutter可以使应用界面变得美丽生动。

对于开发者来说，Flutter降低了开发移动应用程序的门槛。它加速了移动应用程序的开发过程，并降低了同时开发iOS和Android两套应用程序的成本和复杂性。

对于设计师来说，Flutter有助于实现原始设计愿景，高保真度、不妥协。它也是一种高效的原型工具。

### Flutter适合谁?

Flutter适用于希望以更快的方式构建漂亮的移动应用程序的开发人员，或者通过单一研发投入得更多用户的方式(同一份代码支持IOS和Android)。

Flutter也适用于需要领导移动开发团队的工程经理。
Flutter允许经理创建一个移动应用开发团队(而不是IOS和Android两个团队)，统一大家的开发方式以更快地向向iOS和Android发布相同的功能，同时，并降低维护成本。

虽然不是最初的目标受众，但Flutter也适用于那些希望将其原始设计愿景以高保真度呈现给所有移动用户的设计师。

从根本上讲，Flutter适用于那些想要漂亮的应用程序、令人愉快的交互和动画以及具有个性的用户界面的所有人。

### 对于程序员/开发人员来说，要使用Flutter必须具备哪些经验？

Flutter对熟悉面向对象概念（类、方法、变量等）和命令式编程概念（循环、条件等）的程序员来说是很容易入门的。学习和使用Flutter，无需事先具有移动开发经验。
我们已经看到了一些不怎么有编程经验的人学习并使用Flutter进行原型设计和应用程序开发。

### Flutter可以构建什么类型的应用程序？

Flutter针对想要在Android和iOS上运行的2D移动应用进行了优化。您可以使用Flutter构建全功能应用程序，包括相机、地理位置、网络、存储、第三方SDK等。

### 谁创造了Flutter?

Flutter是一个开源项目，来自Google和社区的贡献。

### 谁在使用Flutter？

Google使用Flutter为iOS和Android构建关键业务的应用程序。例如，Google的移动销售工具应用程序是使用Flutter构建的，还有Google Shopping Express的Store Manager应用程序。
还有其他Flutter应用程序正在创建中，或还没有对外发布。

Google以外的团队也使用Flutter。例如，musical Hamilton的官方应用程序（Android、iOS）是用Flutter构建的。

### 是什么让Flutter如此独一无二?

Flutter与用于构建移动应用程序的其它大多数框架不同，因为Flutter既不使用WebView，也不使用操作系统的原生控件。
相反，Flutter使用自己的高性能渲染引擎来绘制widget。

另外，Flutter的不同是因为它核心只有一层轻量的C/C++代码。Flutter在Dart（一种现代的、简洁的、面向对象的语言）中实现了其它大部分系统（组合、手势、动画、框架、widget等），
开发人员可以轻松地进行读取、更改、替换或移除。这为开发人员提供了对系统的巨大可定制性。

### 我应该用Flutter制作我的下一个应用程序吗？

Flutter仍在开发中，尚未发布1.0。

我们的API正在稳定，我们将继续根据用户反馈改进系统的某些部分。当我们发布了可能会影响用户的更改，我们会发送电子邮件至flutter-dev@googlegroups.com。

Flutter在Google内部已经被使用，并且使用Flutter构建的应用程序已经发布了。

一些关键功能（如辅助功能）尚未准备好广泛部署。

所以真的，这取决于你，您需要的功能也许今天已经可用了！如果您向用户发布了使用Flutter构建的应用，请告诉我们。我们很乐意听到你正在建造的东西！

## Flutter提供什么？

### Flutter SDK里面有什么？

* 深度优化了的、移动优先的2D渲染引擎
* 现代、响应式框架
* 丰富的Android和iOS套件
* 单元和集成测试的API
* 连接到系统和第三方SDK的Interop和插件API
* 无头的测试运行器，用于在Windows、Linux和Mac上运行测试
* 用于创建、构建、测试和编译应用程序的命令行工具


### Flutter是否可以与任何编辑器或IDE配合使用？

我们提供了[Android Studio](https://developer.android.com/studio/)、 [IntelliJ IDEA](https://www.jetbrains.com/idea/)和[VS Code](https://code.visualstudio.com/)插件。

请参阅[编辑器配置](/get-started/editor/)以了解安装细节，以及 [使用IDE中开发Flutter应用程序](/using-ide/)，了解如何使用插件的提示。

或者，您可以在终端中使用`flutter`命令，然后选择一款支持[编辑Dart](https://www.dartlang.org/tools)的编辑器，组合使用。

### Flutter是否带有framework？

是! Flutter运用了受React启发的现代framework。Flutter的framework被设计为分层和可定制（可选）的。开发人员可以选择仅使用framework的一部分或不同的framework。

### Flutter是否有自己的控件(widget)集？

是! Flutter配有一套 [高品质Material Design和Cupertino（iOS风格）](/widgets/)的widget，同时包括布局和主题。当然，这些widget只是一个起点。Flutter旨在使您可以轻松创建自己的widget，或者自定义现有的widget。

### Flutter是否带有测试框架？

是的，Flutter提供了编写单元测试和集成测试的API。了解更多关于 [Flutter测试的信息](/testing/)。

我们使用测试功能来测试我们的SDK，衡量每次提交的[测试覆盖率](https://coveralls.io/github/flutter/flutter?branch=master)。

### Flutter是否带有依赖注入框架或解决方案？

目前还没有。请在[flutter-dev@googlegroups.com上](mailto:flutter-dev@googlegroups.com)分享您的想法 。

## 技术

### Flutter使用什么技术？

Flutter使用C、C ++、Dart和Skia（2D渲染引擎）构建。请参阅此 [架构图](https://docs.google.com/presentation/d/1cw7A4HbvM_Abv320rVgPVGiUP2msVs7tfGbkgdrTy0I/edit#slide=id.gbb3c3233b_0_162)以更好地了解主要组件。

### Flutter如何在Android上运行我的代码？

引擎的C/C ++代码是用Android的NDK编译的，任何Dart代码都是AOT编译成本地代码的。Flutter应用程序使用本机指令集运行（不涉及解释器）。

### Flutter如何在iOS上运行我的代码？

引擎的C/C ++代码使用LLVM编译，任何Dart代码都是AOT编译为本地代码的。Flutter应用程序使用本机指令集运行（不涉及解释器）。

### Flutter使用系统原生控件(widget)吗？

不，相反，Flutter提供了一组自己的widget（包括Material Design和Cupertino（iOS风格的widget）），由Flutter的framework和引擎管理和渲染。您可以浏览[Flutter widget](/widgets/)的[目录](/widgets/)。

我们希望最终结果是更高质量的，如果我们重新使用原生系统widget，Flutter应用的质量和性能将受到这些widget本身质量的限制。

例如，在Android中，有一组硬编码的手势和固定的规则来对它们进行手势冲突消歧。在Flutter中，您可以编写自己的手势识别器，该手势识别器是[手势系统](/gestures/)中的一级参与者 。此外，由不同人撰写的两个小工具可协调手势冲突消歧。

现代应用程序设计趋势指向需要更多动态UI和品牌优先设计的设计师和用户。为了达到这个级别的自定义、美观的设计，Flutter的架构驱动像素，而不是系统原生widget。

通过使用相同的渲染器、框架和一组widget，我们可以更轻松地同时发布iOS和Android应用，而无需执行仔细和昂贵的计划来维护两个独立的代码库。

通过为所有用户界面使用一种语言、一种框架和一套相同的库，我们还致力于降低应用程序开发和维护成本。

### 当我的手机操作系统更新并引入新的控件时会发生什么？

Flutter团队密切关注iOS和Android的新控件的采用和需求，旨在与社区合作构建对新控件的支持。

Flutter的分层架构旨在支持大量其它widget库，我们鼓励和支持社区构建和维护widget库。

### 当我的移动操作系统更新并引入新平台功能时会发生什么？

Flutter的互操作和插件系统旨在让开发人员可以随时访问新系统功能。开发人员无需等待Flutter团队公开新的移动操作系统功能。

### 我可以使用哪些操作系统来构建Flutter应用程序？

Flutter支持Linux、Mac和Windows上的开发。

### Flutter用什么语言开发？

我们研究了很多语言和运行时，最终采用了Dart作为开发框架和widget的语言。底层图形框架和Dart虚拟机在C /C++中实现。

### 为什么Flutter选择使用Dart语言？

Flutter在四个主要维度进行了评估，并考虑了框架作者、开发人员和最终用户的需求等因素。我们发现不同的语言在不同的层面符合一部分需求，但Dart在所有评估维度上得分都很高，并且符合我们的所有要求和标准。

Dart运行时和编译器支持Flutter的两个关键特性的组合：**基于JIT的快速开发周期**：允许使用类型的语言进行形状更改和有状态的热重载；**以及AOT编译器**，可生成高效的ARM代码，可以快速启动并拥有可预测的生产部署性能。

此外，我们有机会与Dart社区密切合作，Dart社区正在积极投入资源改进Dart在Flutter中的使用。例如，当我们采用Dart时，该语言没有提供生成原生二进制文件的工具链（这对于实现可预测的高性能是很有帮助的），但是现在实现了，因为Dart团队为Flutter构建了它。同样，Dart VM之前已经针对吞吐量进行了优化，但团队现在正在优化VM的延迟时间，这对于Flutter的工作负载更为重要。

Dart在以下主要标准上得到高分：

- **开发人员的效率**。Flutter的主要价值主张之一是通过让开发人员使用相同的代码库为iOS和Android创建应用程序，从而节省了工程资源。使用高效的语言可以进一步加速开发周期，并使Flutter更具吸引力。这对我们的framework团队和开发人员都非常重要。大部分Flutter功能都是用Dart实现，因此我们需要在10万行代码时能保持高效的而不会牺牲framework和widget的可读性。
- **面向对象**。虽然我们可以使用非面向对象的语言，但这意味着要重新解决几个难题。另外，绝大多数开发人员都具有面向对象开发的经验，因此更容易学习如何使用Flutter进行开发。
- **可预测，高性能**。借助Flutter，我们希望使开发人员能够快速创建流畅的用户体验。为了实现这一点，我们需要能够在每个动画帧中运行大量的代码。这意味着我们需要一种既能提供高性能又能提供可预测性能的语言，而不会出现会导致丢帧的周期性暂停。
- **快速内存分配**。Flutter框架使用函数式流，它很大程度上依赖于底层的内存分配器，从而有效地处理小的、短期的内存分配会非常重要，所以在缺乏此功能的语言中Flutter无法有效地工作。

### Flutter可以运行任何Dart代码？

Flutter能够运行大多数不会直接或间接导入dart:mirrors 或 dart:html的代码。

### Flutter引擎有多大？

2018年12月，我们测量了一个[最小的 Flutter 应用](https://github.com/flutter/flutter/tree/75228a59dacc24f617272f7759677e242bbf74ec/examples/hello_world)（不含 Material 组件，仅有一个 `Center` 控件，使用 `flutter build apk` 构建）的下载大小，并打包为 release 版本，大小约为 4.06 MB.

对于这个简单的应用，核心引擎大约为 2.7MB(已压缩)，框架和应用程序代码大约 820.6KB (已压缩)，LICENSE 文件为 53.5KB(已压缩)，必要的 Java 代码 (classes.dex) 为 61.8KB(已压缩)，此外还有大约 450.4KB(已压缩)的 ICU 数据。

这些数据是通过 [apkanalyzer](https://developer.android.com/studio/command-line/apkanalyzer) 测量得到的，它也是 [Android Studio](https://developer.android.com/studio/build/apk-analyzer) 内置的分析工具。

在 iOS 上，同一个应用的 release 版本 IPA 文件在 iPhone X 上的下载大小为 10.8MB，数据通过苹果的 App Store Connect 得到。IPA 比 APK 更大的主要原因是苹果加密了 IPA 中的二进制文件，这使得压缩的效果有所折扣（参见苹果 [QA1795](https://developer.apple.com/library/archive/qa/qa1795/_index.html) 的 [iOS App Store特定注意事项](https://developer.apple.com/library/archive/qa/qa1795/_index.html#//apple_ref/doc/uid/DTS40014195-CH1-APP_STORE_CONSIDERATIONS)章节）。

当然，数据因人而异，我们也推荐你测量你自己的应用。要测量 Android 应用，请执行 `flutter build apk` 并将 APK (`build/app/outputs/apk/release/app-release.apk`) 导入 Android Studio 中以查看具体的尺寸报告。要测量 iOS 应用，将发行版的 IPA 上传到 App Store Connect 中并在那里获取尺寸报告。

## 功能

### Flutter应用程序性能如何？

Flutter应用程序性能非常出色。Flutter旨在帮助开发人员轻松实现恒定的60fps。Flutter应用程序通过本机编译的代码运行 - 不涉及解释器。这意味着Flutter应用程序可以快速启动并执行。

### Flutter开发体验如何？编辑和刷新之间有多长时间？

Flutter实现了**热重载**开发循环。您可以在设备或模拟器上实现亚秒级重载。

Flutter的热重载是**有状态的**，这意味着应用程序状态在重载后仍然会保留。所以您可以在应用程序中各个页面快速迭代开发，而无需在每次重新加载后都要从主屏幕重新开始。

### “热重载”与“完全重新启动”有何不同？

Hot Reload通过将更新的源代码文件注入正在运行的Dart VM（虚拟机）中工作。这不仅包括添加新类，还包括向现有类添加方法和字段，以及更改现有函数。尽管有几种类型的代码更改无法热重新：

- 全局变量初始化器。
- 静态字段初始化程序。
- `main()`应用程序的方法。

请参阅[使用Hot Reload](/hot-reload/)获取更多详细信息。

### Flutter支持哪些设备和操作系统版本？

移动操作系统：Android Jelly Bean，v16,4.1.x或更新的版本以及iOS 8或更新版本。

移动硬件：64位iOS设备（从iPhone 5S和更新的iPhone型号开始）以及ARM Android设备。

请注意，我们目前不支持：

- ARM32 iOS设备（iPhone 4，iPhone 5;问题[＃2089](https://github.com/flutter/flutter/issues/2089)）
- x86 Android设备（问题[＃9253](https://github.com/flutter/flutter/issues/9253)）

我们支持使用Android和iOS设备(包括Android模拟器和iOS模拟器)来开发测试Flutter应用。

我们测试了各种低端到高端手机，但我们还没有官方设备兼容性保证。

我们不会在平板电脑上进行测试，因此平板电脑上的某些widget可能会出现问题。我们尚未在我们的widget中实施针对平板电脑的改动。

### Flutter是否可以在web上运行？

我们目前正在做一个实验性的项目 [Hummingbird](https://medium.com/flutter-io/hummingbird-building-flutter-for-the-web-e687c2a023a8)。它是一个基于 web 的 Flutter 运行时实现，利用 Dart 平台的能力编译为 JavaScript。这允许 Flutter 代码运行在标准的 web 上而无需改动。

### 我可以使用Flutter构建桌面应用程序吗？

我们专注于移动端优先。但是，Flutter是开源的，我们鼓励社区以各种有趣的方式使用Flutter。

### 我可以在我现有的原生应用程序中使用Flutter吗？

可以，您可以在现有的Android或iOS应用中嵌入Flutter，但是我们的工具目前尚未完全针对此用例进行优化（请参阅[此问题](https://github.com/flutter/flutter/issues/14821)以了解详细信息）。

目前的两个示例是[platform_view](https://github.com/flutter/flutter/tree/master/examples/platform_view) 和[flutter_view](https://github.com/flutter/flutter/tree/master/examples/flutter_view) 示例。一些开始文档可以在wiki页面中 [添加Flutter到现有的应用程序](https://github.com/flutter/flutter/wiki/Add-Flutter-to-existing-apps)。

### 我可以访问平台服务和API，如传感器和本地存储吗？

可以。Flutter为开发人员对操作系统中某些平台特定服务和API提供了访问支持，但是为了尽可能多的功能通用，Flutter本身不会提供太多特定于平台的动能，您可以使用平台通道自己定制。

许多平台服务和API都在Pub仓库中提供了[现成的软件包](https://pub.dartlang.org/flutter/)。使用现有的软件包[非常简单](/using-packages/)。

最后，我们鼓励开发人员使用Flutter的异步消息传递系统与[平台和第三方API](/platform-channels/)创建自己的集成。开发人员可以根据需要公开尽可能多的平台API，并构建最适合他们项目的抽象层。

### 我可以扩展和自定义widget吗？

当然可以！Flutter的widget系统被设计为易于定制的。

不同于让每个widget提供大量的参数的思路，Flutter拥抱**组合**：widget由较小的widget构建而成，您可以以各种方式组合，以制作自定义widget。例如，RaisedButton将一个Material widget与一个GestureDetector widget组合在一起，而不是对一个通用按钮widget进行子类化。Material widget提供了可视化设计，而GestureDetector widget提供了交互设计。

要使用自定义视觉设计创建按钮，可以将实现视觉设计的widget与提供交互设计的GestureDetector结合使用。例如，CupertinoButton遵循这种方法，将GestureDetector与其他几个实现其视觉设计的widget结合在一起。

组合可以最大限度地控制widget的视觉和交互，同时还可以重复使用大量的代码。在框架中，我们已经将复杂的widget分解为各个部分，分别实现视觉、交互和动画设计，您可以将这些widget重新组合。

### 为什么要在iOS和Android上共享布局代码？

您可以选择为iOS和Android实现不同的应用布局。开发人员可以在运行时检查移动操作系统并呈现不同的布局，但我们发现这种做法很少见。

我们看到越来越多的移动应用布局和设计发展为品牌优先的跨平台统一的风格。这意味着在iOS和Android上共享布局和UI代码的意愿非常强烈。

与严格遵守传统平台美学相比，应用程序美学设计的品牌标识和定制现在变得越来越重要。例如，应用程序设计通常需要自定义字体、颜色、形状、动作效果等以清楚地表达其品牌标识。

我们还可以看到在iOS和Android上通用的布局模式。例如，现在可以在iOS和Android上自然找到“底部导航栏”模式。移动平台似乎有设计思想的融合。

### 可以与移动平台的默认编程语言进行交互吗？

可以，Flutter支持与原生之间通过[platform channels](/platform-channels/)进行功能互调，这是通过灵活的消息传递模式支持的。

### Flutter是否带有reflection/mirrors系统？

目前还没有。由于Flutter应用程序是针对iOS预编译的，并且二进制大小始终是移动应用程序关注的问题，因此我们禁用了reflection/mirrors 。我们很好奇您可能需要反射/镜像的场景 - 请通过[flutter-dev@googlegroups.com](mailto:flutter-dev@googlegroups.com)与我们[联系](mailto:flutter-dev@googlegroups.com)。

### 如何在Flutter中进行国际化(i18n)、本地化(l10n)和实现辅助功能(a11y)？

在[Flutter国际化教程中](/tutorials/internationalization/)了解更多关于i18n和l10n的信息 。

Flutter内置了对基本辅助功能的支持，包括Android和iOS上的屏幕阅读器。查看[语义](https://docs.flutter.io/flutter/widgets/Semantics-class.html) widget，了解如何自定义Flutter应用的辅助功能。

我们鼓励您将有关这些功能的问题通过电子邮件[flutter-dev@googlegroups.com](mailto:flutter-dev@googlegroups.com) 发给我们。

### 如何为Flutter编写并行/并发应用程序？

Flutter支持isolates。isolates是Flutter虚拟机中的独立堆，它们可以并行运行（通常作为单独的线程实现）。isolates通过发送和接收异步消息进行通信。虽然我们正在为此评估解决方案，但Flutter目前还没有共享内存并行解决方案。

查看一个[在Flutter中使用isolates的例子](https://github.com/flutter/flutter/blob/master/examples/layers/services/isolate.dart)。

### 我可以在Flutter应用程序的后台运行Dart代码吗？

是的，你可以在 iOS 和 Android 的后台进程里执行 Dart 代码。更多信息请阅读 Medium 文章 [Executing Dart in the Background with Flutter Plugins and Geofencing](https://medium.com/flutter-io/executing-dart-in-the-background-with-flutter-plugins-and-geofencing-2b3e40a1a124).

### 我可以在Flutter中使用JSON / XML / protobuffers...吗？

当然可以！在[pub.dartlang.org](https://pub.dartlang.org/)中有处理JSON、XML、protobufs和许多其他格式的库。

有关如何在Flutter中使用JSON的教程，请参阅[JSON教程](/json/)。

### 我可以使用Flutter构建3D（OpenGL）应用程序吗？

我们暂时不支持OpenGL ES或类似的3D。长期有计划支持，但现在我们专注于2D。

### 为什么我的APK或IPA如此之大？

通常，资源包括图像、声音文件、字体等，这些是APK或IPA的大部分。Android和iOS生态系统中的各种工具可以帮助您了解APK或IPA中的内容。

此外，请务必使用Flutter工具创建APK或IPA的发布版本。发布版本通常**远**小于调试版本。

详细了解如何创建 [Android发布版本](/android-release/)，以及如何创建[iOS应用发布版本](/ios-release/)。

### Flutter应用是否在Chromebook上运行？

我们看到Flutter应用在某些Chromebook上运行。我们正在跟踪 [与在Chromebook上运行Flutter相关的问题](https://github.com/flutter/flutter/labels/platform-arc)。

## Framework

### 为什么build()方法在State（而不是StatefulWidget）上？

将 `Widget build(BuildContext context)`方法放在State上而不是StatefulWidget上是为了在继承StatefulWidget时能更加灵活，您可以阅读 [API文档中关于State.build的详细讨论](https://docs.flutter.io/flutter/widgets/State/build.html)。

### Flutter的标记语言在哪里？为什么Flutter没有标记语法？

我们发现使用代码动态构建的UI会更灵活。例如，我们发现严格的标记系统难以表达和生成具有特定行为的widget。我们还发现，我们的“代码优先”更好地支持热重载和动态环境适应等功能。

当然也可以创建一个自定义标记语言，然后将其动态的转化为dart代码。

### 应用在右上角有一个“Slow Mode”横幅，为什么看到这个？

默认情况下，`flutter run`命令会使用调试版本配置。

调试配置在虚拟机中运行Dart代码，通过[热重载](/faq/#hot-reload)实现快速开发周期（发布版本使用标准[Android](/faq/#run-android)和[iOS](/faq/#run-ios) 工具链进行编译）。

调试配置还会检查所有断言，这有助于在开发过程中尽早发现错误，但会增加运行时成本。“Slow Mode”横幅表示启用了这些检查。您可以使用`--profile`或`--release`参数来通过`flutter run` 命令运行应用程序，这会关闭这些检查，如果您正在使用IntelliJ的Flutter插件，则可以通过**Run> Flutter Run in Profile Mode** 或**Release Mode**。

### Flutter框架使用什么编程范式？

Flutter是一个多范式编程环境。在Flutter中使用了过去几十年中开发的许多编程技术。我们使用的每一个范式都是我们相信该它的优势特别适合Flutter：

- **组合**：Flutter使用的主要范例是使用小对象，然后将它们组合在一起以获得更复杂的对象。Flutter widget库中的大多数widget都是以这种方式构建的。例如，Material [FlatButton](https://docs.flutter.io/flutter/material/FlatButton-class.html) 类是使用[MaterialButton](https://docs.flutter.io/flutter/material/MaterialButton-class.html) 类构建， 该类本身使用[IconTheme](https://docs.flutter.io/flutter/widgets/IconTheme-class.html)、[InkWell](https://docs.flutter.io/flutter/material/InkWell-class.html)、[Padding](https://docs.flutter.io/flutter/widgets/Padding-class.html)、[Center](https://docs.flutter.io/flutter/widgets/Center-class.html)、[Material](https://docs.flutter.io/flutter/material/Material-class.html)、[AnimatedDefaultTextStyle](https://docs.flutter.io/flutter/widgets/AnimatedDefaultTextStyle-class.html)和[ConstrainedBox](https://docs.flutter.io/flutter/widgets/ConstrainedBox-class.html)组合 构建。该[InkWell](https://docs.flutter.io/flutter/material/InkWell-class.html) 使用内置[GestureDetector](https://docs.flutter.io/flutter/widgets/GestureDetector-class.html)。[Material](https://docs.flutter.io/flutter/material/Material-class.html) 是使用内置[AnimatedDefaultTextStyle](https://docs.flutter.io/flutter/widgets/AnimatedDefaultTextStyle-class.html)、[NotificationListener](https://docs.flutter.io/flutter/widgets/NotificationListener-class.html)和[AnimatedPhysicalModel](https://docs.flutter.io/flutter/widgets/AnimatedPhysicalModel-class.html)。等等，它们都是widget。
- **函数式编程**：整个应用程序可以仅使用[StatelessWidget](https://docs.flutter.io/flutter/widgets/StatelessWidget-class.html)来构建 ，这些函数本质上是描述参数如何映射到其他函数的函数。在计算布局或绘制图形的底层（这些应用程序不拥有状态，因此通常是非交互式的）例如，[Icon](https://docs.flutter.io/flutter/widgets/Icon-class.html) widget本质上是一个将其参数（[颜色](https://docs.flutter.io/flutter/widgets/Icon/color.html)、 [icon](https://docs.flutter.io/flutter/widgets/Icon/icon.html)、[size](https://docs.flutter.io/flutter/widgets/Icon/size.html)）映射到布局基本单元的函数。此外，大量使用的是不可变数据结构，包括整个[Widget](https://docs.flutter.io/flutter/widgets/Widget-class.html)类层次结构以及许多支持类，如 [Rect](https://docs.flutter.io/flutter/dart-ui/Rect-class.html)和 [TextStyle](https://docs.flutter.io/flutter/painting/TextStyle-class.html)也都是。Dart中的[Iterable](https://docs.flutter.io/flutter/dart-core/Iterable-class.html) API经常用来处理框架中的值列表，它大量使用了函数式（map，reduce，where等）方法。
- **事件驱动**：用户交互由事件对象表示，这些事件对象被分派给注册了事件处理程序的回调。屏幕刷新也由类似的回调机制触发。[监听](https://docs.flutter.io/flutter/foundation/Listenable-class.html)类是动画系统的基础，它确立了与多个监听事件订阅模式。
- **基于类的面向对象编程**：框架的大部分API都是使用继承类来构建的。我们使用一种方法来在基类中定义非常抽象的API，然后在子类中迭代地对它们进行定制化。例如，我们的渲染对象有一个与坐标系无关的基类（[RenderObject](https://docs.flutter.io/flutter/rendering/RenderObject-class.html)），然后我们有一个子类（[RenderBox](https://docs.flutter.io/flutter/rendering/RenderBox-class.html)），它引入了基于笛卡尔坐标系（x / width）和Y /高度）。
- **基于原型的面向对象编程**： [ScrollPhysics](https://docs.flutter.io/flutter/widgets/ScrollPhysics-class.html) 类将实例链接起来组成适用于在运行时动态滚动的physics。这使得系统可以编写包含特定平台physics的分页physics，而无需在编译时选择平台。
- **命令式编程**：直接命令式编程通常与对象内部封装的状态配对，用于提供最直观的解决方案。例如，测试是以一种强制性风格编写的，首先描述测试中的情况，然后列出测试必须匹配的不变量，然后根据测试需要推进时钟或插入事件。
- **响应式编程**：widget和元素树有时被描述为响应式的，因为在widget的构造函数中提供的新输入会立即作为widget的构建方法对较低级别widget的更改传播，并在较低widget中进行更改（例如，作为响应到用户输入）通过事件处理程序传播回widget树。根据widget的需求，功能向应和命令响应两方面都存在于框架中。具有构建方法的widget仅由一个表达式组成，该表达式描述了widget如何对其配置中的变化做出反应的功能响应widget（例如，[Divider](https://docs.flutter.io/flutter/material/Divider-class.html)类）。 构建方法通过几个语句构建子项列表的widget，描述了widget如何对其配置中的更改作出反应，这些都是命令性响应widget（例如 [Chip](https://docs.flutter.io/flutter/material/Chip-class.html)类）。
- **声明式编程**：widget的构建方法通常是具有多层嵌套构造函数的单一表达式，使用Dart的严格声明子集编写。这样的嵌套表达式可以机械地转换成任何适合表达的标记语言或从任何适合表达的标记语言转换。例如， [UserAccountsDrawerHeader](https://docs.flutter.io/flutter/material/UserAccountsDrawerHeader-class.html) widget具有很长的构建方法（20行以上），由单个嵌套表达式组成。这也可以与命令式风格相结合来构建用纯粹声明式方法难以描述的UI。
- **泛型**：类型可用于帮助开发人员及早发现编程错误。Flutter框架使用泛型编程来处理这个问题。例如， [State](https://docs.flutter.io/flutter/widgets/State-class.html)类根据其关联widget的类型进行参数化，以便Dart分析器可以捕获状态和widget的不匹配。类似地， [GlobalKey](https://docs.flutter.io/flutter/widgets/GlobalKey-class.html)类需要的类型参数，以便它可以访问远程widget的状态下在一个类型安全的方式（使用运行时检查）， [路由](https://docs.flutter.io/flutter/widgets/Route-class.html)接口是参数化时，它是预期使用类型 [pop](https://docs.flutter.io/flutter/widgets/Navigator/pop.html)和集合，例如[List](https://docs.flutter.io/flutter/dart-core/List-class.html)、[Map](https://docs.flutter.io/flutter/dart-core/Map-class.html)和 [Set](https://docs.flutter.io/flutter/dart-core/Set-class.html)都是参数化的，这样可以在分析过程中或在调试期间的运行时提前捕获不匹配的元素。
- **并发**：Flutter大量使用 [Future](https://docs.flutter.io/flutter/dart-async/Future-class.html)和其他异步API。例如，动画系统通过Future来完成动画完成时的通知。图像加载系统同样使用Future在加载完成时进行报告。
- **约束**：Flutter中的布局系统使用弱形式的约束编程来确定场景的几何形状。约束（例如，对于笛卡尔盒子，最小和最大宽度以及最小和最大高度）从父母传递给孩子，并且孩子选择生成的几何结构（例如，对于笛卡尔盒子，大小，特别是宽度和高度）满足这些限制。通过使用这种技术，Flutter通常可以通过一次遍布整个场景。

## 项目

### 我在哪里可以获得支持？

如果您认为遇到了 bug ，请提交到我们的[issue tracker](https://github.com/flutter/flutter/issues)。我们鼓励您使用 [Stack Overflow](https://stackoverflow.com/tags/flutter)来处理“HOWTO”类型的问题。有关讨论，请加入我们的邮件列表 [flutter-dev@googlegroups.com](mailto:flutter-dev@googlegroups.com)。


### Flutter是开源的吗？

是的，Flutter是开源技术。你可以在[GitHub上](https://github.com/flutter/flutter)找到这个项目。

### 我如何参与Flutter项目？

Flutter是开源的，我们鼓励你贡献。您可以先简单地提交功能请求和错误[issue](https://github.com/flutter/flutter/issues)

我们建议您加入我们的邮件列表 [flutter-dev@googlegroups.com，](mailto:flutter-dev@googlegroups.com)并告诉我们您如何使用Flutter以及您想如何使用它。

如果您对提供代码感兴趣，可以先阅读我们的 [“参与指南”](https://github.com/flutter/flutter/blob/master/CONTRIBUTING.md) 并查看我们的[轻松入门issue](https://github.com/flutter/flutter/issues?q=is%3Aopen+is%3Aissue+label%3A%22easy+fix%22)列表 。


### 哪些软件许可证适用于Flutter及其依赖项？

Flutter包含两个组件：一个作为动态链接二进制文件发布的引擎和作为引擎加载的单独二进制文件的Dart framework。该引擎使用多个具有许多依赖关系的软件组件; 在[这里](https://raw.githubusercontent.com/flutter/engine/master/sky/packages/sky_engine/LICENSE)查看完整列表。

该framework完全独立，只需要[一个许可证](https://github.com/flutter/flutter/blob/master/LICENSE)。

另外，您使用的任何Dart软件包可能都有自己的许可证要求。

### 我如何确定我的Flutter应用程序需要显示的许可证？

有一个API可以找到您需要显示的许可证列表：

- 如果您的应用程序具有[Drawer](https://docs.flutter.io/flutter/material/Drawer-class.html)，请添加一个[AboutListTile](https://docs.flutter.io/flutter/material/AboutListTile-class.html)。
- 如果您的应用程序没有一个Drawer但使用Material组件库，调用其中[showAboutDialog](https://docs.flutter.io/flutter/material/showAboutDialog.html)或[showLicensePage](https://docs.flutter.io/flutter/material/showLicensePage.html)。
- 对于自定义的方法，您可以从[LicenseRegistry](https://docs.flutter.io/flutter/foundation/LicenseRegistry-class.html)获取原始许可证。

### 谁在用Flutter工作？

Flutter是一个开源项目。目前，大部分的开发工作都是由Google的工程师完成的。如果您对Flutter感到兴奋，我们鼓励您加入社区并为Flutter做出贡献！

### Flutter的指导原则是什么？

我们相信：

- 为了覆盖每个潜在用户，开发人员需要针对多个移动平台。
- HTML和WebViews在一直存在着性能短板，如由于滚动、连续点击等体验都不是很好，
- 今天，多次构建相同应用程序的成本太高：它需要不同的团队，不同的代码库，不同的工作流程，不同的工具等。
- 开发人员希望使用一个简单的代码库来为多个目标平台构建移动应用程序，他们不想牺牲质量、控制和性能。

我们专注于三件事情：

- **控制** - 开发人员应该可以访问和控制系统的所有层。这导致：
- **性能** - 用户应该拥有完美的流畅、响应快、jank-free的应用程序。
- **精细** - 每个人都值得拥有精细、美观、愉快的移动应用体验。

### 苹果是否会拒绝我的Flutter应用程序？

我们不能为苹果公司发言，但多年来苹果的政策已经发生了变化，他们允许使用Flutter等系统构建的应用程序。我们看到使用Flutter构建的应用程序已经通过App Store进行了审查和发布。

当然，Apple最终负责他们的生态系统，但我们的目标是继续尽我们所能确保Flutter应用程序可以部署到Apple的App Store中。


