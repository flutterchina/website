---
layout: page
title: Flutter for iOS 开发者
permalink: /flutter-for-ios/
summary: 本文档适用于IOS开发人员，可以将您现有的iOS知识应用于使用Flutter构建移动应用程序。如果您了解IOS框架的基础知识，那么您可以将此文档用作Flutter开发的一个开端
keywords: Flutter Android
---

* TOC Placeholder
{:toc}

# Flutter for iOS 开发者

本文档适用那些希望将现有 iOS 经验应用于 Flutter 的开发者。如果你拥有 iOS 开发基础，那么你可以使用这篇文档开始学习 Flutter 的开发。

开发 Flutter 时，你的 iOS 经验和技能将会大有裨益，因为 Flutter 依赖于移动操作系统的众多功能和配置。Flutter 是用于为移动设备构建用户界面的全新方式，但它也有一个插件系统用于和 iOS（及 Android）进行非 UI 任务的通信。如果你是 iOS 开发专家，则你不必将 Flutter 彻底重新学习一遍。

你可以将此文档作为 cookbook，通过跳转并查找与你的需求最相关的问题。

## Views

### UIView 相当于 Flutter 中的什么？

在 iOS 中，构建 UI 的过程中将大量使用 view 对象。这些对象都是 `UIView` 的实例。它们可以用作容器来承载其他的 UIView，最终构成你的界面布局。

在 Flutter 中，你可以粗略地认为 `Widget` 相当于 `UIView` 。Widget 和 iOS 中的控件并不完全等价，但当你试图去理解 Flutter 是如何工作的时候，你可以认为它们是“声明和构建 UI 的方法”。

然而，Widget 和 UIView 还是有些区别的。首先，widgets 拥有不同的生存时间：它们一直存在且保持不变，直到当它们需要被改变。当 widgets 和它们的状态被改变时，Flutter 会构建一颗新的 widgets 树。作为对比，iOS 中的 views 在改变时并不会被重新创建。但是与其说 views 是可变的实例，不如说它们被绘制了一次，并且直到使用 `setNeedsDisplay()` 之后才会被重新绘制。

此外，不像 UIView，由于不可变性，Flutter 的 widgets 非常轻量。这是因为它们本身并不是什么控件，也不会被直接绘制出什么，而只是 UI 的描述。

Flutter 包含了 [Material 组件](https://material.io/develop/flutter/)库。这些 widgets 遵循了 [Material 设计规范](https://material.io/design/)。MD 是一个灵活的设计系统，并且为包括 iOS 在内的[所有系统进行了优化](https://material.io/design/platform-guidance/cross-platform-adaptation.html#cross-platform-guidelines)。

但是用 Flutter 实现任何的设计语言都非常的灵活和富有表现力。在 iOS 平台，你可以使用 [Cupertino widgets](https://flutter.io/widgets/cupertino/) 来构建遵循了 [Apple’s iOS design language](https://developer.apple.com/design/resources/) 的界面。

### 我怎么来更新 Widgets？

在 iOS 上更新 views，只需要直接改变它们就可以了。在 Flutter 中，widgets 是不可变的，而且不能被直接更新。你需要去操纵 widget 的 state。

这也正是有状态的和无状态的 widget 这一概念的来源。一个 `StatelessWidget` 正如它听起来一样，是一个没有附加状态的 widget。

`StatelessWidget` 在你构建初始化后不再进行改变的界面时非常有用。

举个例子，你可能会用一个 `UIImageView` 来展示你的 logo `image` 。如果这个 logo 在运行时不会改变，那么你就可以在 Flutter 中使用 `StatelessWidget` 。

如果你希望在发起 HTTP 请求时，依托接收到的数据动态的改变 UI，请使用 `StatefulWidget`。当 HTTP 请求结束后，通知 Flutter 框架 widget 的 `State` 更新了，好让系统来更新 UI。

有状态和无状态的 widget 之间一个非常重要的区别是，`StatefulWidget` 拥有一个 `State` 对象来存储它的状态数据，并在 widget 树重建时携带着它，因此状态不会丢失。

如果你有疑惑，请记住以下规则：如果一个 widget 在它的 `build` 方法之外改变（例如，在运行时由于用户的操作而改变），它就是有状态的。如果一个 widget 在一次 build 之后永远不变，那它就是无状态的。但是，即便一个 widget 是有状态的，包含它的父亲 widget 也可以是无状态的，只要父 widget 本身不响应这些变化。

下面的例子展示了如何使用一个 `StatelessWidget` 。一个常见的 `StatelessWidget` 是 `Text` widget。如果你查看 Text 的实现，你会发现它是 StatelessWidget 的子类。

```dart
Text(
  'I like Flutter!',
  style: TextStyle(fontWeight: FontWeight.bold),
);
```

阅读上面的代码，你可能会注意到 `Text` widget 并不显示地携带任何状态。它通过传入给它的构造器的数据来渲染，除此之外再无其他。

但是，如果你希望 `I like Flutter `在点击 `FloatingActionButton` 时动态的改变呢？

为了实现这个，用 `StatefulWidget` 包裹 `Text` widget，并在用户点击按钮时更新它。

举个例子：

```dart
class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default placeholder text
  String textToShow = "I Like Flutter";
  void _updateText() {
    setState(() {
      // update the text
      textToShow = "Flutter is Awesome!";
    });
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(child: Text(textToShow)),
      floatingActionButton: FloatingActionButton(
        onPressed: _updateText,
        tooltip: 'Update Text',
        child: Icon(Icons.update),
      ),
    );
  }
}
```

### 我怎么对 widget 布局？我的 Storyboard 在哪？

在 iOS 中，你可能会用 Storyboard 文件来组织 views，并对它们设置约束，或者，你可能在 view controller 中使用代码来设置约束。在 Flutter 中，你通过编写一个 widget 树来声明你的布局。

下面这个例子展示了如何展示一个带有 padding 的简单 widget：

```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text("Sample App"),
    ),
    body: Center(
      child: CupertinoButton(
        onPressed: () {
          setState(() { _pressedCount += 1; });
        },
        child: Text('Hello'),
        padding: EdgeInsets.only(left: 10.0, right: 10.0),
      ),
    ),
  );
}
```

你可以给任何的 widget 添加 padding，这很像 iOS 中约束的功能。

你可以在 [widget catalog](https://flutter.io/widgets/layout/) 中查看 Flutter 提供的布局。

### 我怎么在我的约束中添加或移除组件？

在 iOS 中，你在父 view 中调用 `addSubview()` 或在子 view 中调用 `removeFromSuperview()` 来动态地添加或移除子 views。在 Flutter 中，由于 widget 不可变，所以没有和 `addSubview()` 直接等价的东西。作为替代，你可以向 parent 传入一个返回 widget 的函数，并用一个布尔值来控制子 widget 的创建。

下面这个例子展示了在点击 `FloatingActionButton` 时如何动态地切换两个 widgets：

```dart
class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default value for toggle
  bool toggle = true;
  void _toggle() {
    setState(() {
      toggle = !toggle;
    });
  }

  _getToggleChild() {
    if (toggle) {
      return Text('Toggle One');
    } else {
      return CupertinoButton(
        onPressed: () {},
        child: Text('Toggle Two'),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(
        child: _getToggleChild(),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _toggle,
        tooltip: 'Update Text',
        child: Icon(Icons.update),
      ),
    );
  }
}
```

### 我怎么对 widget 做动画？

在 iOS 中，你通过调用 `animate(withDuration:animations:)` 方法来给一个 view 创建动画。在 Flutter 中，使用动画库来包裹 widgets，而不是创建一个动画 widget。

在 Flutter 中，使用 `AnimationController` 。这是一个可以暂停、寻找、停止、反转动画的 `Animation<double>` 类型。它需要一个 `Ticker` 当 vsync 发生时来发送信号，并且在每帧运行时创建一个介于 0 和 1 之间的线性插值（interpolation）。你可以创建一个或多个的 `Animation` 并附加给一个 controller。

例如，你可能会用 `CurvedAnimation` 来实现一个 interpolated 曲线。在这个场景中，controller 是动画过程的“主人”，而 `CurvedAnimation` 计算曲线，并替代 controller 默认的线性模式。

当构建 widget 树时，你会把 `Animation` 指定给一个 widget 的动画属性，比如 `FadeTransition` 的 opacity，并告诉控制器开始动画。

下面这个例子展示了在点击 `FloatingActionButton` 之后，如何使用 `FadeTransition` 来让 widget 淡出到 logo 图标：

```dart
class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fade Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyFadeTest(title: 'Fade Demo'),
    );
  }
}

class MyFadeTest extends StatefulWidget {
  MyFadeTest({Key key, this.title}) : super(key: key);

  final String title;

  @override
  _MyFadeTest createState() => _MyFadeTest();
}

class _MyFadeTest extends State<MyFadeTest> with TickerProviderStateMixin {
  AnimationController controller;
  CurvedAnimation curve;

  @override
  void initState() {
    controller = AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
    curve = CurvedAnimation(parent: controller, curve: Curves.easeIn);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Container(
          child: FadeTransition(
            opacity: curve,
            child: FlutterLogo(
              size: 100.0,
            )
          )
        )
      ),
      floatingActionButton: FloatingActionButton(
        tooltip: 'Fade',
        child: Icon(Icons.brush),
        onPressed: () {
          controller.forward();
        },
      ),
    );
  }

  @override
  dispose() {
    controller.dispose();
    super.dispose();
  }
}
```

更多信息，请参阅 [Animation & Motion widgets](https://flutter.io/widgets/animation/)， [Animations tutorial](https://flutter.io/tutorials/animation) 以及 [Animations overview](https://flutter.io/animations/)。

### 我该怎么绘图？

在 iOS 上，你通过 `CoreGraphics` 来在屏幕上绘制线条和形状。Flutter 有一套基于 `Canvas` 类的不同的 API，还有 `CustomPaint` 和 `CustomPainter` 这两个类来帮助你绘图。后者实现你在 canvas 上的绘图算法。

想要学习如何实现一个笔迹画笔，请参考 Collin 在 [StackOverflow](https://stackoverflow.com/questions/46241071/create-signature-area-for-mobile-app-in-dart-flutter) 上的回答。

```dart
class SignaturePainter extends CustomPainter {
  SignaturePainter(this.points);

  final List<Offset> points;

  void paint(Canvas canvas, Size size) {
    var paint = Paint()
      ..color = Colors.black
      ..strokeCap = StrokeCap.round
      ..strokeWidth = 5.0;
    for (int i = 0; i < points.length - 1; i++) {
      if (points[i] != null && points[i + 1] != null)
        canvas.drawLine(points[i], points[i + 1], paint);
    }
  }

  bool shouldRepaint(SignaturePainter other) => other.points != points;
}

class Signature extends StatefulWidget {
  SignatureState createState() => SignatureState();
}

class SignatureState extends State<Signature> {

  List<Offset> _points = <Offset>[];

  Widget build(BuildContext context) {
    return GestureDetector(
      onPanUpdate: (DragUpdateDetails details) {
        setState(() {
          RenderBox referenceBox = context.findRenderObject();
          Offset localPosition =
          referenceBox.globalToLocal(details.globalPosition);
          _points = List.from(_points)..add(localPosition);
        });
      },
      onPanEnd: (DragEndDetails details) => _points.add(null),
      child: CustomPaint(painter: SignaturePainter(_points), size: Size.infinite),
    );
  }
}
```

### Widget 的透明度在哪里？

在 iOS 中，什么东西都会有一个 .opacity 或是 .alpha 的属性。在 Flutter 中，你需要给 widget 包裹一个 Opacity widget 来做到这一点。

### 我怎么创建自定义的 widgets？

在 iOS 中，你编写 `UIView` 的子类，或使用已经存在的 view 来重载并实现方法，以达到特定的功能。在 Flutter 中，你会组合（[composing](https://flutter.io/technical-overview/#everythings-a-widget)）多个小的 widgets 来构建一个自定义的 widget（而不是扩展它）。

举个例子，如果你要构建一个 `CustomButton` ，并在构造器中传入它的 label？那就组合 `RaisedButton` 和 label，而不是扩展 `RaisedButton`。

```dart
class CustomButton extends StatelessWidget {
  final String label;

  CustomButton(this.label);

  @override
  Widget build(BuildContext context) {
    return RaisedButton(onPressed: () {}, child: Text(label));
  }
}
```

然后就像你使用其他任何 Flutter 的 widget 一样，使用你的 CustomButton：

```dart
@override
Widget build(BuildContext context) {
  return Center(
    child: CustomButton("Hello"),
  );
}
```

## 导航

### 我怎么在不同页面之间跳转？

在 iOS 中，你可以使用管理了 view controller 栈的 `UINavigationController` 来在不同的 view controller 之间跳转。

Flutter 也有类似的实现，使用了 `Navigator` 和 `Routes`。一个路由是 App 中“屏幕”或“页面”的抽象，而一个 Navigator 是管理多个路由的 [widget](https://flutter.io/flutter-for-ios/technical-overview/#everythings-a-widget) 。你可以粗略地把一个路由对应到一个 `UIViewController`。Navigator 的工作原理和 iOS 中 `UINavigationController` 非常相似，当你想跳转到新页面或者从新页面返回时，它可以 `push()` 和 `pop()` 路由。

在页面之间跳转，你有几个选择：

- 具体指定一个由路由名构成的 `Map`。（MaterialApp）
- 直接跳转到一个路由。（WidgetApp）

下面是构建一个 Map 的例子：

```dart
void main() {
  runApp(MaterialApp(
    home: MyAppHome(), // becomes the route named '/'
    routes: <String, WidgetBuilder> {
      '/a': (BuildContext context) => MyPage(title: 'page A'),
      '/b': (BuildContext context) => MyPage(title: 'page B'),
      '/c': (BuildContext context) => MyPage(title: 'page C'),
    },
  ));
}
```

通过把路由的名字 `push` 给一个 `Navigator` 来跳转：

```dart
Navigator.of(context).pushNamed('/b');
```

`Navigator` 类不仅用来处理 Flutter 中的路由，还被用来获取你刚 push 到栈中的路由返回的结果。通过 `await`等待路由返回的结果来达到这点。

举个例子，要跳转到“位置”路由来让用户选择一个地点，你可能要这么做：

```dart
Map coordinates = await Navigator.of(context).pushNamed('/location');
```

之后，在 location 路由中，一旦用户选择了地点，携带结果一起 `pop()` 出栈：

```dart
Navigator.of(context).pop({"lat":43.821757,"long":-79.226392});
```

### 我怎么跳转到其他 App？

在 iOS 中，要跳转到其他 App，你需要一个特定的 URL Scheme。对系统级别的 App 来说，这个 scheme 取决于 App。为了在 Flutter 中实现这个功能，你可以创建一个原生平台的整合层，或者使用现有的 [plugin](https://flutter.io/flutter-for-ios/#plugins)，例如 [url_launcher](https://pub.dartlang.org/packages/url_launcher)。

## 线程和异步

### 我怎么编写异步的代码？

Dart 是单线程执行模型，但是它支持 `Isolate`（一种让 Dart 代码运行在其他线程的方式）、事件循环和异步编程。除非你自己创建一个 `Isolate` ，否则你的 Dart 代码永远运行在 UI 线程，并由 event loop 驱动。Flutter 的 event loop 和 iOS 中的 main loop 相似——`Looper` 是附加在主线程上的。

Dart 的单线程模型并不意味着你写的代码一定是阻塞操作，从而卡住 UI。相反，使用 Dart 语言提供的异步工具，例如 `async` / `await` ，来实现异步操作。

举个例子，你可以使用 `async` / `await` 来让 Dart 帮你做一些繁重的工作，编写网络请求代码而不会挂起 UI：

```dart
loadData() async {
  String dataURL = "https://jsonplaceholder.typicode.com/posts";
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = json.decode(response.body);
  });
}
```

一旦 `await` 到网络请求完成，通过调用 `setState()` 来更新 UI，这会触发 widget 子树的重建，并更新相关数据。

下面的例子展示了异步加载数据，并用 `ListView` 展示出来：

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();

    loadData();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView.builder(
          itemCount: widgets.length,
          itemBuilder: (BuildContext context, int position) {
            return getRow(position);
          }));
  }

  Widget getRow(int i) {
    return Padding(
      padding: EdgeInsets.all(10.0),
      child: Text("Row ${widgets[i]["title"]}")
    );
  }

  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
```

 更多关于在后台工作的信息，以及 Flutter 和 iOS 的区别，请参考下一章节。

### 你是怎么把工作放到后台线程的？

由于 Flutter 是单线程并且跑着一个 event loop 的（就像 Node.js 那样），你不必为线程管理或是开启后台线程而操心。如果你正在做 I/O 操作，如访问磁盘或网络请求，安全地使用 `async` / `await` 就完事了。如果，在另外的情况下，你需要做让 CPU 执行繁忙的计算密集型任务，你需要使用 `Isolate` 来避免阻塞 event loop。

对于 I/O 操作，通过关键字 `async`，把方法声明为异步方法，然后通过`await`关键字等待该异步方法执行完成(译者语：这和javascript中是相同的)：

```dart
loadData() async {
  String dataURL = "https://jsonplaceholder.typicode.com/posts";
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = json.decode(response.body);
  });
}
```

这就是对诸如网络请求或数据库访问等 I/O 操作的典型做法。

然而，有时候你需要处理大量的数据，这会导致你的 UI 挂起。在 Flutter 中，使用 `Isolate` 来发挥多核心 CPU 的优势来处理那些长期运行或是计算密集型的任务。

Isolates 是分离的运行线程，并且不和主线程的内存堆共享内存。这意味着你不能访问主线程中的变量，或者使用 `setState()` 来更新 UI。正如它们的名字一样，Isolates 不能共享内存。

下面的例子展示了一个简单的 isolate，是如何把数据返回给主线程来更新 UI 的：

```dart
loadData() async {
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(dataLoader, receivePort.sendPort);

  // The 'echo' isolate sends its SendPort as the first message
  SendPort sendPort = await receivePort.first;

  List msg = await sendReceive(sendPort, "https://jsonplaceholder.typicode.com/posts");

  setState(() {
    widgets = msg;
  });
}

// The entry point for the isolate
static dataLoader(SendPort sendPort) async {
  // Open the ReceivePort for incoming messages.
  ReceivePort port = ReceivePort();

  // Notify any other isolates what port this isolate listens to.
  sendPort.send(port.sendPort);

  await for (var msg in port) {
    String data = msg[0];
    SendPort replyTo = msg[1];

    String dataURL = data;
    http.Response response = await http.get(dataURL);
    // Lots of JSON to parse
    replyTo.send(json.decode(response.body));
  }
}

Future sendReceive(SendPort port, msg) {
  ReceivePort response = ReceivePort();
  port.send([msg, response.sendPort]);
  return response.first;
}
```

这里，`dataLoader()` 是一个运行于自己独立执行线程上的 `Isolate`。在 isolate 里，你可以执行 CPU 密集型任务（例如解析一个庞大的 json），或是计算密集型的数学操作，如加密或信号处理等。

你可以运行下面的完整例子：

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:async';
import 'dart:isolate';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  showLoadingDialog() {
    if (widgets.length == 0) {
      return true;
    }

    return false;
  }

  getBody() {
    if (showLoadingDialog()) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  getProgressDialog() {
    return Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Sample App"),
        ),
        body: getBody());
  }

  ListView getListView() => ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (BuildContext context, int position) {
        return getRow(position);
      });

  Widget getRow(int i) {
    return Padding(padding: EdgeInsets.all(10.0), child: Text("Row ${widgets[i]["title"]}"));
  }

  loadData() async {
    ReceivePort receivePort = ReceivePort();
    await Isolate.spawn(dataLoader, receivePort.sendPort);

    // The 'echo' isolate sends its SendPort as the first message
    SendPort sendPort = await receivePort.first;

    List msg = await sendReceive(sendPort, "https://jsonplaceholder.typicode.com/posts");

    setState(() {
      widgets = msg;
    });
  }

// the entry point for the isolate
  static dataLoader(SendPort sendPort) async {
    // Open the ReceivePort for incoming messages.
    ReceivePort port = ReceivePort();

    // Notify any other isolates what port this isolate listens to.
    sendPort.send(port.sendPort);

    await for (var msg in port) {
      String data = msg[0];
      SendPort replyTo = msg[1];

      String dataURL = data;
      http.Response response = await http.get(dataURL);
      // Lots of JSON to parse
      replyTo.send(json.decode(response.body));
    }
  }

  Future sendReceive(SendPort port, msg) {
    ReceivePort response = ReceivePort();
    port.send([msg, response.sendPort]);
    return response.first;
  }
}
```

### 我怎么发起网络请求？

在 Flutter 中，使用流行的 [http package](https://pub.dartlang.org/packages/http) 做网络请求非常简单。它把你可能需要自己做的网络请求操作抽象了出来，让发起请求变得简单。

要使用 `http` 包，在 `pubspec.yaml` 中把它添加为依赖：

```
dependencies:
  ...
  http: ^0.11.3+16
```

发起网络请求，在 `http.get()` 这个 `async` 方法中使用 `await` ：

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
[...]
  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
```

### 我怎么展示一个长时间运行的任务的进度？

在 iOS 中，在后台运行耗时任务时你会使用 `UIProgressView`。 

在 Flutter 中，使用一个 `ProgressIndicator` widget。通过一个布尔 flag 来控制是否展示进度。在任务开始时，告诉 Flutter 更新状态，并在结束后隐去。

在下面的例子中，build 函数被拆分成三个函数。如果 `showLoadingDialog()` 是 `true` （当 `widgets.length == 0` 时），则渲染 `ProgressIndicator`。否则，当数据从网络请求中返回时，渲染 `ListView` 。

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  showLoadingDialog() {
    return widgets.length == 0;
  }

  getBody() {
    if (showLoadingDialog()) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  getProgressDialog() {
    return Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Sample App"),
        ),
        body: getBody());
  }

  ListView getListView() => ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (BuildContext context, int position) {
        return getRow(position);
      });

  Widget getRow(int i) {
    return Padding(padding: EdgeInsets.all(10.0), child: Text("Row ${widgets[i]["title"]}"));
  }

  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
```

## 工程结构、本地化、依赖和资源

### 我怎么在 Flutter 中引入 image assets？多分辨率怎么办？

iOS 把 images 和 assets 作为不同的东西，而 Flutter 中只有 assets。被放到 iOS 中 `Images.xcasset` 文件夹下的资源在 Flutter 中被放到了 assets 文件夹中。assets 可以是任意类型的文件，而不仅仅是图片。例如，你可以把 json 文件放置到 `my-assets` 文件夹中。

```
my-assets/data.json
```

在 `pubspec.yaml` 文件中声明 assets：

```
assets:
 - my-assets/data.json
```

然后在代码中使用 [`AssetBundle`](https://docs.flutter.io/flutter/services/AssetBundle-class.html) 来访问它：

```dart
import 'dart:async' show Future;
import 'package:flutter/services.dart' show rootBundle;

Future<String> loadAsset() async {
  return await rootBundle.loadString('my-assets/data.json');
}
```

对于图片，Flutter 像 iOS 一样，遵循了一个简单的基于像素密度的格式。Image assets 可能是 `1.0x` `2.0x` `3.0x` 或是其他的任何倍数。这些所谓的 [`devicePixelRatio`](https://docs.flutter.io/flutter/dart-ui/Window/devicePixelRatio.html) 传达了物理像素到单个逻辑像素的比率。

Assets 可以被放置到任何属性文件夹中——Flutter 并没有预先定义的文件结构。在 `pubspec.yaml` 文件中声明 assets （和位置），然后 Flutter 会把他们识别出来。

举个例子，要把一个叫 `my_icon.png` 的图片放到 Flutter 工程中，你可能想要把存储它的文件夹叫做 `images`。把基础图片（1.0x）放置到 `images` 文件夹中，并把其他变体放置在子文件夹中，并接上合适的比例系数：

```
images/my_icon.png       // Base: 1.0x image
images/2.0x/my_icon.png  // 2.0x image
images/3.0x/my_icon.png  // 3.0x image
```

接着，在 `pubspec.yaml` 文件夹中声明这些图片：

```
assets:
 - images/my_icon.jpeg
```

你可以用 `AssetImage` 来访问这些图片：

```dart
return AssetImage("images/a_dot_burr.jpeg");
```

或者在 `Image` widget 中直接使用：

```dart
@override
Widget build(BuildContext context) {
  return Image.asset("images/my_image.png");
}
```

更多细节，参见 [Adding Assets and Images in Flutter](https://flutter.io/assets-and-images)。

### 我在哪里放置字符串？我怎么做本地化？

不像 iOS 拥有一个 `Localizable.strings` 文件，Flutter 目前并没有一个用于处理字符串的系统。目前，最佳实践是把你的文本拷贝到静态区，并在这里访问。例如：

```dart
class Strings {
  static String welcomeMessage = "Welcome To Flutter";
}
```

并且这样访问你的字符串：

```dart
Text(Strings.welcomeMessage)
```

默认情况下，Flutter 只支持美式英语字符串。如果你要支持其他语言，请引入 `flutter_localizations` 包。你可能也要引入  [`intl`](https://pub.dartlang.org/packages/intl) 包来支持其他的 i10n 机制，比如日期/时间格式化。

```
dependencies:
  # ...
  flutter_localizations:
    sdk: flutter
  intl: "^0.15.6"
```

要使用 `flutter_localizations` 包，还需要在 app widget 中指定 `localizationsDelegates` 和 `supportedLocales`。

```dart
import 'package:flutter_localizations/flutter_localizations.dart';

MaterialApp(
 localizationsDelegates: [
   // Add app-specific localization delegate[s] here
   GlobalMaterialLocalizations.delegate,
   GlobalWidgetsLocalizations.delegate,
 ],
 supportedLocales: [
    const Locale('en', 'US'), // English
    const Locale('he', 'IL'), // Hebrew
    // ... other locales the app supports
  ],
  // ...
)
```

这些代理包括了实际的本地化值，并且 `supportedLocales` 定义了 App 支持哪些地区。上面的例子使用了一个 `MaterialApp` ，所以它既有 `GlobalWidgetsLocalizations` 用于基础 widgets，也有 `MaterialWidgetsLocalizations` 用于 Material wigets 的本地化。如果你使用 `WidgetsApp` ，则无需包括后者。注意，这两个代理虽然包括了“默认”值，但如果你想让你的 App 本地化，你仍需要提供一或多个代理作为你的 App 本地化副本。

当初始化时，`WidgetsApp` 或 `MaterialApp` 会使用你指定的代理为你创建一个  [`Localizations`](https://docs.flutter.io/flutter/widgets/Localizations-class.html) widget。`Localizations` widget 可以随时从当前上下文中访问设备的地点，或者使用 [`Window.locale`](https://docs.flutter.io/flutter/dart-ui/Window/locale.html)。

要访问本地化文件，使用 `Localizations.of()` 方法来访问提供代理的特定本地化类。如需翻译，使用  [`intl_translation`](https://pub.dartlang.org/packages/intl_translation) 包来取出翻译副本到 [arb](https://code.google.com/p/arb/wiki/ApplicationResourceBundleSpecification) 文件中。把它们引入 App 中，并用 `intl` 来使用它们。

更多 Flutter 中国际化和本地化的细节，请访问 [internationalization guide](https://flutter.io/tutorials/internationalization) ，那里有不使用 `intl` 包的示例代码。

注意，在 Flutter 1.0 beta 2 之前，在 Flutter 中定义的 assets 不能在原生一侧被访问。原生定义的资源在 Flutter 中也不可用，因为它们在独立的文件夹中。

### Cocoapods 相当于什么？我该如何添加依赖？

在 iOS 中，你把依赖添加到 `Podfile` 中。Flutter 使用 Dart 构建系统和 Pub 包管理器来处理依赖。这些工具将本机 Android 和 iOS 包装应用程序的构建委派给相应的构建系统。

如果你的 Flutter 工程中的 iOS 文件夹中拥有 Podfile，请仅在你为每个平台集成时使用它。总体来说，使用 `pubspec.yaml` 来在 Flutter 中声明外部依赖。一个可以找到优秀 Flutter 包的地方是 [Pub](https://pub.dartlang.org/flutter/packages/)。

## ViewControllers

### ViewController 相当于 Flutter 中的什么？

在 iOS 中，一个 ViewController 代表了用户界面的一部分，最常用于一个屏幕，或是其中一部分。它们被组合在一起用于构建复杂的用户界面，并帮助你拆分 App 的 UI。在 Flutter 中，这一任务回落到了 widgets 中。就像在界面导航部分提到的一样，一个屏幕也是被 widgets 来表示的，因为“万物皆 widget！”。使用 `Navigator` 在 `Route` 之间跳转，或者渲染相同数据的不同状态。

### 我该怎么监听 iOS 中的生命周期事件？

在 iOS 中，你可以重写 `ViewController` 中的方法来补货它的视图的生命周期，或者在 `AppDelegate` 中注册生命周期的回调函数。在 Flutter 中没有这两个概念，但你可以通过 hook `WidgetsBinding` 观察者来监听生命周期事件，并监听 `didChangeAppLifecycleState()` 的变化事件。

可观察的生命周期事件有：

- `inactive` - 应用处于不活跃的状态，并且不会接受用户的输入。这个事件仅工作在 iOS 平台，在 Android 上没有等价的事件。
- `paused` - 应用暂时对用户不可见，虽然不接受用户输入，但是是在后台运行的。
- `resumed` - 应用可见，也响应用户的输入。
- `suspending` - 应用暂时被挂起，在 iOS 上没有这一事件。

更多关于这些状态的细节和含义，请参见  [`AppLifecycleStatus` documentation](https://docs.flutter.io/flutter/dart-ui/AppLifecycleState-class.html) 。

## 布局

### UITableView 和 UICollectionView 相当于 Flutter 中的什么？

在 iOS 中，你可能用 UITableView 或 UICollectionView 来展示一个列表。在 Flutter 中，你可以用 `ListView` 来达到相似的实现。在 iOS 中，你通过代理方法来确定行数，每一个 index path 的单元格，以及单元格的尺寸。

由于 Flutter 中 widget 的不可变特性，你需要向 `ListView` 传递一个 widget 列表，Flutter 会确保滚动是快速且流畅的。

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(Padding(padding: EdgeInsets.all(10.0), child: Text("Row $i")));
    }
    return widgets;
  }
}
```

### 我怎么知道列表的哪个元素被点击了？

iOS 中，你通过 `tableView:didSelectRowAtIndexPath:` 代理方法来实现。在 Flutter 中，使用传递进来的 widget 的 touch handle：

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(GestureDetector(
        child: Padding(
          padding: EdgeInsets.all(10.0),
          child: Text("Row $i"),
        ),
        onTap: () {
          print('row tapped');
        },
      ));
    }
    return widgets;
  }
}
```

### 我怎么动态地更新 ListView？

在 iOS 中，你改变列表的数据，并通过 `reloadData()` 方法来通知 table 或是 collection view。

在 Flutter 中，如果你想通过 `setState()` 方法来更新 widget 列表，你会很快发现你的数据展示并没有变化。这是因为当 `setState()` 被调用时，Flutter 渲染引擎会去检查 widget 树来查看是否有什么地方被改变了。当它得到你的 `ListView` 时，它会使用一个 `==` 判断，并且发现两个 `ListView` 是相同的。没有什么东西是变了的，因此更新不是必须的。

一个更新 `ListView` 的简单方法是，在 `setState()` 中创建一个新的 list，并把旧 list 的数据拷贝给新的 list。虽然这样很简单，但当数据集很大时，并不推荐这样做：

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: widgets),
    );
  }

  Widget getRow(int i) {
    return GestureDetector(
      child: Padding(
        padding: EdgeInsets.all(10.0),
        child: Text("Row $i"),
      ),
      onTap: () {
        setState(() {
          widgets = List.from(widgets);
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
```

一个推荐的、高效的且有效的做法是，使用 `ListView.Builder` 来构建列表。这个方法在你想要构建动态列表，或是列表拥有大量数据时会非常好用。

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView.builder(
        itemCount: widgets.length,
        itemBuilder: (BuildContext context, int position) {
          return getRow(position);
        },
      ),
    );
  }

  Widget getRow(int i) {
    return GestureDetector(
      child: Padding(
        padding: EdgeInsets.all(10.0),
        child: Text("Row $i"),
      ),
      onTap: () {
        setState(() {
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
```

与创建一个 “ListView” 不同，创建一个 `ListView.builder` 接受两个主要参数：列表的初始长度，和一个 `ItemBuilder` 方法。

`ItemBuilder` 方法和 `cellForItemAt` 代理方法非常类似，它接受一个位置，并且返回在这个位置上你希望渲染的 cell。

最后，也是最重要的，注意 `onTap()` 函数里并没有重新创建一个 list，而是 `.add` 了一个 widget。

### ScrollView 相当于 Flutter 里的什么？

在 iOS 中，你给 view 包裹上 `ScrollView` 来允许用户在需要时滚动你的内容。

在 Flutter 中，最简单的方法是使用 `ListView` widget。它表现得既和 iOS 中的 `ScrollView` 一致，也能和 `TableView` 一致，因为你可以给它的 widget 做垂直排布：

```dart
@override
Widget build(BuildContext context) {
  return ListView(
    children: <Widget>[
      Text('Row One'),
      Text('Row Two'),
      Text('Row Three'),
      Text('Row Four'),
    ],
  );
}
```

更多关于在 Flutter 总如何排布 widget 的文档，请参阅 [layout tutorial](https://flutter.io/widgets/layout/)。

## 手势检测及触摸事件处理

### 我怎么给 Flutter 的 widget 添加一个点击监听者？

在 iOS 中，你给一个 view 添加 `GestureRecognizer` 来处理点击事件。在 Flutter 中，有两种方法来添加点击监听者：

1. 如果 widget 本身支持事件监测，直接传递给它一个函数，并在这个函数里实现响应方法。例如，`RaisedButton` widget 拥有一个 `RaisedButton` 参数：

   ```dart
   @override
   Widget build(BuildContext context) {
     return RaisedButton(
       onPressed: () {
         print("click");
       },
       child: Text("Button"),
     );
   }
   ```

2. 如果 widget 本身不支持事件监测，则在外面包裹一个 GestureDetector，并给它的 onTap 属性传递一个函数：

   ```dart
   class SampleApp extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return Scaffold(
         body: Center(
           child: GestureDetector(
             child: FlutterLogo(
               size: 200.0,
             ),
             onTap: () {
               print("tap");
             },
           ),
         ),
       );
     }
   }
   ```

### 我怎么处理 widget 上的其他手势？

使用 `GestureDetector` 你可以监听更广阔范围内的手势，比如：

- Tapping
  - `onTapDown` — 在特定位置轻触手势接触了屏幕。
  - `onTapUp` — 在特定位置产生了一个轻触手势，并停止接触屏幕。
  - `onTap` — 产生了一个轻触手势。
  - `onTapCancel` — 触发了 `onTapDown` 但没能触发 tap。
- Double tapping
  - `onDoubleTap` — 用户在同一个位置快速点击了两下屏幕。
- Long pressing
  - `onLongPress` — 用户在同一个位置长时间接触屏幕。
- Vertical dragging
  - `onVerticalDragStart` — 接触了屏幕，并且可能会垂直移动。
  - `onVerticalDragUpdate` — 接触了屏幕，并继续在垂直方向移动。
  - `onVerticalDragEnd` — 之前接触了屏幕并垂直移动，并在停止接触屏幕前以某个垂直的速度移动。
- Horizontal dragging
  - `onHorizontalDragStart` — 接触了屏幕，并且可能会水平移动。
  - `onHorizontalDragUpdate` — 接触了屏幕，并继续在水平方向移动。
  - `onHorizontalDragEnd` — 之前接触屏幕并水平移动的触摸点与屏幕分离。

下面这个例子展示了一个 `GestureDetector` 是如何在双击时旋转 Flutter 的 logo 的：

```dart
AnimationController controller;
CurvedAnimation curve;

@override
void initState() {
  controller = AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
  curve = CurvedAnimation(parent: controller, curve: Curves.easeIn);
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: GestureDetector(
          child: RotationTransition(
            turns: curve,
            child: FlutterLogo(
              size: 200.0,
            )),
          onDoubleTap: () {
            if (controller.isCompleted) {
              controller.reverse();
            } else {
              controller.forward();
            }
          },
        ),
      ),
    );
  }
}
```

## 主题和文字

### 我怎么给 App 设置主题？

Flutter 实现了一套漂亮的 MD 组件，并且开箱可用。它接管了一大堆你需要的样式和主题。

为了充分发挥你的 App 中 MD 组件的优势，声明一个顶级 widget，MaterialApp，用作你的 App 入口。MaterialApp 是一个便利组件，包含了许多 App 通常需要的 MD 风格组件。它通过一个 WidgetsApp 添加了 MD 功能来实现。

但是 Flutter 足够地灵活和富有表现力来实现任何其他的设计语言。在 iOS 上，你可以用 [Cupertino library](https://docs.flutter.io/flutter/cupertino/cupertino-library.html) 来制作遵守  [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/overview/themes/) 的界面。查看这些 widget 的集合，请参阅 [Cupertino widgets gallery](https://flutter.io/widgets/cupertino/)。

你也可以在你的 App 中使用 WidgetApp，它提供了许多相似的功能，但不如 `MaterialApp` 那样强大。

对任何子组件定义颜色和样式，可以给 `MaterialApp` widget 传递一个 `ThemeData` 对象。举个例子，在下面的代码中，primary swatch 被设置为蓝色，并且文字的选中颜色是红色：

```dart
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        textSelectionColor: Colors.red
      ),
      home: SampleAppPage(),
    );
  }
}
```

### 我怎么给 Text widget 设置自定义字体？

在 iOS 中，你在项目中引入任意的 `ttf` 文件，并在 `info.plist` 中设置引用。在 Flutter 中，在文件夹中放置字体文件，并在 `pubspec.yaml` 中引用它，就像添加图片那样。

```
fonts:
   - family: MyCustomFont
     fonts:
       - asset: fonts/MyCustomFont.ttf
       - style: italic
```

然后在你的 `Text` widget 中指定字体：

```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text("Sample App"),
    ),
    body: Center(
      child: Text(
        'This is a custom font text',
        style: TextStyle(fontFamily: 'MyCustomFont'),
      ),
    ),
  );
}
```

### 我怎么给我的 Text widget 设置样式？

除了字体以外，你也可以给 Text widget 的样式元素设置自定义值。`Text` widget 接受一个  `TextStyle` 对象，你可以指定许多参数，比如：

- `color`
- `decoration`
- `decorationColor`
- `decorationStyle`
- `fontFamily`
- `fontSize`
- `fontStyle`
- `fontWeight`
- `hashCode`
- `height`
- `inherit`
- `letterSpacing`
- `textBaseline`
- `wordSpacing`

## 表单输入

### Flutter 中表单怎么工作？我怎么拿到用户的输入？

我们已经提到 Flutter 使用不可变的 widget，并且状态是分离的，你可能会好奇在这种情境下怎么处理用户的输入。在 iOS 中，你经常在需要提交数据时查询组件当前的状态或动作，但这在 Flutter 中是怎么工作的呢？

在表单处理的实践中，就像在 Flutter 中任何其他的地方一样，要通过特定的 widgets。如果你有一个 `TextField` 或是 `TextFormField`，你可以通过 [`TextEditingController`](https://docs.flutter.io/flutter/widgets/TextEditingController-class.html) 来获得用户输入：

```dart
class _MyFormState extends State<MyForm> {
  // Create a text controller and use it to retrieve the current value.
  // of the TextField!
  final myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when disposing of the Widget.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          controller: myController,
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // When the user presses the button, show an alert dialog with the
        // text the user has typed into our text field.
        onPressed: () {
          return showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                // Retrieve the text the user has typed in using our
                // TextEditingController
                content: Text(myController.text),
              );
            },
          );
        },
        tooltip: 'Show me the value!',
        child: Icon(Icons.text_fields),
      ),
    );
  }
}
```

你可以在这里获得更多信息，或是完整的代码列表： [Retrieve the value of a text field](https://flutter.io/cookbook/forms/retrieve-input/)，来自 [Flutter Cookbook](https://flutter.io/cookbook/) 。

### Text field 中的 placeholder 相当于什么？

在 Flutter 中，你可以轻易地通过向 Text widget 的装饰构造器参数重传递 `InputDecoration` 来展示“小提示”，或是占位符文字：

```dart
body: Center(
  child: TextField(
    decoration: InputDecoration(hintText: "This is a hint"),
  ),
)
```

### 我怎么展示验证错误信息？

就像展示“小提示”一样，向 Text widget 的装饰器构造器参数中传递一个 `InputDecoration`。

然而，你并不想在一开始就显示错误信息。相反，当用户输入了验证信息，更新状态，并传入一个新的 `InputDecoration` 对象：

```dart
class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  String _errorText;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(
        child: TextField(
          onSubmitted: (String text) {
            setState(() {
              if (!isEmail(text)) {
                _errorText = 'Error: This is not an email';
              } else {
                _errorText = null;
              }
            });
          },
          decoration: InputDecoration(hintText: "This is a hint", errorText: _getErrorText()),
        ),
      ),
    );
  }

  _getErrorText() {
    return _errorText;
  }

  bool isEmail(String em) {
    String emailRegexp =
        r'^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$';

    RegExp regExp = RegExp(p);

    return regExp.hasMatch(em);
  }
}
```

## 和硬件、第三方服务以及平台交互

### 我怎么和平台，以及平台的原生代码交互？

Flutter 的代码并不直接在平台之下运行，相反，Dart 代码构建的 Flutter 应用在设备上以原生的方式运行，却“侧步躲开了”平台提供的 SDK。这意味着，例如，你在 Dart 中发起一个网络请求，它就直接在 Dart 的上下文中运行。你并不会用上平常在 iOS 或 Android 上使用的原生 API。你的 Flutter 程序仍然被原生平台的 `ViewController` 管理作一个 view，但是你并不会直接访问 `ViewController` 自身，或是原生框架。

但这并不意味着 Flutter 不能和原生 API，或任何你编写的原生代码交互。Flutter 提供了 [platform channels](https://flutter.io/platform-channels/) ，来和管理你的 Flutter view 的 ViewController 通信和交互数据。平台管道本质上是一个异步通信机制，桥接了 Dart 代码和宿主 ViewController，以及它运行于的 iOS 框架。你可以用平台管道来执行一个原生的函数，或者是从设备的传感器中获取数据。

除了直接使用平台管道之外，你还可以使用一系列预先制作好的 [plugins](https://flutter.io/using-packages/)。例如，你可以直接使用插件来访问相机胶卷或是设备的摄像头，而不必编写你自己的集成层代码。你可以在 [Pub](https://pub.dartlang.org/) 上找到插件，这是一个 Dart 和 Flutter 的开源包仓库。其中一些包可能会支持集成 iOS 或 Android，或两者均可。

如果你在 Pub 上找不到符合你需求的插件，你可以[自己编写](https://flutter.io/developing-packages/) ，并且[发布在 Pub 上](https://flutter.io/developing-packages/#publish)。

### 我怎么访问 GPS 传感器？

使用 [`location`](https://pub.dartlang.org/packages/location) 社区插件。

### 我怎么访问摄像头？

[`image_picker`](https://pub.dartlang.org/packages/image_picker) 在访问摄像头时非常常用。

### 我怎么登录 Facebook？

登录 Facebook 可以使用 [`flutter_facebook_login`](https://pub.dartlang.org/packages/flutter_facebook_login) 社区插件。

### 我怎么使用 Firebase 特性？

大多数 Firebase 特性被  [first party plugins](https://pub.dartlang.org/flutter/packages?q=firebase) 包含了。这些第一方插件由 Flutter 团队维护：

- [`firebase_admob`](https://pub.dartlang.org/packages/firebase_admob) for Firebase AdMob
- [`firebase_analytics`](https://pub.dartlang.org/packages/firebase_analytics) for Firebase Analytics
- [`firebase_auth`](https://pub.dartlang.org/packages/firebase_auth) for Firebase Auth
- [`firebase_core`](https://pub.dartlang.org/packages/firebase_core) for Firebase’s Core package
- [`firebase_database`](https://pub.dartlang.org/packages/firebase_database) for Firebase RTDB
- [`firebase_storage`](https://pub.dartlang.org/packages/firebase_storage) for Firebase Cloud Storage
- [`firebase_messaging`](https://pub.dartlang.org/packages/firebase_messaging) for Firebase Messaging (FCM)
- [`cloud_firestore`](https://pub.dartlang.org/packages/cloud_firestore) for Firebase Cloud Firestore

你也可以在 Pub 上找到 Firebase 的第三方插件。

### 我怎创建自己的原生集成层？

如果有一些 Flutter 和社区插件遗漏的平台相关的特性，可以根据  [developing packages and plugins](https://flutter.io/developing-packages/) 页面构建自己的插件。

Flutter 的插件结构，简要来说，就像 Android 中的 Event bus。你发送一个消息，并让接受者处理并反馈结果给你。在这种情况下，接受者就是在 Android 或 iOS 上的原生代码。

## 数据库和本地存储

### 我怎么在 Flutter 中访问 UserDefaults？

在 iOS 中，你可以使用属性列表来存储键值对的集合，即我们熟悉的 UserDefaults。

在 Flutter 中，可以使用  [Shared Preferences plugin](https://pub.dartlang.org/packages/shared_preferences) 来达到相似的功能。它包裹了 `UserDefaluts` 以及 Android 上等价的 `SharedPreferences` 的功能。

### CoreData 相当于 Flutter 中的什么？

在 iOS 中，你通过 CoreData 来存储结构化的数据。这是一个 SQL 数据库的上层封装，让查询和关联模型变得更加简单。

在 Flutter 中，使用 [SQFlite](https://pub.dartlang.org/packages/sqflite) 插件来实现这个功能。

## 通知

### 我怎么推送通知？

在 iOS 中，你需要向苹果开发者平台中注册来允许推送通知。

在 Flutter 中，使用 `firebase_messaging` 插件来实现这一功能。

更多使用 Firebase Cloud Messaging API 的信息，请参阅 [`firebase_messaging`](https://pub.dartlang.org/packages/firebase_messaging) 插件文档。
