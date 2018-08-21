---
layout: page
title: "处理Taps"
permalink: /cookbook/gestures/handling-taps/
summary: Flutter教程-本文介绍了在Flutter中如何处理点击事件(tap)。
keywords: Flutter事件处理
---

我们不仅希望向用户展示信息，还希望我们的用户与我们的应用互动！那么，我们如何响应用户基本操作，如点击和拖动？ 在Flutter中我们可以使用[`GestureDetector`](https://docs.flutter.io/flutter/widgets/GestureDetector-class.html) Widget！

假设我们想要创建一个自定义按钮，当点击时显示一个SnackBar。我们如何解决这个问题？

## 步骤

  1. 创建一个button。
  2. 把它包装在 `GestureDetector`中并提供一个`onTap`回调。

```dart
// Our GestureDetector wraps our button
new GestureDetector(
  // When the child is tapped, show a snackbar 
  onTap: () {
    final snackBar = new SnackBar(content: new Text("Tap"));

    Scaffold.of(context).showSnackBar(snackBar);
  },
  // Our Custom Button!
  child: new Container(
    padding: new EdgeInsets.all(12.0),
    decoration: new BoxDecoration(
      color: Theme.of(context).buttonColor,
      borderRadius: new BorderRadius.circular(8.0),
    ),
    child: new Text('My Button'),
  ),
);
```

## 注意

  1. 如果您想将Material 水波效果添加到按钮中，请参阅"[添加Material 触摸水波](/cookbook/gestures/ripples/)" 。
  2. 虽然我们已经创建了一个自定义按钮来演示这些概念，但Flutter也提供了一些其它开箱即用的按钮：[RaisedButton](https://docs.flutter.io/flutter/material/RaisedButton-class.html)、[FlatButton](https://docs.flutter.io/flutter/material/FlatButton-class.html)和[CupertinoButton](https://docs.flutter.io/flutter/cupertino/CupertinoButton-class.html)。
    

## 完整的例子

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Gesture Demo';

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
    // Our GestureDetector wraps our button
    return new GestureDetector(
      // When the child is tapped, show a snackbar
      onTap: () {
        final snackBar = new SnackBar(content: new Text("Tap"));

        Scaffold.of(context).showSnackBar(snackBar);
      },
      // Our Custom Button!
      child: new Container(
        padding: new EdgeInsets.all(12.0),
        decoration: new BoxDecoration(
          color: Theme.of(context).buttonColor,
          borderRadius: new BorderRadius.circular(8.0),
        ),
        child: new Text('My Button'),
      ),
    );
  }
}
```
