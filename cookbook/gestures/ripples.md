---
layout: page
title: "添加Material触摸水波效果"
permalink: /cookbook/gestures/ripples/
summary: Flutter教程-本文介绍了在设计应遵循Material Design指南的应用程序时，我们希望在点击时将水波动画添加到Widgets
keywords: Flutter水波动画
---

在设计应遵循Material Design指南的应用程序时，我们希望在点击时将水波动画添加到Widgets。

Flutter提供了[`InkWell`](https://docs.flutter.io/flutter/material/InkWell-class.html)Widget来实现这个效果。

## 步骤

  1. 创建一个可点击的Widget。
  2. 将它包裹在一个`InkWell`中来管理点击回调和水波动画。
  
```dart
// The InkWell Wraps our custom flat button Widget
new InkWell(
  // When the user taps the button, show a snackbar
  onTap: () {
    Scaffold.of(context).showSnackBar(new SnackBar(
      content: new Text('Tap'),
    ));
  },
  child: new Container(
    padding: new EdgeInsets.all(12.0),
    child: new Text('Flat Button'),
  ),
);
```   

## 完整的例子

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'InkWell Demo';

    return new MaterialApp(
      title: title,
      home: new MyHomePage(title: title),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(title),
      ),
      body: new Center(child: new MyButton()),
    );
  }
}

class MyButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // The InkWell Wraps our custom flat button Widget
    return new InkWell(
      // When the user taps the button, show a snackbar
      onTap: () {
        Scaffold.of(context).showSnackBar(new SnackBar(
          content: new Text('Tap'),
        ));
      },
      child: new Container(
        padding: new EdgeInsets.all(12.0),
        child: new Text('Flat Button'),
      ),
    );
  }
}
```
