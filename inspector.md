---
layout: page
title: Flutter Widget Inspector
permalink: /inspector/
summary: 本文介绍了Flutter强大的调试工具Widget Inspector，有了它我们可以可视化的浏览Flutter的Widget树...
keywords: Flutter调试
---

Flutter Widget Inspector是用于可视化和浏览Flutter Widget树的强大工具。

* TOC Placeholder
{:toc}

## Flutter Widget Inspector

Flutter框架使用widget作为[核心构建块](/widgets-intro/)，从控件（文本、按钮、toggle等）到布局（居中、填充、行、列等）的任何内容都是。
Inspector是用于可视化和浏览Flutter这些widget树的强大工具。在以下情况下可能会有帮助：

* 不清楚现有布局
* 诊断布局问题

![IntelliJ Flutter Inspector Window](/images/intellij/visual-debugging.png)

点击Flutter Inspector工具栏上的“Select widget”，然后点击设备(真机或虚拟机)以选择一个widget。所选widget将在设备和widget树中高亮显示。

![Select Demo](/images/intellij/inspector_select_example.gif)

然后，您可以浏览IDE中的交互式widget树，以查看附近的widget并查看其字段值。如果您想调试布局问题，那么Widgets树可能不够详细。
在这种情况下，单击“Render Tree”选项卡查看树中相同位置的渲染树。当调试布局问题时，关键是看`size`和`constraints`字段。约束沿着树向下传递，尺寸向上传递。

![Switch Trees](/images/intellij/switch_inspector_tree.gif)

有关Inspector的更完整演示，请参阅最近的[DartConf talk](https://www.youtube.com/watch?v=JIcmJNT9DNI).

## 开始使用Inspector

Inspector目前可用于Android Studio或IntelliJ IDEA的[Flutter插件](/get-started/editor/)。

## 反馈问题

如果您有任何建议或遇到问题，请在[file an issue in our tracker](https://github.com/flutter/flutter-intellij/issues/new?labels=inspector)此提交。
