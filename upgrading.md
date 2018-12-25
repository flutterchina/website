---
layout: page
title: 升级 Flutter
permalink: /upgrading/
summary: 更新升级Flutter
keywords: 升级Flutter
---

我们强烈建议跟踪flutter的`stable`分支，这是Flutter稳定分支。
如果你需要查看最新的变化，你可以跟踪`master`分支，但注意这是开发分支，所以稳定性要低得多。

要查看您当前使用的分支，请运行`flutter channel`查看。

要切换分支，请使用`flutter channel beta` 或 `flutter channel master`

## 为您的项目指定Flutter SDK

您可以在`pubspec.yaml`文件中指定Flutter SDK的依赖项。例如，以下代码片段指定`flutter`和`flutter_test`包使用Flutter SDK。

```yaml
name: hello_world
dependencies:
  flutter:
    sdk: flutter
dev_dependencies:
  flutter_test:
    sdk: flutter
```

不要使用`pub get`或`pub upgrade`命令来管理你的依赖关系。相反，应该使用`flutter packages get`或`flutter packages upgrade`。如果您想手动使用pub，则可以通过设置`FLUTTER_ROOT`环境变量来直接运行它。

## 升级 Flutter channel 和 packages

要同时更新Flutter SDK和你的依赖包，在你的应用程序根目录（包含`pubspec.yaml`文件的目录）中运行`flutter upgrade` 命令:

```shell
$ flutter upgrade
```

## 升级你的依赖包

如果您修改了`pubspec.yaml`文件，或者只想更新应用依赖的包(不包括Flutter SDK)，请使用以下命令：
* `flutter packages get`获取`pubspec.yaml`文件中列出的所有依赖包
* `flutter packages upgrade` 获取`pubspec.yaml`文件中列出的所有依赖包的最新版本

我们向我们的[邮件列表](https://groups.google.com/forum/#!forum/flutter-dev)发布重大更新公告 。
我们强烈建议您订阅我们的通知。另外，我们很乐意听取您的意见！
