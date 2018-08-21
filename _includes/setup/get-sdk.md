## 获取Flutter SDK

1. 去flutter官网下载其最新可用的安装包，[转到下载页](https://flutter.io/sdk-archive/#macos) 。

   注意，Flutter的渠道版本会不停变动，请以Flutter官网为准。另外，在中国大陆地区，要想正常获取安装包列表或下载安装包，可能需要翻墙，读者也可以去Flutter github项目下去下载安装包，[转到下载页](https://github.com/flutter/flutter/releases) 。

2. 解压安装包到你想安装的目录，如：

   ```shell
   cd ~/development
   unzip ~/Downloads/flutter_macos_v0.5.1-beta.zip
   ```

3. 添加`flutter`相关工具到path中：

   ```shell
   export PATH=`pwd`/flutter/bin:$PATH
   ```

   此代码只能暂时针对当前命令行窗口设置PATH环境变量，要想永久将Flutter添加到PATH中请参考下面**更新环境变量** 部分。

<div class="alert alert-info" markdown="1" style="margin-top:-5px">
 **注意：** 由于一些`flutter`命令需要联网获取数据，如果您是在国内访问，由于众所周知的原因，直接访问很可能不会成功。
 上面的`PUB_HOSTED_URL`和`FLUTTER_STORAGE_BASE_URL`是google为国内开发者搭建的临时镜像。详情请参考 [Using Flutter in China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)
</div>


要更新现有版本的Flutter，请参阅[升级Flutter](/upgrading/)。



### 运行 flutter doctor

运行以下命令查看是否需要安装其它依赖项来完成安装：

```commandline
flutter doctor
```

该命令检查您的环境并在终端窗口中显示报告。Dart SDK已经在捆绑在Flutter里了，没有必要单独安装Dart。
仔细检查命令行输出以获取可能需要安装的其他软件或进一步需要执行的任务（以粗体显示）

例如:
<pre>
[-] Android toolchain - develop for Android devices
    • Android SDK at /Users/obiwan/Library/Android/sdk
    <strong>✗ Android SDK is missing command line tools; download from https://goo.gl/XxQghQ</strong>
    • Try re-installing or updating your Android SDK,
      visit https://flutter.io/setup/#android-setup for detailed instructions.
</pre>

一般的错误会是xcode或Android Studio版本太低、或者没有`ANDROID_HOME`环境变量等，请按照提示解决。下面贴一个笔者本机(mac)的环境变量配置，您可以对比修正：
```commandline
  export PATH=/Users/用户名/Documents/flutter/flutter/bin:$PATH
  export ANDROID_HOME="/Users/用户名/Documents/android_sdk" //android sdk目录，替换为你自己的即可
  export PATH=${PATH}:${ANDROID_HOME}/tools
  export PATH=${PATH}:${ANDROID_HOME}/platform-tools
  export PUB_HOSTED_URL=https://pub.flutter-io.cn
  export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

第一次运行一个flutter命令（如flutter doctor）时，它会下载它自己的依赖项并自行编译。以后再运行就会快得多。

以下各部分介绍如何执行这些任务并完成设置过程。你会看到在`flutter doctor`输出中，
如果你选择使用IDE，我们提供了，IntelliJ IDEA，Android Studio和VS Code的插件，
请参阅[编辑器设置](/get-started/editor/) 以了解安装Flutter和Dart插件的步骤。

一旦你安装了任何缺失的依赖，再次运行`flutter doctor`命令来验证你是否已经正确地设置了。

该flutter工具使用Google Analytics匿名报告功能使用情况统计信息和基本崩溃报告。
这些数据用于帮助改进Flutter工具。Analytics不是一运行或在运行涉及`flutter config`的任何命令时就发送，
因此您可以在发送任何数据之前退出分析。要禁用报告，请执行`flutter config --no-analytics`并显示当前设置，然后执行`flutter config`。
请参阅[Google的隐私政策](https://www.google.com/intl/en/policies/privacy/)。
{: .alert-warning}
