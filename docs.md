---
layout: page
title: Flutter文档
permalink: /docs/
summary: Flutter文档，从搭建开发环境到写出您的第一个Flutter APP，从零到一逐步介绍了Flutter技术的方方面面，是学习Flutter最佳入门教程。
keywords: Flutter文档, Flutter教程
---

<ul class="cards">
{% for card in site.data.docs_cards %}
	<li class="cards__item">
	    <div class="card">
		    <h3 class="catalog-category-title"><a class="action-link" href="{{card.url}}">{{card.name}}</a></h3>
		    <p>{{card.description}}</p>
		    <div class="card-action">
		        <a class="action-link" href="{{card.url}}">查看</a>
		    </div>
		</div>
	</li>
{% endfor %}
</ul>

&nbsp;


## Flutter新手?

一旦您完成了[起步](/get-started/install/)中的内容-[编写您的第一个Flutter应用程序](/get-started/codelab/)，那么以下将是您接下来该做的。

* [Flutter for Android 开发者](/flutter-for-android/)<br>
  如果您有Android开发经验，请查看这篇文章。

* [HTML/CSS 模式](/web-analogs/)<br>
  如果您有Web开发经验，请查看这篇文章，这是h5和Flutter的一个类比。

* [在Flutter中构建布局](/tutorials/layout/)<br>
  了解如何在Flutter中创建布局，其中所有的一切皆是widget！

* [为Flutter App添加交互](/tutorials/interactive/)<br>
  了解如何为您的应用程序添加一个有状态的widget。

* [Flutter Widget框架概述](/widgets-intro/)<br>
  了解更多关于Flutter的响应式框架

&nbsp;


## 想再提高?

一旦掌握了基本知识，请尝试这些内容。

* [Cookbook](/cookbook/)<br>
  一些（会持续添加）处理常见Flutter问题的例子。

* [示例程序目录](/catalog/samples/)<br>
  一些简单的应用程序示例，演示了如何实现Flutter的功能和widget。

* [在Flutter中添加 Assets和图片](/assets-and-images/)<br>
  如何向Flutter应用中添加资源和图像。

* [Flutter中的动画](/animations/)<br>
  如何创建标准、hero或交错动画，如何命名Flutter支持的一些动画样式（todo:校准）。

* [路由和导航](/routing-and-navigation/)<br>
  如何创建路由并导航到新页面（在Flutter中称为route）

* [国际化](/tutorials/internationalization/)<br>
  走向全球！如何国际化您的Flutter应用程序(多语言支持)。

* [Effective Dart](https://www.dartlang.org/guides/language/effective-dart)<br>
  一个指南，介绍如何编写更好的Dart代码。

&nbsp;


## 专题

深入探讨您感兴趣的主题。

* [Flutter Widget Inspector](/inspector/)<br>
  如何使用widget检查器，这是一个功能强大的工具，可让您浏览widget树、禁用模拟器“slow mode”横幅、显示性能覆盖图等等。

* [自定义字体](/custom-fonts/)<br>
  如何将新字体添加到您的应用程序。

* [文本输入](/text-input/)<br>
  如何实现文本输入。

* [调试 Flutter Apps](/debugging/)<br>
  调试工具和技巧。

&nbsp;

这不是一个完整的列表。请使用左侧导航栏或搜索栏查找其他主题。
