---
layout: page
title: "进行认证请求"
permalink: /cookbook/networking/authenticated-requests/
summary: 为了从更多的Web服务获取数据，有些时候您需要提供授权。有很多方法可以做到这一点，但最常见的可能是使用HTTP header中的Authorization。
keywords: Flutter网络请求
---

为了从更多的Web服务获取数据，有些时候您需要提供授权。有很多方法可以做到这一点，但最常见的可能是使用HTTP header中的`Authorization`。

> 注：本篇示例官方使用的是用`http` package发起简单的网络请求，但是`http` package功能较弱，很多功能都不支持。我们建议您使用[dio](https://github.com/flutterchina/dio) 来发起网络请求，它是一个强大易用的dart http请求库，支持Restful API、FormData、拦截器、请求取消、Cookie管理、文件上传/下载…...详情请查看[github dio](https://github.com/flutterchina/dio) .

## 添加 Authorization Headers

[`http`](https://pub.dartlang.org/packages/http) package提供了一种方便的方法来为请求添加headers。您也可以使用`dart:io`package来添加。

```dart
Future<http.Response> fetchPost() {
  return http.get(
    'https://jsonplaceholder.typicode.com/posts/1',
    // Send authorization headers to your backend
    headers: {HttpHeaders.AUTHORIZATION: "Basic your_api_token_here"},
  );
}
```

## 完整的例子

这个例子建立在[从互联网上获取数据](/cookbook/networking/fetch-data/)之上 。

```dart
import 'dart:async';
import 'dart:convert';
import 'dart:io';
import 'package:http/http.dart' as http;

Future<Post> fetchPost() async {
  final response = await http.get(
    'https://jsonplaceholder.typicode.com/posts/1',
    headers: {HttpHeaders.AUTHORIZATION: "Basic your_api_token_here"},
  );
  final json = JSON.decode(response.body); 
  
  return new Post.fromJson(json); 
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
```