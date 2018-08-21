---
layout: page
title: "使用WebSockets"
permalink: /cookbook/networking/web-sockets/
summary: 在Flutter中，除了可以发起正常的HTTP请求外，我们还可以使用WebSocket连接到服务器。WebSocket允许与服务器进行双向通信而无需轮询。
keywords: Flutter网络请求
---

除了正常的HTTP请求外，我们还可以使用WebSocket连接到服务器。WebSocket允许与服务器进行双向通信而无需轮询。

在这个例子中，我们将连接到由[websocket.org提供的测试服务器](http://www.websocket.org/echo.html)。服务器将简单地返回我们发送给它的相同消息！

## 步骤

  1. 连接到WebSocket服务器。
  2. 监听来自服务器的消息。
  3. 将数据发送到服务器。
  4. 关闭WebSocket连接。
  
## 1. 连接到WebSocket服务器

[web_socket_channel](https://pub.dartlang.org/packages/web_socket_channel)
package 提供了我们需要连接到WebSocket服务器的工具.

该package提供了一个`WebSocketChannel`允许我们既可以监听来自服务器的消息，又可以将消息发送到服务器的方法。

在Flutter中，我们可以创建一个`WebSocketChannel`连接到一台服务器：

```dart
final channel = new IOWebSocketChannel.connect('ws://echo.websocket.org');
```

## 2. 监听来自服务器的消息

现在我们建立了连接，我们可以监听来自服务器的消息，在我们发送消息给测试服务器之后，它会返回相同的消息。

我们如何收取消息并显示它们？在这个例子中，我们将使用一个[`StreamBuilder`](https://docs.flutter.io/flutter/widgets/StreamBuilder-class.html) Widget来监听新消息，
并用一个Text Widget来显示它们。

```dart
new StreamBuilder(
  stream: widget.channel.stream,
  builder: (context, snapshot) {
    return new Text(snapshot.hasData ? '${snapshot.data}' : '');
  },
);
```

### 这是如何工作的?

`WebSocketChannel`提供了一个来自服务器的消息Stream 。

该`Stream`类是`dart:async`包中的一个基础类。它提供了一种方法来监听来自数据源的异步事件。与`Future`返回单个异步响应不同，`Stream`类可以随着时间推移传递很多事件。

该[`StreamBuilder`](https://docs.flutter.io/flutter/widgets/StreamBuilder-class.html) Widget将连接到一个Stream，
并在每次收到消息时通知Flutter重新构建界面。

## 3. 将数据发送到服务器

为了将数据发送到服务器，我们会`add`消息给`WebSocketChannel`提供的sink。

```dart
channel.sink.add('Hello!');
```

### 这是如何工作的？

`WebSocketChannel`提供了一个[`StreamSink`](https://docs.flutter.io/flutter/dart-async/StreamSink-class.html)，它将消息发给服务器。

`StreamSink`类提供了给数据源同步或异步添加事件的一般方法。

## 4. 关闭WebSocket连接

在我们使用`WebSocket`后，要关闭连接：
```dart
channel.sink.close();
```

## 完整的例子

```dart
import 'package:flutter/foundation.dart';
import 'package:web_socket_channel/io.dart';
import 'package:flutter/material.dart';
import 'package:web_socket_channel/web_socket_channel.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'WebSocket Demo';
    return new MaterialApp(
      title: title,
      home: new MyHomePage(
        title: title,
        channel: new IOWebSocketChannel.connect('ws://echo.websocket.org'),
      ),
    );
  }
}

class MyHomePage extends StatefulWidget {
  final String title;
  final WebSocketChannel channel;

  MyHomePage({Key key, @required this.title, @required this.channel})
      : super(key: key);

  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  TextEditingController _controller = new TextEditingController();

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(widget.title),
      ),
      body: new Padding(
        padding: const EdgeInsets.all(20.0),
        child: new Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            new Form(
              child: new TextFormField(
                controller: _controller,
                decoration: new InputDecoration(labelText: 'Send a message'),
              ),
            ),
            new StreamBuilder(
              stream: widget.channel.stream,
              builder: (context, snapshot) {
                return new Padding(
                  padding: const EdgeInsets.symmetric(vertical: 24.0),
                  child: new Text(snapshot.hasData ? '${snapshot.data}' : ''),
                );
              },
            )
          ],
        ),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _sendMessage,
        tooltip: 'Send message',
        child: new Icon(Icons.send),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }

  void _sendMessage() {
    if (_controller.text.isNotEmpty) {
      widget.channel.sink.add(_controller.text);
    }
  }

  @override
  void dispose() {
    widget.channel.sink.close();
    super.dispose();
  }
}
```
