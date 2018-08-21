---
layout: page
title: "使用长列表"
permalink: /cookbook/lists/long-lists/
summary: Flutter教程-本文介绍了如何在Flutter使用使用长列表
keywords: Flutter长列表
---

标准的[`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html)构造函数适用于小列表。
为了处理包含大量数据的列表，最好使用[`ListView.builder`](https://docs.flutter.io/flutter/widgets/ListView/ListView.builder.html)构造函数。

`ListView`的构造函数需要一次创建所有项目，但`ListView.builder`的构造函数不需要，它将在列表项滚动到屏幕上时创建该列表项。

## 1. 创建一个数据源

首先，我们需要一个数据源来。例如，您的数据源可能是消息列表、搜索结果或商店中的产品。大多数情况下，这些数据将来自互联网或数据库。

在这个例子中，我们将使用[`List.generate`](https://docs.flutter.io/flutter/dart-core/List/List.generate.html)构造函数生成拥有10000个字符串的列表

```dart
final items = new List<String>.generate(10000, (i) => "Item $i");
```

## 2. 将数据源转换成Widgets

为了显示我们的字符串列表，我们需要将每个字符串展现为一个Widget ！

这正是`ListView.builder`发挥作用的地方。在我们的例子中，我们将每一行显示一个字符串：

```dart
new ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return new ListTile(
      title: new Text('${items[index]}'),
    );
  },
);
```

## 完整的例子

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp(
    items: new List<String>.generate(10000, (i) => "Item $i"),
  ));
}

class MyApp extends StatelessWidget {
  final List<String> items;

  MyApp({Key key, @required this.items}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final title = 'Long List';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index) {
            return new ListTile(
              title: new Text('${items[index]}'),
            );
          },
        ),
      ),
    );
  }
}
```
