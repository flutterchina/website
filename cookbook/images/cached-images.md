---
layout: page
title: "使用缓存图片"
permalink: /cookbook/images/cached-images/
---

在某些情况下，在从网上下载图片后缓存图片可能会很方便，以便它们可以脱机使用。为此，我们可以使用[`cached_network_image`](https://pub.dartlang.org/packages/cached_network_image)包来达到目的。

除了缓存之外，cached_image_network包在加载时还支持占位符和淡入淡出图片！

```dart
new CachedNetworkImage(
  imageUrl: 'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
);
```

## 添加一个占位符

`cached_network_image`包允许我们使用任何Widget作为占位符！在这个例子中，我们将在图片加载时显示一个进度圈。

```dart
new CachedNetworkImage(
  placeholder: (context, url) => CircularProgressIndicator(),
  imageUrl: 'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
);
``` 

## 完整的例子

```dart
import 'package:flutter/material.dart';
import 'package:cached_network_image/cached_network_image.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Cached Images';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new Center(
          child: new CachedNetworkImage(
            placeholder: (context, url) => CircularProgressIndicator(),
            imageUrl:
                'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
          ),
        ),
      ),
    );
  }
}
```
