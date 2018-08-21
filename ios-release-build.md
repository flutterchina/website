---
layout: page
title: 发布的IOS版APP
permalink: /ios-release/
summary: 本文介绍了如何构建Flutter的IOS发布版，并将其发布到App Store或TestFlight。
keywords: 发布Flutter应用
---

本指南会一步步帮你将Flutter应用程序发布到[App Store][appstore]和[TestFlight][testflight]

* TOC Placeholder
{:toc}

## 准备

在开始发布您的应用程序之前，请确保它符合Apple的[App Review Guidelines][appreview].

为了将您的应用发布到App Store，您需要注册[Apple开发者计划](devprogram)。您可以在Apple的[Choosing a Membership][devprogram_membership]中阅读更多关于各种会员选项的信息。

## 在iTunes Connect上注册您的应用程序

[iTunes Connect][itunesconnect]是您管理应用程序生命周期的地方。您将定义您的应用程序名称和说明，添加屏幕截图，设置价格并管理版本到App Store和TestFlight。

注册您的应用程序涉及两个步骤：注册唯一的Bundle ID，并在iTunes Connect上创建应用程序记录。

有关iTunes Connect的详细概述，请参阅[iTunes Connect开发者指南][itunesconnect_guide]

### 注册一个 Bundle ID

每个iOS应用程序都与一个Bundle ID关联，这是一个在Apple注册的唯一标识符。要为您的应用注册一个Bundle ID，请按照以下步骤操作:

1. 打开开发者帐户的[App IDs][devportal_appids]页.
1. 点击 **+** 创建一个 Bundle ID.
1. 输入应用程序名称, 选择 **Explicit App ID**, 然后输入一个 ID.
1. 选择您的应用将使用的服务，然后点击"Continue"
1. 在下一页中，确认详细信息，然后点击 **Register** 注册你的Bundle ID

### 在iTunes Connect上创建应用程序记录

接下来，您将在iTunes Connect上注册您的应用程序：

1. 在浏览器中打开[iTunes Connect][itunesconnect_login].
1. 在iTunes Connect登陆页上, 点击 **My Apps**.
1. 点击My App页面左上角的 **+** ，然后选择**New App**.
1. 填写您的应用详细信息。在Platforms部分中，确保已选中iOS。由于Flutter目前不支持tvOS，请不要选中该复选框。点击**Create**
1. 导航到您app的应用程序详细信息，**App Information** 。
1. 在 General Information 部分, 选择您在上一步中注册的软件包ID。

有关详细的概述，请参阅 [Creating an iTunes Connect Record for an App][itunesconnect_guide_register].

## 查看Xcode项目设置

在这一步中，您将回顾Xcode工作区中最重要的设置。有关详细的过程和说明，请参阅Configuring Your Xcode Project for Distribution][distributionguide_config]

在Xcode中导航到您的target设置:

1. 在Xcode中, 在你的工程目录中的`ios`文件夹下打开`Runner.xcworkspace`.
1. 要查看您的应用程序的设置，请在Xcode项目导航器中选择**Runner**项目。然后，在主视图边栏中，选择**Runner**target
1. 选择 **General** 选项卡.

接下来，您将验证最重要的设置：

在 Identity 部分:

  * `Display Name:` 要在主屏幕和其他地方显示的应用程序的名称
  * `Bundle Identifier:` 您在iTunes Connect上注册的App ID.

在 Signing 部分:

  * `Automatically manage signing:`  Xcode是否应该自动管理应用程序签名和生成。默认设置为`true`，对大多数应用程序来说应该足够了。对于更复杂的场景，请参阅[Code Signing Guide][codesigning_guide]。
  * `Team:` 选择与您注册的Apple Developer帐户关联的团队。如果需要，请选择**Add Account...**，然后更新此设置

在 Deployment Info 部分:

  * `Deployment Target:` 您的应用将支持的最低iOS版本。Flutter支持iOS 8.0及更高版本。如果您的应用程序包含使用iOS 8中不可用的API的Objective-C或Swift代码，请适当更新此设置。

项目设置的General选项卡应该类似于以下内容:

![Xcode Project Settings](/images/releaseguide/xcode_settings.png)

有关应用程序签名的详细概述，请参阅 Certificates][appsigning].

## 添加应用程序图标


当创建新的Flutter应用程序时，会创建一个占位图标集。在这一步中，您将用应用图标替换这些占位图标：

1. 查看[iOS App Icon][appicon] 指南.
1. 在Xcode项目导航器中，在`Runner`文件夹中选择`Assets.xcassets`。使用您自己的应用程序图标更换占位图标
1. 运行`flutter run`， 验证应用图标已被替换

## 创建一个构建档案

在这一步中，您将创建一个构建档案并将您的构建上传到iTunes Connect：

在开发过程中，您一直在构建、调试、测试*debug*版本。当您准备将应用发布到App Store或TestFlight上时，您需要准备*release* 版本:

在命令行上，在您的应用程序目录中执行以下步骤：

1. 运行`flutter build ios`以创建release版本（`flutter build`默认为`--release`）
1. 为确保Xcode刷新release模式配置，关闭并重新打开Xcode workspace。对于Xcode 8.3和更高版本，这一步不是必需的

在Xcode中，配置应用程序版本并构建：

1. 在Xcode中，在您工程目录下的`ios`文件夹中打开`Runner.xcworkspace`.
1. 选择 **Product > Scheme > Runner**.
1. 选择 **Product > Destination > Generic iOS Device**.
1. 在Xcode项目导航器中选择 **Runner** , 然后在设置视图边栏中选择选择 **Runner** target .
1. 在Identity部分中，将**Version**更新为您希望发布的面向用户的版本号
1. 在Identity部分中，将**Build**标识更新为用于跟踪iTunes Connect上的此版本的唯一版本号。每次上传都需要一个唯一的build号

最后，创建一个构建档案并将其上传到iTunes Connect：

1. 选择 **Product > Archive** 以生成构建档案.
1. 在Xcode Organizer窗口的边栏中，选择您的iOS应用程序，然后选择您刚刚生成的build档案
1. 点击**Validate...** 按钮. 如果报错，请解决它们并生成另一个build。您可以重复使用相同的build ID，直到您上传档案
1. 档案已成功验证后，单击**Upload to App Store...**，您可以在iTunes Connect的应用详情也的“Activities”选项卡中查看构建状态

您应该在30分钟内收到一封电子邮件，通知您您的构建已经过验证，并可以在TestFlight上发布给测试人员。此时，您可以选择是否在TestFlight上发布，或继续并将您的release版发布到App Store。

有关更多详细信息，请参阅 [Uploading Your App to iTunes Connect][distributionguide_upload].

## 在TestFlight上发布您的应用程序

[TestFlight][testflight]许开发人员将他们的应用程序推送给内部和外部测试人员。在这个可选步骤中，您将在TestFlight上发布build：

1. 在[iTunes Connect][itunesconnect_login]上导航到应用程序详细信息页面的TestFlight选项卡
1. 在侧边栏选择 **Internal Testing**.
1. 选择要发布到测试人员的build，然后单击 **Save**.
1. 加任何内部测试人员的电子邮件地址。您可以在iTunes Connect的用户和角色页面添加更多的内部用户，可从页面顶部的下拉菜单中获得.

有关更多详细信息，请参阅 [Distributing Your App Using TestFlight][distributionguide_testflight].

## 将您的应用发布到App Store

当您准备将应用发布到全世界时，请按照以下步骤将您的应用提交给App Store进行审查和发布：

1. 从iTunes应用程序的应用程序详情页的边栏中选择**Pricing and Availability**，然后填写所需的信息。
1. 从边栏选择状态。如果这是该应用的第一个版本，则其状态将为**1.0 Prepare for Submission**。完成所有必填字段
1. 点击 **Submit for Review**.

Apple会在应用程序审查过程完成时通知您。您的应用将根据您在**Version Release**部分指定的说明进行发布：

有关更多详细信息，请参阅将 [Submitting Your App to the Store][distributionguide_submit].

## 故障排除

[App Distribution Guide][distributionguide]提供了发布应用程序到App Store的详细介绍。它包含一个[Troubleshooting guide][distributionguide_troubleshooting]，其中包含针对应用程序分发常见问题的解决方案。

[appicon]: https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/
[appreview]: https://developer.apple.com/app-store/review/
[appsigning]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html
[appstore]: https://developer.apple.com/app-store/submissions/
[codesigning_guide]: https://developer.apple.com/library/content/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html
[devportal_appids]: https://developer.apple.com/account/ios/identifier/bundle
[devprogram]: https://developer.apple.com/programs/
[devprogram_membership]: https://developer.apple.com/support/compare-memberships/
[distributionguide]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html
[distributionguide_config]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html
[distributionguide_submit]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/SubmittingYourApp/SubmittingYourApp.html
[distributionguide_testflight]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/DistributingYourAppUsingTestFlight/DistributingYourAppUsingTestFlight.html
[distributionguide_troubleshooting]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Troubleshooting/Troubleshooting.html
[distributionguide_upload]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/UploadingYourApptoiTunesConnect/UploadingYourApptoiTunesConnect.html
[itunesconnect]: https://developer.apple.com/support/itunes-connect/
[itunesconnect_guide]: https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/About.html
[itunesconnect_guide_register]: https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/CreatingiTunesConnectRecord.html
[itunesconnect_login]: https://itunesconnect.apple.com/
[testflight]: https://developer.apple.com/testflight/
