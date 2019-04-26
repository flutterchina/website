---
layout: page
title: 在Flutter中发起HTTP网络请求
permalink: /networking/
summary: 本文介绍了如何在Flutter中发起网络请求，包括http、websocket.
keywords: Flutter发起http请求
---

本页介绍如何在Flutter中创建HTTP网络请求。对于socket，请参阅[dart:io][dartio]。

> 注：本篇文档官方使用的是用dart io中的`HttpClient`发起的请求，但`HttpClient`本身功能较弱，很多常用功能都不支持。我们建议您使用[dio](https://github.com/flutterchina/dio) 来发起网络请求，它是一个强大易用的dart http请求库，支持Restful API、FormData、拦截器、请求取消、Cookie管理、文件上传/下载…...详情请查看[github dio](https://github.com/flutterchina/dio) .


* TOC Placeholder
{:toc}

## 发起HTTP请求

http支持位于[`dart:io`][dartio]，所以要创建一个HTTP client， 我们需要添加一个导入：
<!-- skip -->
```dart
import 'dart:io';

var httpClient = new HttpClient();
```

该 client 支持常用的HTTP操作, such as [`GET`][get],
[`POST`][post], [`PUT`][put], [`DELETE`][delete].  

## 处理异步

注意，HTTP API 在返回值中使用了[Dart Futures](https://www.dartlang.org/tutorials/language/futures)。
我们建议使用`async`/`await`语法来调用API。

网络调用通常遵循如下步骤：

1. 创建 client.
2. 构造 Uri.
3. 发起请求, 等待请求，同时您也可以配置请求headers、 body。
4. 关闭请求, 等待响应.
5. 解码响应的内容.

Several of these steps use Future based APIs. Sample APIs calls for each step
above are:
其中的几个步骤使用基于Future的API。上面步骤的示例：

<!-- skip -->
```dart
get() async {
  var httpClient = new HttpClient();
  var uri = new Uri.http(
      'example.com', '/path1/path2', {'param1': '42', 'param2': 'foo'});
  var request = await httpClient.getUrl(uri);
  var response = await request.close();
  var responseBody = await response.transform(UTF8.decoder).join();
}
```

有关完整的代码示例，请参阅下面的“示例”。

## 解码和编码JSON

使用[`dart:convert`](https://docs.flutter.io/flutter/dart-convert/dart-convert-library.html)库可以简单解码和编码JSON。
有关其他的JSON文档，请参阅[JSON和序列化](/json/)。

解码简单的JSON字符串并将响应解析为Map：

<!-- skip -->
```dart
Map data = JSON.decode(responseBody);
// Assume the response body is something like: ['foo', { 'bar': 499 }]
int barValue = data[1]['bar']; // barValue is set to 499
```

要对简单的JSON进行编码，请将简单值（字符串，布尔值或数字字面量）或包含简单值的Map，list等传给encode方法：

<!-- skip -->
```dart
String encodedString = JSON.encode([1, 2, { 'a': null }]);
```

## 示例: 解码 HTTPS GET请求的JSON

以下示例显示了如何在Flutter应用中对HTTPS GET请求返回的JSON数据进行解码

It calls the [httpbin.com](https://httpbin.com) web service testing API,
which then responds with your local IP address. Note that secure
networking (HTTPS) is used.
它调用[httpbin.com](https://httpbin.com)Web service测试API，请注意，使用安全网络请求（HTTPS）

1. 运行`flutter create`，创建一个新的Flutter应用.

1. 将`lib/main.dart`替换为一下内容:

```dart
import 'dart:convert';
import 'dart:io';

import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: new MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key}) : super(key: key);

  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  var _ipAddress = 'Unknown';

  _getIPAddress() async {
    var url = 'https://httpbin.org/ip';
    var httpClient = new HttpClient();

    String result;
    try {
      var request = await httpClient.getUrl(Uri.parse(url));
      var response = await request.close();
      if (response.statusCode == HttpStatus.OK) {
        var json = await response.transform(utf8.decoder).join();
        var data = jsonDecode(json);
        result = data['origin'];
      } else {
        result =
            'Error getting IP address:\nHttp status ${response.statusCode}';
      }
    } catch (exception) {
      result = 'Failed getting IP address';
    }

    // If the widget was removed from the tree while the message was in flight,
    // we want to discard the reply rather than calling setState to update our
    // non-existent appearance.
    if (!mounted) return;

    setState(() {
      _ipAddress = result;
    });
  }

  @override
  Widget build(BuildContext context) {
    var spacer = new SizedBox(height: 32.0);

    return new Scaffold(
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Text('Your current IP address is:'),
            new Text('$_ipAddress.'),
            spacer,
            new RaisedButton(
              onPressed: _getIPAddress,
              child: new Text('Get IP address'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## API 文档

有关完整的API文档，请参阅:

  * [库`dart:io`][dartio]
  * [库`dart:convert`][convert]

[dartio]:     https://docs.flutter.io/flutter/dart-io/dart-io-library.html
[convert]:    https://docs.flutter.io/flutter/dart-convert/dart-convert-library.html
[client]:     https://docs.flutter.io/flutter/dart-io/HttpClient-class.html
[get]:        https://docs.flutter.io/flutter/dart-io/HttpClient/getUrl.html
[post]:       https://docs.flutter.io/flutter/dart-io/HttpClient/postUrl.html
[put]:        https://docs.flutter.io/flutter/dart-io/HttpClient/putUrl.html
[delete]:     https://docs.flutter.io/flutter/dart-io/HttpClient/deleteUrl.html
