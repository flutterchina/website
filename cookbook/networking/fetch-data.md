---
layout: page
title: "从互联网上获取数据"
permalink: /cookbook/networking/fetch-data/
summary: Flutter教程-如何从网上获取数据，并将其转为Dart对象。
keywords: Flutter网络请求
---

从大多数应用程序都需要从互联网上获取数据，Dart和Flutter提供了相关工具！

> 注：本篇示例官方使用的是用`http` package发起简单的网络请求，但是`http` package功能较弱，很多常用功能都不支持。我们建议您使用[dio](https://github.com/flutterchina/dio) 来发起网络请求，它是一个强大易用的dart http请求库，支持Restful API、FormData、拦截器、请求取消、Cookie管理、文件上传/下载…...详情请查看[github dio](https://github.com/flutterchina/dio) .

## 步骤

    1. 添加`http` package依赖
    2. 使用`http` package发出网络请求
    3. 将响应转为自定义的Dart对象
    4. 获取并显示数据

## 1. 添加`http` package

[`http`](https://pub.dartlang.org/packages/http) package提供了从互联网获取数据的最简单方法。

```
dependencies:
  http: <latest_version>
```

## 2. 发起网络请求

在这个例子中，我们将使用[`http.get`](https://docs.flutter.io/flutter/package-http_http/package-http_http-library.html) 从[JSONPlaceholder REST API](https://jsonplaceholder.typicode.com/)中获取示例文章。

```dart
Future<http.Response> fetchPost() {
  return http.get('https://jsonplaceholder.typicode.com/posts/1');
}
```

`http.get`方法返回一个包含一个`Response`的`Future`。

  * [`Future`](https://docs.flutter.io/flutter/dart-async/Future-class.html)是与异步操作一起工作的核心Dart类。它用于表示未来某个时间可能会出现的可用值或错误。
  * `http.Response`类包含一个成功的HTTP请求接收到的数据

## 3. 将响应转换为自定义Dart对象

虽然发出网络请求很简单，但如果要使用原始的`Future<http.Response>`并不简单。为了让我们可以开开心心的写代码，我们可以将`http.Response`转换成我们自己的Dart对象。

### 创建一个`Post`类

首先，我们需要创建一个`Post`类，它包含我们网络请求的数据。它还将包括一个工厂构造函数，它允许我们可以通过json创建一个`Post`对象。

手动转换JSON只是一种选择。有关更多信息，请参阅关于[JSON和序列化](/json)的完整文章。

```dart
class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({this.userId, this.id, this.title, this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return new Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}
```

### 将`http.Response` 转换成一个`Post`对象

现在，我们将更新`fetchPost`函数以返回一个`Future<Post>`。为此，我们需要：

  1. 使用`dart:convert` package将响应内容转化为一个json `Map`。
  2. 使用`fromJson`工厂函数，将json `Map` 转化为一个`Post`对象。

```dart
Future<Post> fetchPost() async {
  final response = await http.get('https://jsonplaceholder.typicode.com/posts/1');
  final json = JSON.decode(response.body); 
  
  return new Post.fromJson(json); 
}
```

Hooray! 现在我们有一个函数，我们可以调用它从互联网上获取一篇文章！

## 4. 获取并显示数据

为了获取数据并将其显示在屏幕上，我们可以使用[`FutureBuilder`](https://docs.flutter.io/flutter/widgets/FutureBuilder-class.html) widget！`FutureBuilder` Widget可以很容易与异步数据源一起工作。

我们需要提供两个参数：

1. `future`参数是一个异步的网络请求，在这个例子中，我们传递调用`fetchPost()`函数的返回值(一个future)。

2. 一个 `builder` 函数，这告诉Flutter在`future`执行的不同阶段应该如何渲染界面。

   ```dart
   new FutureBuilder<Post>(
     future: fetchPost(),
     builder: (context, snapshot) {
       if (snapshot.hasData) {
         return new Text(snapshot.data.title);
       } else if (snapshot.hasError) {
         return new Text("${snapshot.error}");
       }
   
       // By default, show a loading spinner
       return new CircularProgressIndicator();
     },
   );
   ```

   译者注：其中`snapshot.data`即为Future的结果。

## 完整的例子

```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<Post> fetchPost() async {
  final response =
      await http.get('https://jsonplaceholder.typicode.com/posts/1');
  final responseJson = json.decode(response.body);

  return new Post.fromJson(responseJson);
}

class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({this.userId, this.id, this.title, this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return new Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Fetch Data Example',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Fetch Data Example'),
        ),
        body: new Center(
          child: new FutureBuilder<Post>(
            future: fetchPost(),
            builder: (context, snapshot) {
              if (snapshot.hasData) {
                return new Text(snapshot.data.title);
              } else if (snapshot.hasError) {
                return new Text("${snapshot.error}");
              }

              // By default, show a loading spinner
              return new CircularProgressIndicator();
            },
          ),
        ),
      ),
    );
  }
}
```
