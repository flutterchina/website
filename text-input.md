---
layout: page
title: 处理文本输入
permalink: /text-input/
summary: Flutter中处理用户输入，同时介绍了一些常用的Flutter表单组件
summary: Flutter文本输入
---

* TOC Placeholder
{:toc}

## 介绍

本页面介绍如何在Flutter应用程序中输入文本

## 选择一个widget

[`TextField`](https://docs.flutter.io/flutter/material/TextField-class.html)
是最常用的文本输入widget.

默认情况下，`TextField`有一个下划线装饰（decoration）。您可以通过提供给[`decoration`](https://docs.flutter.io/flutter/material/TextField/decoration.html)属性设置一个[`InputDecoration`](https://docs.flutter.io/flutter/material/InputDecoration-class.html)来添加一个标签、一个图标、提示文字和错误文本。
要完全删除装饰（包括下划线和为标签保留的空间），将decoration明确设置为空即可。

[`TextFormField`](https://docs.flutter.io/flutter/material/TextFormField-class.html)包裹一个`TextField`
并将其集成在[`Form`](https://docs.flutter.io/flutter/widgets/Form-class.html)中。你要提供一个验证函数来检查用户的输入是否满足一定的约束（例如，一个电话号码）或当你想将`TextField`与其他`FormField`集成时，使用`TextFormField`。

## 获取用户输入

有两种获取用户输入的主要方法：:

- 处理 [`onChanged`](https://docs.flutter.io/flutter/material/TextField/onChanged.html)回调
- 提供一个[`TextEditingController`](https://docs.flutter.io/flutter/widgets/TextEditingController-class.html).

### onChanged

每当用户输入时，TextField会调用它的[`onChanged`](https://docs.flutter.io/flutter/material/TextField/onChanged.html)回调。
您可以处理此回调以查看用户输入的内容。例如，如果您正在输入搜索字段，则可能需要在用户输入时更新搜索结果。

### TextEditingController

一个更强大（但更精细）的方法是提供一个[`TextEditingController`](https://docs.flutter.io/flutter/widgets/TextEditingController-class.html)作为`TextField`的[`controller`](https://docs.flutter.io/flutter/material/TextField/controller.html)属性。
在用户输入时，controller的`text`和`selection`属性不断的更新。要在这些属性更改时得到通知，请使用controller的[`addListener`](https://docs.flutter.io/flutter/foundation/ChangeNotifier/addListener.html)方法监听控制器 。
（如果你添加了一个监听器，记得在你的State对象的dispose方法中删除监听器 ）。

该`TextEditingController`还可以让您控制`TextField`的内容。如果修改controller的`text`或`selection`的属性，`TextField`将更新，以显示修改后的文本或选中区间。
例如，您可以使用此功能来实现推荐内容的自动补全。

## 例子
以下是使用一个[`TextEditingController`](https://docs.flutter.io/flutter/widgets/TextEditingController-class.html)读取TextField中用户输入的值的示例：

```dart
/// Opens an [AlertDialog] showing what the user typed.
class ExampleWidget extends StatefulWidget {
  ExampleWidget({Key key}) : super(key: key);

  @override
  _ExampleWidgetState createState() => new _ExampleWidgetState();
}

/// State for [ExampleWidget] widgets.
class _ExampleWidgetState extends State<ExampleWidget> {
  final TextEditingController _controller = new TextEditingController();

  @override
  Widget build(BuildContext context) {
    return new Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        new TextField(
          controller: _controller,
          decoration: new InputDecoration(
            hintText: 'Type something',
          ),
        ),
        new RaisedButton(
          onPressed: () {
            showDialog(
              context: context,
              child: new AlertDialog(
                title: new Text('What you typed'),
                content: new Text(_controller.text),
              ),
            );
          },
          child: new Text('DONE'),
        ),
      ],
    );
  }
}
```
