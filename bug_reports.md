---
layout: page
title: 创建Bug Reports

permalink: /bug-reports/
---

* TOC Placeholder
{:toc}

## 介绍

本文档中的说明详细介绍了为崩溃和其他不正常的行为提供最具操作性的bug reports所需的步骤。
每一步都是可选的，但会大大提高问题诊断和解决的速度。我们感谢您向我们发送尽可能多的反馈。

## 在Github上创建一个Issue
* 创建地址：[https://github.com/flutter/flutter/issues/new](https://github.com/flutter/flutter/issues/new)

## 提供一些 Flutter 的诊断信息
* 运行 `flutter doctor` 然后粘贴输出结果到Github Issue:

```
[✓] Flutter (on Mac OS, channel master)
    • Flutter at /Users/me/projects/flutter
    • Framework revision 8cbeb2e (4 hours ago), engine revision 5c28578

[✓] Android toolchain - develop for Android devices (Android SDK 23.0.2)
    • Android SDK at /usr/local/Cellar/android-sdk/24.4.1_1
    • Platform android-23, build-tools 23.0.2
    • Java(TM) SE Runtime Environment (build 1.8.0_73-b02)

[✓] iOS toolchain - develop for iOS devices (Xcode 7.3)
    • XCode at /Applications/Xcode.app/Contents/Developer
    • Xcode 7.3, Build version 7D175

[✓] IntelliJ IDEA Ultimate Edition (version 2016.2.5)
    • Dart plugin installed
    • Flutter plugin installed
```

## 以详细模式运行命令
仅当您的问题与`flutter`**工具**相关时，请遵循这些步骤。

* 所有Flutter命令都接受`--verbose`参数。将结果附加到issue中，可能有助于诊断问题。
* 将命令的结果附加到Github issue.
![flutter verbose](/images/verbose_flag.png)

## 提供最近的日志
* `flutter logs` 可以通过`flutter logs`访问当前连接的设备的日志
* 如果崩溃是可重现的，请清除日志（Mac上的⌘+ k），重现崩溃并将新生成的日志复制错误报告的文件中
* 如果您收到框架抛出的异常，请包括第一个此类异常虚线之间的所有输出
![flutter logs](/images/logs.png)


## 提供崩溃报告
* 如果iOS模拟器崩溃，则会生成一个崩溃报告，在 `~/Library/Logs/DiagnosticReports/`.
* 如果iOS设备崩溃，则会生成一个崩溃报告，在 `~/Library/Logs/CrashReporter/MobileDevice`.
* 找到与崩溃相对应的报告（通常是最新的文件）并将其附加到Github issue中
![crash report](/images/crash_reports.png)
