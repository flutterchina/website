---
layout: page
title: 发布Android版APP
permalink: /android-release/
summary: 本文详细的介绍了如何构建Flutter Android发布包以及如何发布到应用市场
keywords: 发布Flutter应用
---

在典型的开发周期中，您将使用`flutter run`命令行或者IntelliJ中通过工具栏运行和调试按钮进行测试。默认情况下，Flutter构建应用程序的**debug**版本。

当您准备好为Android准备的**release**版时，例如要发布到应用商店，请按照此页面上的步骤操作。

* TOC Placeholder
{:toc}

## 检查 App Manifest


查看默认[应用程序清单][manifest]文件(位于`<app dir>/android/app/src/main/`中的`AndroidManifest.xml`文件)，并验证这些值是否正确，特别是：

* `application`: 编辑 [`application`][applicationtag] 标签， 这是应用的名称。

* `uses-permission`: 如果您的应用程序代码不需要Internet访问，请删除`android.permission.INTERNET`权限。标准模板包含此标记是为了启用Flutter工具和正在运行的应用程序之间的通信。

## 查看构建配置
查看默认[Gradle 构建文件][gradlebuild]"build.gradle"，它位于`<app dir>/android/app/`，验证这些值是否正确，尤其是：

* `defaultConfig`: 

  * `applicationId`: 指定始终唯一的 (Application Id)[appid]

  * `versionCode` & `versionName`: 指定应用程序版本号和版本号字符串。有关详细信息，请参考[版本文档][versions]

  * `minSdkVersion` & `targetSdkVersion`: 指定最低的API级别以及应用程序设计运行的API级别。有关详细信息，请参阅[版本文档][versions]中的API级别部分。

## 添加启动图标

当一个新的Flutter应用程序被创建时，它有一个默认的启动器图标。要自定义此图标：

1. 查看[Android启动图标][launchericons] 设计指南，然后创建图标。

1. 在`<app dir>/android/app/src/main/res/`目录中，将图标文件放入使用[配置限定符][configurationqualifiers]命名的文件夹中。默认`mipmap-`文件夹演示正确的命名约定。

1. 在`AndroidManifest.xml`中，将[`application`][applicationtag]标记的`android:icon`属性更新为引用上一步中的图标（例如  `<application android:icon="@mipmap/ic_launcher" ...`）。

1. 要验证图标是否已被替换，请运行您的应用程序并检查应用图标

## app签名

### 创建 keystore

如果您有现有keystore，请跳至下一步。如果没有，请通过在运行以下命令来创建一个：` keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key`

**注意**：保持文件私密; 不要将它加入到公共源代码控制中。

**注意**: `keytool`可能不在你的系统路径中。它是Java JDK的一部分，它是作为Android Studio的一部分安装的。有关具体路径，请百度。

### 引用应用程序中的keystore


创建一个名为`<app dir>/android/key.properties`的文件，其中包含对密钥库的引用：

```groovy
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<location of the key store file, e.g. /Users/<user name>/key.jks>
```

**注意**: 保持文件私密; 不要将它加入公共源代码控制中

### 在gradle中配置签名


通过编辑`<app dir>/android/app/build.gradle`文件为您的应用配置签名

1. 替换:
```groovy
   android {
```
  为：
```groovy
   def keystorePropertiesFile = rootProject.file("key.properties")
   def keystoreProperties = new Properties()
   keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

   android {
```

1. 替换:
```groovy
   buildTypes {
       release {
           // TODO: Add your own signing config for the release build.
           // Signing with the debug keys for now, so `flutter run --release` works.
           signingConfig signingConfigs.debug
       }
   }
```
   为:
```groovy
   signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile file(keystoreProperties['storeFile'])
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
```
现在，您的应用的release版本将自动进行签名。

## 开启混淆

默认情况下 flutter 不会开启 Android 的混淆。

如果使用了第三方 Java 或 Android 库，也许你想减小 apk 文件的大小或者防止代码被逆向破解。

### 配置混淆

创建 `/android/app/proguard-rules.pro` 文件，并添加以下规则：

```
#Flutter Wrapper
-keep class io.flutter.app.** { *; }
-keep class io.flutter.plugin.**  { *; }
-keep class io.flutter.util.**  { *; }
-keep class io.flutter.view.**  { *; }
-keep class io.flutter.**  { *; }
-keep class io.flutter.plugins.**  { *; }
```

上述配置只混淆了 Flutter 引擎库，任何其他库（比如 Firebase）需要添加与之对应的规则。

### 开启混淆/压缩

打开 `/android/app/build.gradle` 文件，定位到 `buildTypes` 块。

在 `release ` 配置中将 `minifyEnabled ` 和 `useProguard ` 设为 `true`，再将混淆文件指向上一步创建的文件。

```
android {

    ...

    buildTypes {

        release {

            signingConfig signingConfigs.release

            minifyEnabled true
            useProguard true

            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

        }
    }
}
```

## 构建一个发布版（release）APK

本节介绍如何构建发布版（release）APK。如果您完成了前一节中的签名步骤，则会对APK进行签名。

使用命令行:

1. `cd <app dir>` (`<app dir>` 为您的工程目录).
1. 运行`flutter build apk` (`flutter build` 默认会包含 `--release`选项).

打包好的发布APK位于`<app dir>/build/app/outputs/apk/app-release.apk`。

## 在设备上安装发行版APK

按照以下步骤在已连接的Android设备上安装上一步中构建的APK

使用命令行:

1. 用USB您的Android设备连接到您的电脑
1. `cd <app dir>` .
1. 运行 `flutter install` .

## 将APK发布到Google Play商店

将应用的release版发布到Google Play商店的详细说明，请参阅 [Google Play publishing documentation][play]. (国内不存在的，但你可以发布到国内的各种应用商店)


[manifest]: http://developer.android.com/guide/topics/manifest/manifest-intro.html
[manifesttag]: https://developer.android.com/guide/topics/manifest/manifest-element.html
[appid]: https://developer.android.com/studio/build/application-id.html
[permissiontag]: https://developer.android.com/guide/topics/manifest/uses-permission-element.html
[applicationtag]: https://developer.android.com/guide/topics/manifest/application-element.html
[versions]: https://developer.android.com/studio/publish/versioning.html
[launchericons]: https://developer.android.com/guide/practices/ui_guidelines/icon_design_launcher.html
[configurationqualifiers]: https://developer.android.com/guide/practices/screens_support.html#qualifiers
[play]: https://developer.android.com/distribute/googleplay/start.html
