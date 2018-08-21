## Android设置

### 安装Android Studio

要为Android开发Flutter应用，您可以使用Mac，Windows或Linux（64位）机器.

Flutter需要安装和配置Android Studio:

1. 下载并安装 [Android Studio](https://developer.android.com/studio/index.html).

2. 启动Android Studio，然后执行“Android Studio安装向导”。这将安装最新的Android SDK，Android SDK平台工具和Android SDK构建工具，这是Flutter为Android开发时所必需的

### 设置您的Android设备

要准备在Android设备上运行并测试您的Flutter应用，您需要安装Android 4.1（API level 16）或更高版本的Android设备.

1. 在您的设备上启用 **开发人员选项** 和 **USB调试** 。详细说明可在[Android文档](https://developer.android.com/studio/debug/dev-options.html)中找到。
2. 使用USB将手机插入电脑。如果您的设备出现提示，请授权您的计算机访问您的设备。
3. 在终端中，运行 `flutter devices` 命令以验证Flutter识别您连接的Android设备。
4. 运行启动您的应用程序 `flutter run`。

默认情况下，Flutter使用的Android SDK版本是基于你的 `adb` 工具版本。
如果您想让Flutter使用不同版本的Android SDK，则必须将该 `ANDROID_HOME` 环境变量设置为SDK安装目录。

### 设置Android模拟器

要准备在Android模拟器上运行并测试您的Flutter应用，请按照以下步骤操作：

1. 在您的机器上启用 [VM acceleration](https://developer.android.com/studio/run/emulator-acceleration.html) .
1. 启动 **Android Studio>Tools>Android>AVD Manager** 并选择 **Create Virtual Device**.
1. 选择一个设备并选择 **Next**。
1. 为要模拟的Android版本选择一个或多个系统映像，然后选择 **Next**. 建议使用 _x86_ 或 _x86\_64_ image .
1. 在 Emulated Performance下, 选择 **Hardware - GLES 2.0** 以启用
[硬件加速](https://developer.android.com/studio/run/emulator-acceleration.html).
1. 验证AVD配置是否正确，然后选择 **Finish**。

   有关上述步骤的详细信息，请参阅 [Managing AVDs](https://developer.android.com/studio/run/managing-avds.html).
1. 在 Android Virtual Device Manager中, 点击工具栏的 **Run**。模拟器启动并显示所选操作系统版本或设备的启动画面.
1. 运行 `flutter run` 启动您的设备. 连接的设备名是 `Android SDK built for <platform>`,其中 _platform_ 是芯片系列, 如 x86.
