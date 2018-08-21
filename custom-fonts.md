---
layout: page
title: 使用字体
permalink: /custom-fonts/
summary: 本文介绍了如何在Flutter中使用自定义字体。
keywords: Flutter自定义字体
---

## 介绍

你可以在你的Flutter应用程序中使用不同的字体。例如，您可能会使用您的设计人员创建的自定义字体，或者您可能会使用[Google Fonts(需翻墙)）](https://fonts.google.com/)中的字体。

本页介绍如何为Flutter应用配置字体，并在渲染文本时使用它们。


## 使用字体

在Flutter应用程序中使用字体分两步完成。首先在`pubspec.yaml`中声明它们，以确保它们包含在应用程序中。然后通过[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)属性使用字体。
### 在asset中声明

要在应用中包含字体，请先在`pubspec.yaml`中声明它。然后将字体文件复制到您在`pubspec.yaml`中指定的位置。

```yaml
flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: assets/fonts/Raleway-Regular.ttf
        - asset: assets/fonts/Raleway-Medium.ttf
          weight: 500
        - asset: assets/fonts/Raleway-SemiBold.ttf
          weight: 600
    - family: AbrilFatface
      fonts:
        - asset: assets/fonts/abrilfatface/AbrilFatface-Regular.ttf
```

### 使用字体


```dart
// declare the text style
const textStyle = const TextStyle(
  fontFamily: 'Raleway',
);

// use the text style
var buttonText = const Text(
  "Use the font for this text",
  style: textStyle,
);
```
### 依赖包中的字体{#from-packages}

要使用依赖包中定义的字体，必须提供`package`参数。例如，假设上面的字体声明位于pubspec.yaml中声明的`my_package`包中。然后创建TextStyle的过程如下：

```dart
const textStyle = const TextStyle(
  fontFamily: 'Raleway',
  package: 'my_package',
);
```

如果包内部使用它定义的字体，则也应该在创建文本样式时指定`package`参数，如上例所示。

一个包也可以提供字体文件而不需要在pubspec.yaml中声明。
这些文件应该包的`lib/`文件夹中。字体文件不会自动绑定到应用程序中，应用程序可以在声明字体时有选择地使用这些字体。假设一个名为my_package的包中有一个：

```
lib/fonts/Raleway-Medium.ttf
```
然后，应用程序可以声明一个字体，如下面的示例所示：

```yaml
 flutter:
   fonts:
     - family: Raleway
       fonts:
         - asset: assets/fonts/Raleway-Regular.ttf
         - asset: packages/my_package/fonts/Raleway-Medium.ttf
           weight: 500
```

这`lib/`是隐含的，所以它不应该包含在asset路径中。

在这种情况下，由于应用程序本地定义了字体，所以创建的TextStyle没有`package`参数：

```dart
const textStyle = const TextStyle(
  fontFamily: 'Raleway',
);
```

## 使用Material Design字体图标

如果要使用Material Design字体图标，可以通过向`pubspec.yaml`文件添加属性`uses-material-design: true`来简单包含它。

```yaml
flutter:
  # The following line ensures that the Material Icons font is
  # included with your application, so that you can use the icons in
  # the Icons class.
  uses-material-design: true
```

## pubspec.yaml 选项定义

`family` 是字体的名称, 你可以在[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)的
[`fontFamily`](https://docs.flutter.io/flutter/painting/TextStyle/fontFamily.html)
属性中使用.

`asset` 是相对于 `pubspec.yaml` 文件的路径.这些文件包含字体中字形的轮廓。在构建应用程序时，这些文件会包含在应用程序的asset包中。

可以给字体设置粗细、倾斜等样式

  * `weight`属性指定字体的粗细，取值范围是100到900之间的整百数(100的倍数). 这些值对应
    [`FontWeight`](https://docs.flutter.io/flutter/dart-ui/FontWeight-class.html)，
    可以用于 [`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)的[`fontWeight`](https://docs.flutter.io/flutter/painting/TextStyle/fontWeight.html)属性


  * `style` 指定字体是倾斜还是正常，对应的值为`italic`和 `normal`. 这些值对应
    [`FontStyle`](https://docs.flutter.io/flutter/dart-ui/FontStyle-class.html)
    可以用于[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)的 [fontStyle](https://docs.flutter.io/flutter/painting/TextStyle/fontStyle.html)
    [`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html) 属性


## TextStyle

如果一个 [`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)
对象指定了一个没有确切字体文件的weight或style，那么引擎会为该字体使用一个通用的文件，并尝试为指定的weight和style推断一个轮廓。


## 例子

以下是在应用程序中声明和使用字体的示例。

| iOS | Android |
|------|------|
| <img style="width: 200px;" src="/images/fonts-example-ios.png" alt="Display of the fonts on ios." /> | <img style="width: 200px;" src="/images/fonts-example-android.png" alt="Display of the fonts on android." /> |


在pubsec.yaml中声明字体。

```yaml
name: my_application
description: A new Flutter project.

dependencies:
  flutter:
    sdk: flutter
    
flutter:
  # Include the Material Design fonts.
  uses-material-design: true

  fonts:
    - family: Rock Salt
      fonts:
        # https://fonts.google.com/specimen/Rock+Salt
        - asset: fonts/RockSalt-Regular.ttf
    - family: VT323
      fonts:
        # https://fonts.google.com/specimen/VT323
        - asset: fonts/VT323-Regular.ttf
    - family: Ewert
      fonts:
        # https://fonts.google.com/specimen/Ewert
        - asset: fonts/Ewert-Regular.ttf
```

源码是：

```dart
import 'package:flutter/material.dart';

const String words1 = "Almost before we knew it, we had left the ground.";
const String words2 = "A shining crescent far beneath the flying vessel.";
const String words3 = "A red flair silhouetted the jagged edge of a wing.";
const String words4 = "Mist enveloped the ship three hours out from port.";

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Flutter Fonts',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new FontsPage(),
    );
  }
}

class FontsPage extends StatefulWidget {
  @override
  _FontsPageState createState() => new _FontsPageState();
}

class _FontsPageState extends State<FontsPage> {
  @override
  Widget build(BuildContext context) {
    // Rock Salt - https://fonts.google.com/specimen/Rock+Salt
    var rockSaltContainer = new Container(
      child: new Column(
        children: <Widget>[
          new Text(
            "Rock Salt",
          ),
          new Text(
            words2,
            textAlign: TextAlign.center,
            style: new TextStyle(
              fontFamily: "Rock Salt",
              fontSize: 17.0,
            ),
          ),
        ],
      ),
      margin: const EdgeInsets.all(10.0),
      padding: const EdgeInsets.all(10.0),
      decoration: new BoxDecoration(
        color: Colors.grey.shade200,
        borderRadius: new BorderRadius.all(new Radius.circular(5.0)),
      ),
    );

    // VT323 - https://fonts.google.com/specimen/VT323
    var v2t323Container = new Container(
      child: new Column(
        children: <Widget>[
          new Text(
            "VT323",
          ),
          new Text(
            words3,
            textAlign: TextAlign.center,
            style: new TextStyle(
              fontFamily: "VT323",
              fontSize: 25.0,
            ),
          ),
        ],
      ),
      margin: const EdgeInsets.all(10.0),
      padding: const EdgeInsets.all(10.0),
      decoration: new BoxDecoration(
        color: Colors.grey.shade200,
        borderRadius: new BorderRadius.all(new Radius.circular(5.0)),
      ),
    );

    // https://fonts.google.com/specimen/Ewert
    var ewertContainer = new Container(
      child: new Column(
        children: <Widget>[
          new Text(
            "Ewert",
          ),
          new Text(
            words4,
            textAlign: TextAlign.center,
            style: new TextStyle(
              fontFamily: "Ewert",
              fontSize: 25.0,
            ),
          ),
        ],
      ),
      margin: const EdgeInsets.all(10.0),
      padding: const EdgeInsets.all(10.0),
      decoration: new BoxDecoration(
        color: Colors.grey.shade200,
        borderRadius: new BorderRadius.all(new Radius.circular(5.0)),
      ),
    );

    // Material Icons font - included with Material Design
    String icons = "";

    // https://material.io/icons/#ic_accessible
    // accessible: &#xE914; or 0xE914 or E914
    icons += "\u{E914}";

    // https://material.io/icons/#ic_error
    // error: &#xE000; or 0xE000 or E000
    icons += "\u{E000}";

    // https://material.io/icons/#ic_fingerprint
    // fingerprint: &#xE90D; or 0xE90D or E90D
    icons += "\u{E90D}";

    // https://material.io/icons/#ic_camera
    // camera: &#xE3AF; or 0xE3AF or E3AF
    icons += "\u{E3AF}";

    // https://material.io/icons/#ic_palette
    // palette: &#xE40A; or 0xE40A or E40A
    icons += "\u{E40A}";

    // https://material.io/icons/#ic_tag_faces
    // tag faces: &#xE420; or 0xE420 or E420
    icons += "\u{E420}";

    // https://material.io/icons/#ic_directions_bike
    // directions bike: &#xE52F; or 0xE52F or E52F
    icons += "\u{E52F}";

    // https://material.io/icons/#ic_airline_seat_recline_extra
    // airline seat recline extra: &#xE636; or 0xE636 or E636
    icons += "\u{E636}";

    // https://material.io/icons/#ic_beach_access
    // beach access: &#xEB3E; or 0xEB3E or EB3E
    icons += "\u{EB3E}";

    // https://material.io/icons/#ic_public
    // public: &#xE80B; or 0xE80B or E80B
    icons += "\u{E80B}";

    // https://material.io/icons/#ic_star
    // star: &#xE838; or 0xE838 or E838
    icons += "\u{E838}";

    var materialIconsContainer = new Container(
      child: new Column(
        children: <Widget>[
          new Text(
            "Material Icons",
          ),
          new Text(
            icons,
            textAlign: TextAlign.center,
            style: new TextStyle(
              inherit: false,
              fontFamily: "MaterialIcons",
              color: Colors.black,
              fontStyle: FontStyle.normal,
              fontSize: 25.0,
            ),
          ),
        ],
      ),
      margin: const EdgeInsets.all(10.0),
      padding: const EdgeInsets.all(10.0),
      decoration: new BoxDecoration(
        color: Colors.grey.shade200,
        borderRadius: new BorderRadius.all(new Radius.circular(5.0)),
      ),
    );

    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Fonts"),
      ),
      body: new ListView(
        children: <Widget>[
          rockSaltContainer,
          v2t323Container,
          ewertContainer,
          materialIconsContainer,
        ],
      ),
    );
  }
}
```
