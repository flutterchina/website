---
layout: page
title: 读写文件
permalink: /reading-writing-files/
summary: 本文介绍了在Flutter中如何读写文件
keywords: Flutter文件读写, Flutter文件操作, Flutter读写文件, Flutter文件操作
---

本指南介绍如何使用[`PathProvider`](https://pub.dartlang.org/packages/path_provider) 插件和Dart的[`IO`](https://api.dartlang.org/stable/dart-io/dart-io-library.html)库在Flutter中读写文件 。

* TOC
{:toc}

## 介绍

[`PathProvider`](https://pub.dartlang.org/packages/path_provider) 插件提供了一种平台透明的方式来访问设备文件系统上的常用位置。该类当前支持访问两个文件系统位置：

+ **临时目录:** 系统可随时清除的临时目录（缓存）。在iOS上，这对应于[`NSTemporaryDirectory()`](https://developer.apple.com/reference/foundation/1409211-nstemporarydirectory) 返回的值。在Android上，这是[`getCacheDir()`](https://developer.android.com/reference/android/content/Context.html#getCacheDir())返回的值。
+ **文档目录:** 应用程序的目录，用于存储只有自己可以访问的文件。只有当应用程序被卸载时，系统才会清除该目录。在iOS上，这对应于`NSDocumentDirectory`。在Android上，这是`AppData`目录。

一旦你的Flutter应用程序有一个文件位置的引用，你可以使用[dart:io](https://api.dartlang.org/stable/dart-io/dart-io-library.html)API来执行对文件系统的读/写操作。有关使用Dart处理文件和目录的更多信息，请参阅此[概述](https://www.dartlang.org/articles/dart-vm/io) 和这些[示例](https://www.dartlang.org/dart-vm/dart-by-example#files-directories-and-symlinks)。

## 读写文件的示例


以下示例展示了如何统计应用程序中按钮被点击的次数(关闭重启数据不丢失)：

1. 通过 `flutter create` 或在IntelliJ中 File > New Project 创建一个新Flutter App.

1. 在`pubspec.yaml`文件中[声明依赖](https://pub.dartlang.org/packages/path_provider#-installing-tab-)  PathProvider 插件

1. 用以下代码替换 `lib/main.dart`中的:

```dart
import 'dart:io';
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';

void main() {
  runApp(
    new MaterialApp(
      title: 'Flutter Demo',
      theme: new ThemeData(primarySwatch: Colors.blue),
      home: new FlutterDemo(),
    ),
  );
}

class FlutterDemo extends StatefulWidget {
  FlutterDemo({Key key}) : super(key: key);

  @override
  _FlutterDemoState createState() => new _FlutterDemoState();
}

class _FlutterDemoState extends State<FlutterDemo> {
  int _counter;

  @override
  void initState() {
    super.initState();
    _readCounter().then((int value) {
      setState(() {
        _counter = value;
      });
    });
  }

  Future<File> _getLocalFile() async {
    // get the path to the document directory.
    String dir = (await getApplicationDocumentsDirectory()).path;
    return new File('$dir/counter.txt');
  }

  Future<int> _readCounter() async {
    try {
      File file = await _getLocalFile();
      // read the variable as a string from the file.
      String contents = await file.readAsString();
      return int.parse(contents);
    } on FileSystemException {
      return 0;
    }
  }

  Future<Null> _incrementCounter() async {
    setState(() {
      _counter++;
    });
    // write the variable as a string to the file
    await (await _getLocalFile()).writeAsString('$_counter');
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(title: new Text('Flutter Demo')),
      body: new Center(
        child: new Text('Button tapped $_counter time${
          _counter == 1 ? '' : 's'
        }.'),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: new Icon(Icons.add),
      ),
    );
  }
}
```
