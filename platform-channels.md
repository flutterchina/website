---
layout: page
title: 使用平台通道编写平台特定的代码
permalink: /platform-channels/
summary: 本文介绍在Flutter中如何与原生系统通信、调用彼此的接口，并同时介绍了Flutter的插件系统。
keywords: Flutter插件
---
> 译者语：所谓“平台特定”或“特定平台”，平台指的就是原生Android或IOS，本文主要讲原生和Flutter之间如何通信、如何进行功能互调。

本指南介绍如何编写自定义平台特定的代码。一些平台特定的功能可通过现有软件包获得; 请参阅[使用 packages](/using-packages/)。

* TOC
{:toc}

Flutter使用了一个灵活的系统，允许您调用特定平台的API，无论在Android上的Java或Kotlin代码中，还是iOS上的ObjectiveC或Swift代码中均可用。

Flutter平台特定的API支持不依赖于代码生成，而是依赖于灵活的消息传递的方式：

* 应用的Flutter部分通过平台通道（platform channel）将消息发送到其应用程序的所在的宿主（iOS或Android）。

* 宿主监听的平台通道，并接收该消息。然后它会调用特定于该平台的API（使用原生编程语言） - 并将响应发送回客户端，即应用程序的Flutter部分。

## 框架概述: 平台通道 {#architecture}

使用平台通道在客户端（Flutter UI）和宿主（平台）之间传递消息，如下图所示：

![Platform channels architecture](/images/PlatformChannels.png)

消息和响应是异步传递的，以确保用户界面保持响应(不会挂起)。

在客户端，`MethodChannel` ([API][MethodChannel])可以发送与方法调用相对应的消息。
在宿主平台上，`MethodChannel` 在Android（([API][MethodChannelAndroid]) 和 FlutterMethodChannel iOS ([API][MethodChanneliOS])
可以接收方法调用并返回结果。这些类允许您用很少的“脚手架”代码开发平台插件。

<div class="alert alert-info" markdown="1">
**注意**: 如果需要，方法调用也可以反向发送，宿主作为客户端调用Dart中实现的API。
这个[`quick_actions`](https://pub.dartlang.org/packages/quick_actions)插件就是一个具体的例子
</div>

[MethodChannel]: https://docs.flutter.io/flutter/services/MethodChannel-class.html
[MethodChannelAndroid]: https://docs.flutter.io/javadoc/io/flutter/plugin/common/MethodChannel.html
[MethodChanneliOS]: https://docs.flutter.io/objcdoc/Classes/FlutterMethodChannel.html

### 平台通道数据类型支持和解码器 {#codec}

标准平台通道使用标准消息编解码器，以支持简单的类似JSON值的高效二进制序列化，例如
booleans,numbers, Strings, byte buffers, List, Maps（请参阅[`StandardMessageCodec`](https://docs.flutter.io/flutter/services/StandardMessageCodec-class.html)了解详细信息）。
当您发送和接收值时，这些值在消息中的序列化和反序列化会自动进行。

下表显示了如何在宿主上接收Dart值，反之亦然：

| Dart        | Android             | iOS                        
|-------------|---------------------|----
| null        | null                | nil (NSNull when nested)
| bool        | java.lang.Boolean   | NSNumber numberWithBool:
| int         | java.lang.Integer   | NSNumber numberWithInt:
| int, if 32 bits not enough | java.lang.Long       | NSNumber numberWithLong:
| int, if 64 bits not enough | java.math.BigInteger | FlutterStandardBigInteger
| double      | java.lang.Double    | NSNumber numberWithDouble:
| String      | java.lang.String    | NSString
| Uint8List   | byte[]   | FlutterStandardTypedData typedDataWithBytes:
| Int32List   | int[]    | FlutterStandardTypedData typedDataWithInt32:
| Int64List   | long[]   | FlutterStandardTypedData typedDataWithInt64:
| Float64List | double[] | FlutterStandardTypedData typedDataWithFloat64:
| List        | java.util.ArrayList | NSArray
| Map         | java.util.HashMap   | NSDictionary

<br>
## 示例: 使用平台通道调用iOS和Android代码 {#example}

以下演示如何调用平台特定的API来获取和显示当前的电池电量。它通过一个平台消息`getBatteryLevel` 调用Android `BatteryManager` API和iOS `device.batteryLevel` API。  。

该示例在应用程序内添加了特定于平台的代码。如果您想开发一个通用的平台包，可以在其它应用中也使用的话，你需要开发一个插件，
则项目创建步骤稍有不同（请参阅[开发 packages](/developing-packages/#plugin)），但平台通道代码仍以相同方式编写。

<div class="alert alert-info" markdown="1">
**注意**: 此示例的完整的可运行源代码位于：[`/examples/platform_channel/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel)，
这个示例Android是用的Java, IOS用的是Objective-C，IOS Swift版本请参阅 [`/examples/platform_channel_swift/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel_swift)
</div>

### Step 1: 创建一个新的应用程序项目 {#example-project}

首先创建一个新的应用程序:

* 在终端运行中：`flutter create batterylevel`

默认情况下，模板支持使用Java编写Android代码，或使用Objective-C编写iOS代码。要使用Kotlin或Swift，请使用-i和/或-a标志:

* 在终端中运行: `flutter create -i swift -a kotlin batterylevel`

### Step 2: 创建Flutter平台客户端 {#example-client}

该应用的`State`类拥有当前的应用状态。我们需要延长这一点以保持当前的电量

首先，我们构建通道。我们使用`MethodChannel`调用一个方法来返回电池电量。

通道的客户端和宿主通过通道构造函数中传递的通道名称进行连接。单个应用中使用的所有通道名称必须是唯一的;
我们建议在通道名称前加一个唯一的“域名前缀”，例如`samples.flutter.io/battery`。

<!-- skip -->
```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
...
class _MyHomePageState extends State<MyHomePage> {
  static const platform = const MethodChannel('samples.flutter.io/battery');

  // Get battery level.
}
```

接下来，我们调用通道上的方法，指定通过字符串标识符调用方法`getBatteryLevel`。
该调用可能失败 - 例如，如果平台不支持平台API（例如在模拟器中运行时），所以我们将invokeMethod调用包装在try-catch语句中。

我们使用返回的结果，在`setState`中来更新用户界面状态`batteryLevel`。

<!-- skip -->
```dart
  // Get battery level.
  String _batteryLevel = 'Unknown battery level.';

  Future<Null> _getBatteryLevel() async {
    String batteryLevel;
    try {
      final int result = await platform.invokeMethod('getBatteryLevel');
      batteryLevel = 'Battery level at $result % .';
    } on PlatformException catch (e) {
      batteryLevel = "Failed to get battery level: '${e.message}'.";
    }

    setState(() {
      _batteryLevel = batteryLevel;
    });
  }
```

最后，我们在build创建包含一个小字体显示电池状态和一个用于刷新值的按钮的用户界面。

<!-- skip -->
```dart
@override
Widget build(BuildContext context) {
  return new Material(
    child: new Center(
      child: new Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          new RaisedButton(
            child: new Text('Get Battery Level'),
            onPressed: _getBatteryLevel,
          ),
          new Text(_batteryLevel),
        ],
      ),
    ),
  );
}
```


### Step 3a: 使用Java添加Android平台特定的实现 {#example-java}

*注意*: 以下步骤使用Java。如果您更喜欢Kotlin，请跳到步骤3b.

首先在Android Studio中打开您的Flutter应用的Android部分：

1. 启动 Android Studio

1. 选择 'File > Open...'

1. 定位到您 Flutter app目录, 然后选择里面的 `android`文件夹，点击 OK

1. 在`java`目录下打开 `MainActivity.java`

接下来，在`onCreate`里创建MethodChannel并设置一个`MethodCallHandler`。确保使用与在Flutter客户端使用的通道名称相同。

```java
import io.flutter.app.FlutterActivity;
import io.flutter.plugin.common.MethodCall;
import io.flutter.plugin.common.MethodChannel;
import io.flutter.plugin.common.MethodChannel.MethodCallHandler;
import io.flutter.plugin.common.MethodChannel.Result;

public class MainActivity extends FlutterActivity {
    private static final String CHANNEL = "samples.flutter.io/battery";

    @Override
    public void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);

        new MethodChannel(getFlutterView(), CHANNEL).setMethodCallHandler(
                new MethodCallHandler() {
                    @Override
                    public void onMethodCall(MethodCall call, Result result) {
                        // TODO
                    }
                });
    }
}
```

接下来，我们添加Java代码，使用Android电池API来获取电池电量。此代码与您在原生Android应用中编写的代码完全相同。

首先，添加需要导入的依赖。

```
import android.content.ContextWrapper;
import android.content.Intent;
import android.content.IntentFilter;
import android.os.BatteryManager;
import android.os.Build.VERSION;
import android.os.Build.VERSION_CODES;
import android.os.Bundle;
```

然后，将下面的新方法添加到activity类中的，位于onCreate 方法下方：

```java
private int getBatteryLevel() {
  int batteryLevel = -1;
  if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
    BatteryManager batteryManager = (BatteryManager) getSystemService(BATTERY_SERVICE);
    batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY);
  } else {
    Intent intent = new ContextWrapper(getApplicationContext()).
        registerReceiver(null, new IntentFilter(Intent.ACTION_BATTERY_CHANGED));
    batteryLevel = (intent.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100) /
        intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1);
  }

  return batteryLevel;
}
```

最后，我们完成之前添加的`onMethodCall`方法。我们需要处理平台方法名为`getBatteryLevel`，所以我们在call参数中进行检测是否为`getBatteryLevel`。
这个平台方法的实现只需调用我们在前一步中编写的Android代码，并使用response参数返回成功和错误情况的响应。如果调用未知的方法，我们也会通知返回：

```java
@Override
public void onMethodCall(MethodCall call, Result result) {
    if (call.method.equals("getBatteryLevel")) {
        int batteryLevel = getBatteryLevel();

        if (batteryLevel != -1) {
            result.success(batteryLevel);
        } else {
            result.error("UNAVAILABLE", "Battery level not available.", null);
        }
    } else {
        result.notImplemented();
    }
}               
```

您现就可以在Android上运行该应用程序。如果您使用的是Android模拟器，则可以通过工具栏中的`...`按钮访问Extended Controls面板中的电池电量

### Step 3b: 使用Kotlin添加Android平台特定的实现 {#example-kotlin}

*注意*: 以下步骤与步骤3a类似，只是使用Kotlin而不是Java。

此步骤假定您在[step 1.](#example-project)中 使用该`-a kotlin`选项创建了项目

首先在Android Studio中打开您的Flutter应用的Android部分

1. 启动 Android Studio

1. 选择 the menu item 'File > Open...'

1. 定位到您 Flutter app目录, 然后选择里面的 `android`文件夹，点击 OK

1. 在`kotlin`目录中打开`MainActivity.kt`. (注意：如果您使用Android Studio 2.3进行编辑，请注意'kotlin'文件夹将显示为'java'。)

接下来，在`onCreate`里创建MethodChannel并设置一个`MethodCallHandler`。确保使用与在Flutter客户端使用的通道名称相同。

```kotlin
import android.os.Bundle
import io.flutter.app.FlutterActivity
import io.flutter.plugin.common.MethodChannel
import io.flutter.plugins.GeneratedPluginRegistrant

class MainActivity() : FlutterActivity() {
  private val CHANNEL = "samples.flutter.io/battery"

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    GeneratedPluginRegistrant.registerWith(this)

    MethodChannel(flutterView, CHANNEL).setMethodCallHandler { call, result ->
      // TODO
    }
  }
}
```

接下来，我们添加Kotlin代码，使用Android电池API来获取电池电量。此代码与您在原生Android应用中编写的代码完全相同。

首先，添加需要导入的依赖。

```
import android.content.Context
import android.content.ContextWrapper
import android.content.Intent
import android.content.IntentFilter
import android.os.BatteryManager
import android.os.Build.VERSION
import android.os.Build.VERSION_CODES
```

然后，将下面的新方法添加到activity类中的，位于onCreate 方法下方：

```kotlin
  private fun getBatteryLevel(): Int {
    val batteryLevel: Int
    if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
      val batteryManager = getSystemService(Context.BATTERY_SERVICE) as BatteryManager
      batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY)
    } else {
      val intent = ContextWrapper(applicationContext).registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
      batteryLevel = intent!!.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100 / intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1)
    }

    return batteryLevel
  }
```

最后，我们完成之前添加的`onMethodCall`方法。我们需要处理平台方法名为`getBatteryLevel`，所以我们在call参数中进行检测是否为`getBatteryLevel`。
这个平台方法的实现只需调用我们在前一步中编写的Android代码，并使用response参数返回成功和错误情况的响应。如果调用未知的方法，我们也会通知返回：

```kotlin
    MethodChannel(flutterView, CHANNEL).setMethodCallHandler { call, result ->
      if (call.method == "getBatteryLevel") {
        val batteryLevel = getBatteryLevel()

        if (batteryLevel != -1) {
          result.success(batteryLevel)
        } else {
          result.error("UNAVAILABLE", "Battery level not available.", null)
        }
      } else {
        result.notImplemented()
      }
    }
```

您现就可以在Android上运行该应用程序。如果您使用的是Android模拟器，则可以通过工具栏中的`...`按钮访问Extended Controls面板中的电池电量

### Step 4a: 使用Objective-C添加iOS平台特定的实现 {#example-objc}

*注意*: 以下步骤使用Objective-C。如果您喜欢Swift，请跳到步骤4b

首先打开Xcode中Flutter应用程序的iOS部分:

1. 启动 Xcode

1. 选择 'File > Open...'

1. 定位到您 Flutter app目录, 然后选择里面的 `iOS`文件夹，点击 OK

1. 确保Xcode项目的构建没有错误。

1. 选择 Runner > Runner ，打开`AppDelegate.m


接下来，在`application didFinishLaunchingWithOptions:`方法内部创建一个`FlutterMethodChannel`，并添加一个处理方法。
确保与在Flutter客户端使用的通道名称相同。

```objectivec
#import <Flutter/Flutter.h>

@implementation AppDelegate
- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {
  FlutterViewController* controller = (FlutterViewController*)self.window.rootViewController;

  FlutterMethodChannel* batteryChannel = [FlutterMethodChannel
                                          methodChannelWithName:@"samples.flutter.io/battery"
                                          binaryMessenger:controller];

  [batteryChannel setMethodCallHandler:^(FlutterMethodCall* call, FlutterResult result) {
    // TODO
  }];

  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}
```

接下来，我们添加ObjectiveC代码，使用iOS电池API来获取电池电量。此代码与您在本机iOS应用程序中编写的代码完全相同。

在`AppDelegate`类中添加以下新的方法：

```objectivec
- (int)getBatteryLevel {
  UIDevice* device = UIDevice.currentDevice;
  device.batteryMonitoringEnabled = YES;
  if (device.batteryState == UIDeviceBatteryStateUnknown) {
    return -1;
  } else {
    return (int)(device.batteryLevel * 100);
  }
}
```

最后，我们完成之前添加的`setMethodCallHandler`方法。我们需要处理的平台方法名为`getBatteryLevel`，所以我们在call参数中进行检测是否为`getBatteryLevel`。
这个平台方法的实现只需调用我们在前一步中编写的IOS代码，并使用response参数返回成功和错误情况的响应。如果调用未知的方法，我们也会通知返回：

```objectivec
[batteryChannel setMethodCallHandler:^(FlutterMethodCall* call, FlutterResult result) {
  if ([@"getBatteryLevel" isEqualToString:call.method]) {
    int batteryLevel = [self getBatteryLevel];

    if (batteryLevel == -1) {
      result([FlutterError errorWithCode:@"UNAVAILABLE"
                                 message:@"Battery info unavailable"
                                 details:nil]);
    } else {
      result(@(batteryLevel));
    }
  } else {
    result(FlutterMethodNotImplemented);
  }
}];
```

您现在可以在iOS上运行应用程序。如果您使用的是iOS模拟器，请注意，它不支持电池API，因此应用程序将显示“电池信息不可用”。

### Step 4b: 使用Swift添加一个iOS平台的实现 {#example-swift}

*注意*: 以下步骤与步骤4a类似，只不过是使用Swift而不是Objective-C.

此步骤假定您在步骤1中 使用`-i swift`选项创建了项目。

首先打开Xcode中Flutter应用程序的iOS部分:

1. 启动 Xcode

1. 选择 'File > Open...'

1. 定位到您 Flutter app目录, 然后选择里面的 `android`文件夹，点击 OK

1. 确保Xcode项目的构建没有错误。

1. 选择 Runner > Runner ，然后打开`AppDelegate.swift`

接下来，覆盖application方法并创建一个`FlutterMethodChannel`绑定通道名称`samples.flutter.io/battery`：

```swift
@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    GeneratedPluginRegistrant.register(with: self);

    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController;
    let batteryChannel = FlutterMethodChannel.init(name: "samples.flutter.io/battery",
                                                   binaryMessenger: controller);
    batteryChannel.setMethodCallHandler({
      (call: FlutterMethodCall, result: FlutterResult) -> Void in
      // Handle battery messages.
    });

    return super.application(application, didFinishLaunchingWithOptions: launchOptions);
  }
}
```

接下来，我们添加Swift代码，使用iOS电池API来获取电池电量。此代码与您在本机iOS应用程序中编写的代码完全相同。

将以下新方法天骄到`AppDelegate.swift`底部

```swift
private func receiveBatteryLevel(result: FlutterResult) {
  let device = UIDevice.current;
  device.isBatteryMonitoringEnabled = true;
  if (device.batteryState == UIDeviceBatteryState.unknown) {
    result(FlutterError.init(code: "UNAVAILABLE",
                             message: "Battery info unavailable",
                             details: nil));
  } else {
    result(Int(device.batteryLevel * 100));
  }
}
```

最后，我们完成之前添加的`setMethodCallHandler`方法。我们需要处理的平台方法名为`getBatteryLevel`，所以我们在call参数中进行检测是否为`getBatteryLevel`。
这个平台方法的实现只需调用我们在前一步中编写的IOS代码，并使用response参数返回成功和错误情况的响应。如果调用未知的方法，我们也会通知返回：

```swift
batteryChannel.setMethodCallHandler({
  (call: FlutterMethodCall, result: FlutterResult) -> Void in
  if ("getBatteryLevel" == call.method) {
    receiveBatteryLevel(result: result);
  } else {
    result(FlutterMethodNotImplemented);
  }
});
```

您现在可以在iOS上运行应用程序。如果您使用的是iOS模拟器，请注意，它不支持电池API，因此应用程序将显示“电池信息不可用”。

## 从UI代码中分离平台特定的代码 {#separate}

如果您希望在多个Flutter应用程序中使用特定于平台的代码，将代码分离为位于主应用程序之外的目录中，做一个平台插件会很有用。详情请参阅[开发 packages](/developing-packages/) 。

## 将平台特定的代码作为一个包发布 {#publish}

如果您希望与Flutter生态系统中的其他开发人员分享您的特定平台的代码，请参阅发[发布 packages](/developing-packages/#publish以了解详细信息。

## 自定义平台通道和编解码器

除了上面提到的`MethodChannel`，你还可以使用[`BasicMessageChannel`][BasicMessageChannel]，它支持使用自定义消息编解码器进行基本的异步消息传递。
此外，您可以使用专门的[`BinaryCodec`][BinaryCodec]，[`StringCodec`][StringCodec]和 [`JSONMessageCodec`][JSONMessageCodec]类，或创建自己的编解码器。

[BasicMessageChannel]: https://docs.flutter.io/flutter/services/BasicMessageChannel-class.html
[BinaryCodec]: https://docs.flutter.io/flutter/services/BinaryCodec-class.html
[StringCodec]: https://docs.flutter.io/flutter/services/StringCodec-class.html
[JSONMessageCodec]: https://docs.flutter.io/flutter/services/JSONMessageCodec-class.html


