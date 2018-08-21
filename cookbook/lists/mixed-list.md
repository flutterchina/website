---
layout: page
title: "使用不同类型的子项创建列表"
permalink: /cookbook/lists/mixed-list/
summary: Flutter教程-本文介绍了如何在Flutter列表中创建不同类型的子项，从而构建一个拥有多种布局的列表
keywords: Flutter混合列表
---

我们经常需要创建显示不同类型内容的列表。例如，我们可能正在制作一个列表，其中显示一个标题，后面跟着与该标题相关的几个子项，再后面是另一个标题，等等。

我们如何用Flutter创建这样的结构？

## 步骤

  1. 使用不同类型的数据创建数据源
  2. 将数据源转换为Widgets列表

## 1. 使用不同类型的数据创建数据源

### 条目(子项)类型

为了表示列表中的不同类型的条目，我们需要为每个类型的条目定义一个类。

在这个例子中，我们将在一个应用程序上显示一个标题，后面跟着五条消息。因此，我们将创建三个类：`ListItem`、`HeadingItem`、和`MessageItem`。

```dart
// The base class for the different types of items the List can contain
abstract class ListItem {}

// A ListItem that contains data to display a heading
class HeadingItem implements ListItem {
  final String heading;

  HeadingItem(this.heading);
}

// A ListItem that contains data to display a message
class MessageItem implements ListItem {
  final String sender;
  final String body;

  MessageItem(this.sender, this.body);
}
```

### 创建Item列表

大多数时候，我们会从互联网或本地数据库中读取数据，并将该数据转换成item的列表。

对于这个例子，我们将生成一个Item列表来处理。该列表将包含一个标题、后跟五条消息，然后重复。

```dart
final items = new List<ListItem>.generate(
  1200,
  (i) => i % 6 == 0
      ? new HeadingItem("Heading $i")
      : new MessageItem("Sender $i", "Message body $i"),
);
```

## 2. 将数据源转换为Widgets列表

为了将每个item转换为Widget，我们将使用[`ListView.builder`](https://docs.flutter.io/flutter/widgets/ListView/ListView.builder.html)构造函数。

通常，我们需要提供一个`builder`函数来检查我们正在处理的item类型，并返回该item类型对应的Widget。

在这个例子中，使用`is`关键字来检查我们正在处理的item的类型，这个速度很快，并会自动将每个item转换为适当的类型。
但是，如果您更喜欢另一种模式，也有不同的方法可以解决这个问题！

```dart
new ListView.builder(
  // Let the ListView know how many items it needs to build
  itemCount: items.length,
  // Provide a builder function. This is where the magic happens! We'll
  // convert each item into a Widget based on the type of item it is.
  itemBuilder: (context, index) {
    final item = items[index];

    if (item is HeadingItem) {
      return new ListTile(
        title: new Text(
          item.heading,
          style: Theme.of(context).textTheme.headline,
        ),
      );
    } else if (item is MessageItem) {
      return new ListTile(
        title: new Text(item.sender),
        subtitle: new Text(item.body),
      );
    }
  },
);
```

## 完整的例子

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp(
    items: new List<ListItem>.generate(
      1000,
      (i) => i % 6 == 0
          ? new HeadingItem("Heading $i")
          : new MessageItem("Sender $i", "Message body $i"),
    ),
  ));
}

class MyApp extends StatelessWidget {
  final List<ListItem> items;

  MyApp({Key key, @required this.items}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final title = 'Mixed List';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new ListView.builder(
          // Let the ListView know how many items it needs to build
          itemCount: items.length,
          // Provide a builder function. This is where the magic happens! We'll
          // convert each item into a Widget based on the type of item it is.
          itemBuilder: (context, index) {
            final item = items[index];

            if (item is HeadingItem) {
              return new ListTile(
                title: new Text(
                  item.heading,
                  style: Theme.of(context).textTheme.headline,
                ),
              );
            } else if (item is MessageItem) {
              return new ListTile(
                title: new Text(item.sender),
                subtitle: new Text(item.body),
              );
            }
          },
        ),
      ),
    );
  }
}

// The base class for the different types of items the List can contain
abstract class ListItem {}

// A ListItem that contains data to display a heading
class HeadingItem implements ListItem {
  final String heading;

  HeadingItem(this.heading);
}

// A ListItem that contains data to display a message
class MessageItem implements ListItem {
  final String sender;
  final String body;

  MessageItem(this.sender, this.body);
}
```
