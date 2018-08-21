---
layout: page
title: 格式化Flutter代码

permalink: /formatting/
---

* TOC Placeholder
{:toc}

## 自动格式化代码

尽管您可以按照任何喜欢的样式 - 但根据我们的经验 ，一个开发团队会：

1. 有一个单一的、共享的样式

1. 通过自动格式化来强制执行此样式.


### 在Android Studio和IntelliJ中自动格式化代码

安装`Dart`插件（请参阅[编辑器设置](/get-started/editor/)），以便在Android Studio和IntelliJ中自动格式化代码。

要在当前源代码窗口中自动格式化代码，请右键单击代码窗口并选择`Reformat code with dartfmt`。您也可以通过快捷键来格式化代码。

### 自动格式化VS Code中的代码

安装`Dart-Code`插件（请参阅[编辑器设置](/get-started/editor/)）以在VS Code中自动格式化代码。

要在当前源代码窗口中自动格式化代码，请右键单击代码窗口并选择`Format Document`。您也可以通过VS Code的快捷键来格式化代码。

要在保存文件时自动格式化代码，请将`editor.formatOnSave`设置设置为true。

### 使用`flutter`命令自动格式化代码

您还可以使用以下`flutter format`命令在命令行界面（CLI）中自动格式化代码：

```shell
Usage: flutter format <one or more paths>
-h, --help    Print this usage information.
```

### 使用 '尾随逗号'

Flutter代码通常涉及构建相当深的树状数据结构，例如在一个build方法中。
为了获得良好的自动格式化，我们建议您采用可选的尾部逗号。添加尾随逗号很简单：始终在函数、方法和构造函数的参数列表末尾添加尾随逗号，以便保留您的编码格式。
这将有助于自动格式化程序为Flutter样式代码插入适当的换行符。

这里是一个自动格式化公式格式化带有尾部逗号代码的示例：

![Automatically formatted code with trailing commas](/images/intellij/trailing-comma-with.png)

如果没有尾部逗号，格式化后则会是下面这样：

![Automatically formatted code without trailing commas](/images/intellij/trailing-comma-without.png)
