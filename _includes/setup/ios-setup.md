## iOS 设置

### 安装 Xcode

要为iOS开发Flutter应用程序，您需要Xcode 7.2或更高版本:

1. 安装Xcode 7.2或更新版本(通过[链接下载](https://developer.apple.com/xcode/)或[苹果应用商店](https://itunes.apple.com/us/app/xcode/id497799835)).

2. 配置Xcode命令行工具以使用新安装的Xcode版本 `sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer`
   对于大多数情况，当您想要使用最新版本的Xcode时，这是正确的路径。如果您需要使用不同的版本，请指定相应路径。

3. 确保Xcode许可协议是通过打开一次Xcode或通过命令`sudo xcodebuild -license`同意过了.

使用Xcode，您可以在iOS设备或模拟器上运行Flutter应用程序。

### 设置iOS模拟器

要准备在iOS模拟器上运行并测试您的Flutter应用，请按以下步骤操作：

1. 在Mac上，通过Spotlight或使用以下命令找到模拟器:
   ```commandline
   open -a Simulator
   ```
2. 通过检查模拟器 **硬件>设备** 菜单中的设置，确保您的模拟器正在使用64位设备（iPhone 5s或更高版本）.
3. 根据您的开发机器的屏幕大小，模拟的高清屏iOS设备可能会使您的屏幕溢出。在模拟器的 **Window> Scale** 菜单下设置设备比例
4. 运行 `flutter run`启动您的应用.


### 安装到iOS设备

要将您的Flutter应用安装到iOS真机设备，您需要一些额外的工具和一个Apple帐户，您还需要在Xcode中进行设置。


1. 安装 [homebrew](http://brew.sh/) （如果已经安装了brew,跳过此步骤）.
2. 打开终端并运行这些命令来安装用于将Flutter应用安装到iOS设备的工具
   ```commandline
   brew update
   brew install --HEAD libimobiledevice
   brew install ideviceinstaller ios-deploy cocoapods
   pod setup
   ```

如果这些命令中的任何一个失败并出现错误，请运行brew doctor并按照说明解决问题.

1. 遵循Xcode签名流程来配置您的项目:
    1. 在你Flutter项目目录中通过 `open ios/Runner.xcworkspace` 打开默认的Xcode workspace.

    2. 在Xcode中，选择导航面板左侧中的`Runner`项目

    3. 在`Runner` target设置页面中，确保在 **常规>签名>团队** 下选择了您的开发团队。当您选择一个团队时，Xcode会创建并下载开发证书，向您的设备注册您的帐户，并创建和下载配置文件（如果需要）
        * 要开始您的第一个iOS开发项目，您可能需要使用您的Apple ID登录Xcode.<br>
        ![Xcode account add](/images/setup/xcode-account.png)<br>
        任何Apple ID都支持开发和测试。需要注册Apple开发者计划才能将您的应用分发到App Store. 查看[differences between Apple membership types](https://developer.apple.com/support/compare-memberships).
        * 当您第一次attach真机设备进行iOS开发时，您需要同时信任你的Mac和该设备上的开发证书。首次将iOS设备连接到Mac时,请在对话框中选择 `Trust`。<br><br>
        ![Trust Mac](/images/setup/trust-computer.png)<br><br>
        然后，转到iOS设备上的设置应用程序，选择 **常规>设备管理** 并信任您的证书。
        * 如果Xcode中的自动签名失败，请验证项目的 **General > Identity > Bundle Identifier** 值是否唯一.<br>

        ![Check the app's Bundle ID](/images/setup/xcode-unique-bundle-id.png)

2. 运行启动您的应用程序 `flutter run`.
