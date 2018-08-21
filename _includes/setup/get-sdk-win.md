## 获取Flutter SDK

1. 去flutter官网下载其最新可用的安装包，[点击下载](https://flutter.io/sdk-archive/#windows) ；

   注意，Flutter的渠道版本会不停变动，请以Flutter官网为准。另外，在中国大陆地区，要想正常获取安装包列表或下载安装包，可能需要翻墙，读者也可以去Flutter github项目下去[下载安装包](https://github.com/flutter/flutter/releases) 。

2. 将安装包zip解压到你想安装Flutter SDK的路径（如：`C:\src\flutter`；注意，**不要**将flutter安装到需要一些高权限的路径如`C:\Program Files\`）。

3. 在Flutter安装目录的`flutter`文件下找到`flutter_console.bat`，双击运行并启动**flutter命令行**，接下来，你就可以在Flutter命令行运行flutter命令了。
<div class="alert alert-info" markdown="1" style="margin-top:-5px">
 **注意：** 由于一些`flutter`命令需要联网获取数据，如果您是在国内访问，由于众所周知的原因，直接访问很可能不会成功。
 上面的`PUB_HOSTED_URL`和`FLUTTER_STORAGE_BASE_URL`是google为国内开发者搭建的临时镜像。详情请参考 [Using Flutter in China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)
</div>

上述命令为当前终端窗口临时设置PATH变量。要将Flutter永久添加到路径中，请参阅[更新路径](#更新环境变量)。

要更新现有版本的Flutter，请参阅[升级Flutter](/upgrading/)。

### 更新环境变量


要在终端运行 `flutter` 命令， 你需要添加以下环境变量到系统PATH：

* 转到 “控制面板>用户帐户>用户帐户>更改我的环境变量”
* 在“用户变量”下检查是否有名为“Path”的条目:
    * 如果该条目存在, 追加 `flutter\bin`的全路径，使用 `;` 作为分隔符.
    * 如果条目不存在, 创建一个新用户变量 `Path` ，然后将 `flutter\bin`的全路径作为它的值.
* 在“用户变量”下检查是否有名为"PUB_HOSTED_URL"和"FLUTTER_STORAGE_BASE_URL"的条目，如果没有，也添加它们。

重启Windows以应用此更改


### 运行 flutter doctor

打开一个新的命令提示符或PowerShell窗口并运行以下命令以查看是否需要安装任何依赖项来完成安装：

```commandline
 flutter doctor
```
在命令提示符或PowerShell窗口中运行此命令。目前，Flutter不支持像Git Bash这样的第三方shell。
{: .alert-warning}

该命令检查您的环境并在终端窗口中显示报告。Dart SDK已经在捆绑在Flutter里了，没有必要单独安装Dart。
仔细检查命令行输出以获取可能需要安装的其他软件或进一步需要执行的任务（以粗体显示）

例如:
<pre>
[-] Android toolchain - develop for Android devices
    • Android SDK at D:\Android\sdk
    <strong>✗ Android SDK is missing command line tools; download from https://goo.gl/XxQghQ</strong>
    • Try re-installing or updating your Android SDK,
      visit https://flutter.io/setup/#android-setup for detailed instructions.
</pre>


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

