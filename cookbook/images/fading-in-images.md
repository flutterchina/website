---
layout: page
title: "用占位符淡入图片"
permalink: /cookbook/images/fading-in-images/
---

当使用默认`Image` widget显示图片时，您可能会注意到它们在加载完成后会直接显示到屏幕上。这可能会让用户产生视觉突兀。

相反，如果你最初显示一个占位符，然后在图像加载完显示时淡入，那么它会不会更好？我们可以使用[`FadeInImage`](https://docs.flutter.io/flutter/widgets/FadeInImage-class.html)来达到这个目的！

`FadeInImage`适用于任何类型的图片：内存、本地Asset或来自网上的图片。

在这个例子中，我们将使用[transparent_image](https://pub.dartlang.org/packages/transparent_image)包作为一个简单的透明占位图。
您也可以考虑按照[Assets和图片](/assets-and-images/)指南使用本地资源来做占位图。

```dart
new FadeInImage.memoryNetwork(
  placeholder: kTransparentImage,
  image: 'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
);
```

## 完整的例子

```dart
import 'package:flutter/material.dart';
import 'package:transparent_image/transparent_image.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Fade in images';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new Stack(
          children: <Widget>[
            new Center(child: new CircularProgressIndicator()),
            new Center(
              child: new FadeInImage.memoryNetwork(
                placeholder: kTransparentImage,
                image:
                    'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```
