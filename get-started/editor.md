---
layout: page
title: "起步: 配置编辑器"
permalink: /get-started/editor/
---


您可以使用任何文本编辑器与命令行工具来构建Flutter应用程序。
不过，我们建议使用我们的编辑器插件之一，以获得更好的体验。通过我们的编辑器插件，您可以获得代码补全、语法高亮、widget编辑辅助、运行和调试支持等等。


按照下面步骤为Android Studio、IntelliJ或VS Code添加编辑器插件。如果你想使用其他的编辑器，
那没关系，直接跳到 [下一步:创建并运行你的第一个应用程序](/get-started/test-drive/)。

<ul class="tabs__top-bar">
    <li class="tab-link current" data-tab="tab-install-androidsstudio">Android Studio</li>
    <li class="tab-link" data-tab="tab-install-vscode">VS Code</li>
</ul>

<div id="tab-install-androidsstudio" class="tabs__content current" markdown="1">

## Android Studio 安装

*Android Studio:* 为Flutter提供完整的IDE体验

### 安装Android Studio

   * [Android Studio](https://developer.android.com/studio/index.html), 3.0或更高版本.

或者，您也可以使用IntelliJ：

   * [IntelliJ IDEA Community](https://www.jetbrains.com/idea/download/), version 2017.1或更高版本.
   * [IntelliJ IDEA Ultimate](https://www.jetbrains.com/idea/download/), version 2017.1 或更高版本.

### 安装Flutter和Dart插件

需要安装两个插件:

   * `Flutter`插件： 支持Flutter开发工作流 (运行、调试、热重载等).
   * `Dart`插件： 提供代码分析 (输入代码时进行验证、代码补全等).

要安装这些:

   1. 启动Android Studio.
   1. 打开插件首选项 (**Preferences>Plugins** on macOS,
      **File>Settings>Plugins** on Windows & Linux).
   1. 选择 **Browse repositories…**,  选择 Flutter 插件并点击 `install`.
   1. 重启Android Studio后插件生效.

</div>

<div id="tab-install-vscode" class="tabs__content" markdown="1">

## Visual Studio Code (VS Code) 安装

*VS Code:* 轻量级编辑器，支持Flutter运行和调试.

### 安装 VS Code

  * [VS Code](https://code.visualstudio.com/), 安装1.20.1或更高版本.

### 安装Flutter插件

  1. 启动 VS Code
  1. 调用 **View>Command Palette...**
  1. 输入 'install', 然后选择 **Extensions: Install Extension** action
  1. 在搜索框输入 `flutter` , 在搜索结果列表中选择 'Flutter', 然后点击 **Install**
  1. 选择 'OK' 重新启动 VS Code

## 通过Flutter Doctor验证您的设置

  1. 调用 **View>Command Palette...**
  1. 输入 'doctor', 然后选择 **'Flutter: Run Flutter Doctor'** action
  1. 查看“OUTPUT”窗口中的输出是否有问题

</div>

## 下一步

让我们来体验一下Flutter：创建第一个项目，运行它，并体验“热重载”.

[下一步: 体验 Flutter](/get-started/test-drive/)
