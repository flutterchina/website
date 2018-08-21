---
layout: page
title: "显示来自网上的图片"
permalink: /cookbook/images/network-image/
---

显示图片是大多数移动应用程序的基础。Flutter提供了[`Image`](https://docs.flutter.io/flutter/widgets/Image-class.html) Widget来显示不同类型的图片。

为了处理来自URL的图像，请使用[`Image.network`](https://docs.flutter.io/flutter/widgets/Image/Image.network.html)构造函数。

```dart
new Image.network(
  'https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg',
)
```

## Bonus: GIF动画

`Image` Widget的一个惊艳的功能是：支持GIF动画！

```dart
new Image.network(
  'https://github.com/flutter/plugins/raw/master/packages/video_player/doc/demo_ipod.gif?raw=true',
);
```

## 占位图和缓存

`Image.network`默认不能处理一些高级功能，例如在在下载完图片后加载或缓存图片到设备中后，使图片渐隐渐显。要实现这种功能，请参阅以下内容：

  * [用占位符淡入图片](/cookbook/images/fading-in-images/)
  * [使用缓存图片](/cookbook/images/cached-images/)

## 完整的例子

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var title = 'Web Images';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new Image.network(
          'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
        ),
      ),
    );
  }
}
```
