---
layout: page
title: "导航到新页面并返回"
permalink: /cookbook/navigation/navigation-basics/
summary: Flutter教程-本文介绍了在Flutter中如何导航到新页面并返回数据给上个页面
---

大多数应用程序包含多个页面。例如，我们可能有一个显示产品的页面，然后，用户可以点击产品，跳到该产品的详情页。

在Android中，页面对应的是Activity，在iOS中是ViewController。而在Flutter中，页面只是一个widget！

在Flutter中，我们那么我们可以使用[`Navigator`](https://docs.flutter.io/flutter/widgets/Navigator-class.html)在页面之间跳转。

## 步骤

  1. 创建两个页面。
  2. 调用`Navigator.push`导航到第二个页面。
  3. 调用`Navigator.pop`返回第一个页面。

## 1. 创建两个页面

我们创建两个页面，每个页面包含一个按钮。点击第一个页面上的按钮将导航到第二个页面。点击第二个页面上的按钮将返回到第一个页面。页面结构如下：

```dart
class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('First Screen'),
      ),
      body: new Center(
        child: new RaisedButton(
          child: new Text('Launch new screen'),
          onPressed: () {
            // Navigate to second screen when tapped!
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Second Screen"),
      ),
      body: new Center(
        child: new RaisedButton(
          onPressed: () {
            // Navigate back to first screen when tapped!
          },
          child: new Text('Go back!'),
        ),
      ),
    );
  }
}
```

## 2. 调用`Navigator.push`导航到第二个页面

为了导航到新的页面，我们需要调用[`Navigator.push`](https://docs.flutter.io/flutter/widgets/Navigator/push.html)方法。
该`push`方法将添加`Route`到由导航器管理的路由栈中！

该`push`方法需要一个`Route`，但`Route`从哪里来？我们可以创建自己的，或直接使用[`MaterialPageRoute`](https://docs.flutter.io/flutter/material/MaterialPageRoute-class.html)。
`MaterialPageRoute`很方便，因为它使用平台特定的动画跳转到新的页面(Android和IOS屏幕切换动画会不同)。

在`FirstScreen` Widget的`build`方法中，我们添加`onPressed`回调：

```dart
// Within the `FirstScreen` Widget
onPressed: () {
  Navigator.push(
    context,
    new MaterialPageRoute(builder: (context) => new SecondScreen()),
  );
}
``` 

## 3. 调用`Navigator.pop`返回第一个页面。

现在我们在第二个屏幕上，我们如何关闭它并返回到第一个屏幕？使用[`Navigator.pop`](https://docs.flutter.io/flutter/widgets/Navigator/pop.html)方法！
该`pop`方法将`Route`从导航器管理的路由栈中移除当前路径。代码如下：

```dart
// Within the SecondScreen Widget
onPressed: () {
  Navigator.pop(context);
}
```    

## 完整的例子

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(new MaterialApp(
    title: 'Navigation Basics',
    home: new FirstScreen(),
  ));
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('First Screen'),
      ),
      body: new Center(
        child: new RaisedButton(
          child: new Text('Launch new screen'),
          onPressed: () {
            Navigator.push(
              context,
              new MaterialPageRoute(builder: (context) => new SecondScreen()),
            );
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Second Screen"),
      ),
      body: new Center(
        child: new RaisedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: new Text('Go back!'),
        ),
      ),
    );
  }
}
```
