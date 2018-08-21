---
layout: page
title: "从新页面返回数据给上一个页面"
permalink: /cookbook/navigation/returning-data/
---

在某些情况下，我们可能想要从新页面返回数据。例如，假设我们导航到了一个新页面，向用户呈现两个选项。当用户点击某个选项时，我们需要将用户选择通知给第一个页面，以便它能够处理这些信息！

我们如何实现？使用[`Navigator.pop`](https://docs.flutter.io/flutter/widgets/Navigator/pop.html) ！

## 步骤

  1. 定义主页。
  2. 添加一个打开选择页面的按钮。
  3. 在选择页面上显示两个按钮。
  4. 点击一个按钮时，关闭选择的页面。
  5. 主页上弹出一个snackbar以显示用户的选择。

## 1. 定义主页

主页将显示一个按钮。点击后，它将打开选择页面！

```dart
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Returning Data Demo'),
      ),
      // We'll create the SelectionButton Widget in the next step 
      body: new Center(child: new SelectionButton()),
    );
  }
}
```

## 2. 添加一个打开选择页面的按钮。

现在，我们将创建我们的SelectionButton。我们的选择按钮将会：

  1. 点击时启动SelectionScreen
  2. 等待SelectionScreen返回结果

```dart
class SelectionButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new RaisedButton(
      onPressed: () {
        _navigateAndDisplaySelection(context);
      },
      child: new Text('Pick an option, any option!'),
    );
  }

  // A method that launches the SelectionScreen and awaits the result from
  // Navigator.pop
  _navigateAndDisplaySelection(BuildContext context) async {
    // Navigator.push returns a Future that will complete after we call
    // Navigator.pop on the Selection Screen!
    final result = await Navigator.push(
      context,
      // We'll create the SelectionScreen in the next step!
      new MaterialPageRoute(builder: (context) => new SelectionScreen()),
    );
  }
}
```

## 3. 在选择页面上显示两个按钮。


现在，我们需要构建一个选择页面！它将包含两个按钮。当用户点击按钮时，应该关闭选择页面并让主页知道哪个按钮被点击！

现在，我们将定义UI，并确定如何在下一步中返回数据。

```dart
class SelectionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Pick an option'),
      ),
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                  // Pop here with "Yep"...
                },
                child: new Text('Yep!'),
              ),
            ),
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                  // Pop here with "Nope"
                },
                child: new Text('Nope.'),
              ),
            )
          ],
        ),
      ),
    );
  }
}
``` 

## 4. 点击一个按钮时，关闭选择的页面。

现在，我们完成两个按钮的`onPressed`回调。为了将数据返回到第一个页面，我们需要使用[`Navitator.pop`](https://docs.flutter.io/flutter/widgets/Navigator/pop.html)方法。

`Navigator.pop`接受一个可选的(第二个)参数`result`。如果我们返回结果，它将返回到一个`Future`到主页的SelectionButton中！

### Yep 按钮

```dart
new RaisedButton(
  onPressed: () {
    // Our Yep button will return "Yep!" as the result
    Navigator.pop(context, 'Yep!');
  },
  child: new Text('Yep!'),
);
```

### Nope 按钮

```dart
new RaisedButton(
  onPressed: () {
    // Our Nope button will return "Nope!" as the result
    Navigator.pop(context, 'Nope!');
  },
  child: new Text('Nope!'),
);
```

## 5. 主页上弹出一个snackbar以显示用户的选择。

既然我们正在启动一个选择页面并等待结果，那么我们会想要对返回的信息进行一些操作！

在这种情况下，我们将显示一个显示结果的Snackbar。为此，我们将更新`SelectionButton`中的`_navigateAndDisplaySelection`方法。

```dart
_navigateAndDisplaySelection(BuildContext context) async {
  final result = await Navigator.push(
    context,
    new MaterialPageRoute(builder: (context) => new SelectionScreen()),
  );

  // After the Selection Screen returns a result, show it in a Snackbar!
  Scaffold
      .of(context)
      .showSnackBar(new SnackBar(content: new Text("$result")));
}
```
 

## 完整的例子

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(new MaterialApp(
    title: 'Returning Data',
    home: new HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Returning Data Demo'),
      ),
      body: new Center(child: new SelectionButton()),
    );
  }
}

class SelectionButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new RaisedButton(
      onPressed: () {
        _navigateAndDisplaySelection(context);
      },
      child: new Text('Pick an option, any option!'),
    );
  }

  // A method that launches the SelectionScreen and awaits the result from
  // Navigator.pop!
  _navigateAndDisplaySelection(BuildContext context) async {
    // Navigator.push returns a Future that will complete after we call
    // Navigator.pop on the Selection Screen!
    final result = await Navigator.push(
      context,
      new MaterialPageRoute(builder: (context) => new SelectionScreen()),
    );

    // After the Selection Screen returns a result, show it in a Snackbar!
    Scaffold
        .of(context)
        .showSnackBar(new SnackBar(content: new Text("$result")));
  }
}

class SelectionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Pick an option'),
      ),
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                  // Close the screen and return "Yep!" as the result
                  Navigator.pop(context, 'Yep!');
                },
                child: new Text('Yep!'),
              ),
            ),
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                  // Close the screen and return "Nope!" as the result
                  Navigator.pop(context, 'Nope.');
                },
                child: new Text('Nope.'),
              ),
            )
          ],
        ),
      ),
    );
  }
}
```
