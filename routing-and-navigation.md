---
layout: page
title: 路由和导航
permalink: /routing-and-navigation/
summary: 本文介绍了如何在Flutter中创建新页面(Route)，并如何在页面进行跳转、传值。
keywords: Flutter路由和导航
---

大多数应用程序具有多个页面或视图，并且希望将用户从页面平滑过渡到另一个页面。Flutter的路由和导航功能可帮助您管理应用中屏幕之间的命名和过渡。

* Placeholder for TOC
{:toc}

## 核心概念

管理多个页面时有两个核心概念和类：[Route][routedoc]和 [Navigator][navigatordoc]。
一个route是一个屏幕或页面的抽象，Navigator是管理route的Widget。Navigator可以通过route入栈和出栈来实现页面之间的跳转。

要开始使用，我们建议您阅读[Navigator的API文档](navigatordoc)。在那里，您将了解各种路由、命名路由、路由过度等等。

## 例子

[stocks 示例][stocks]是一个[MaterialApp][materialappdoc]，其中有如何声明命名route的一个简单的例子

[Navigator API 文档][navigatordoc] 包含了一些路由跳转、命名路由、路由返回数据等多个示例

[routedoc]: https://docs.flutter.io/flutter/widgets/Route-class.html
[navigatordoc]: https://docs.flutter.io/flutter/widgets/Navigator-class.html
[stocks]: https://github.com/flutter/flutter/blob/master/examples/stocks/lib/main.dart#L122
[materialappdoc]: https://docs.flutter.io/flutter/material/MaterialApp-class.html