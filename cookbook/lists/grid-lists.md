---
layout: page
title: "创建一个 Grid List"
permalink: /cookbook/lists/grid-lists/
summary: Flutter教程-本文介绍了在Flutter中如何使用GridView
keywords: Flutter GridView
---

在某些情况下，您可能希望将item显示为网格，而不是一个普通列表。对于这需求，我们可以使用[`GridView`](https://docs.flutter.io/flutter/widgets/GridView-class.html) Widget。

使用网格的最简单方法是使用[`GridView.count`](https://docs.flutter.io/flutter/widgets/GridView/GridView.count.html)构造函数，它允许我们指定行数或列数。

在这个例子中，我们将生成一个包含100个widget的list，在网格中显示它们的索引。这将帮助我们观察`GridView`如何工作。

```dart
new GridView.count(
  // Create a grid with 2 columns. If you change the scrollDirection to 
  // horizontal, this would produce 2 rows.
  crossAxisCount: 2,
  // Generate 100 Widgets that display their index in the List
  children: new List.generate(100, (index) {
    return new Center(
      child: new Text(
        'Item $index',
        style: Theme.of(context).textTheme.headline,
      ),
    );
  }),
);
```

## 完整的例子

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Grid List';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new GridView.count(
          // Create a grid with 2 columns. If you change the scrollDirection to
          // horizontal, this would produce 2 rows.
          crossAxisCount: 2,
          // Generate 100 Widgets that display their index in the List
          children: new List.generate(100, (index) {
            return new Center(
              child: new Text(
                'Item $index',
                style: Theme.of(context).textTheme.headline,
              ),
            );
          }),
        ),
      ),
    );
  }
}
```
