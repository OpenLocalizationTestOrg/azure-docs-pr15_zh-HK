<properties
    pageTitle="将推式通知添加到 Apache Cordova Azure 的移动应用程序的应用程序 |Azure 应用程序服务"
    description="了解如何使用 Azure 移动应用程序可于 Apache Cordova 应用程序发送推式通知。"
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>添加到 Apache Cordova app 推式通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>概述

在本教程中，您添加推式通知到[Apache Cordova 快速启动]项目，以便每次插入记录到设备发送推式通知。

如果您不使用下载快速入门服务器项目，将需要推送通知扩展包。 有关详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 。

##<a name="prerequisites"></a>系统必备组件

本教程介绍了开发使用 Visual Studio 在 Google Android 模拟器、 Android 设备，Windows 设备和 iOS 设备运行的 2015年的 Apache Cordova 应用程序。

若要完成本教程，您需要︰

* 采用[Visual Studio 社区 2015年]或更高版本的计算机。
* [Apache Cordova visual Studio 工具]。
* [活动的 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。
* 已完成的[Apache Cordova 快速启动]项目。
* (Android)具有经验证的电子邮件地址的[Google 帐户]。
* (iOS)苹果开发商计划成员资格和 iOS 设备 （iOS 模拟器不支持推入）。
* (Windows)Windows 商店开发者帐户和 Windows 10 设备。

##<a name="configure-hub"></a>配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[观看视频显示在此部分中的步骤](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>更新发送推式通知的服务器项目

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>修改您的 Cordova 应用程序来接收推式通知

您必须确保您 Apache Cordova 应用程序项目已准备好安装 Cordova 推插件以及任何特定于平台的推送服务处理推式通知。

#### <a name="update-the-cordova-version-in-your-project"></a>更新您的项目中 Cordova 版本。

我们建议，如果您的项目配置为使用较旧的版本，Cordova 6.1.1 为更新客户端项目。 若要更新该项目，右键单击打开配置设计器类。 选定的平台选项卡并**Cordova CLI**在文本框中选择 6.1.1。

选择**生成**，然后**生成解决方案**更新项目。

#### <a name="install-the-push-plugin"></a>安装推插件

Apache Cordova 应用程序本身不处理设备或网络功能。  通过发布[npm](https://www.npmjs.com/)或 GitHub 上的插件提供这些功能。  `phonegap-plugin-push`插件用于处理网络推式通知。

可以通过以下方式之一安装推插件︰

**从命令提示符处︰**

执行以下命令︰

    cordova plugin add phonegap-plugin-push

**从 Visual Studio 内︰**

1.  在解决方案资源管理器中打开`config.xml`文件，请单击**插件** > **自定义**、 选择**Git**安装源，然后输入`https://github.com/phonegap/phonegap-plugin-push`作为源。

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  单击安装源旁边的箭头。

3. 在**SENDER_ID**，如果您已经有数字项目 ID 对于 Google 开发人员控制台项目中，您可以添加它这里。 否则，输入占位符值，如 777777，然后如果面向的 Android 可以更新此值在类以后。

4. 单击**添加**。

现在安装推插件。

####<a name="install-the-device-plugin"></a>安装设备插件

相同的步骤用来安装推插件，但您会发现该设备插件的核心插件列表中 (单击**插件** > **核心**找到它)。 您需要获取平台名称，此插件 (`device.platform`)。

#### <a name="register-your-device-for-push-on-start-up"></a>注册您的设备为推上启动

最初，我们将为 Android 包括一些很少的代码。 稍后，我们将进行一些小的修改，要运行 iOS 或 Windows 10。

1. 在登录过程中，或在底部的**onDeviceReady**方法的回调过程中添加对**registerForPushNotifications**的调用︰

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    本示例演示调用**registerForPushNotifications**身份验证成功后，其时，建议在您的应用程序中使用推式通知和身份验证。

2. 添加新的**registerForPushNotifications**方法，如下所示︰

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android)在上面的代码中，替换`Your_Project_ID`与数字从[Google 开发人员控制台]应用程序的项目 ID。

## <a name="optional-configure-and-run-the-app-on-android"></a>（可选）配置和运行在 Android 上的应用程序

完成此节可启用推式通知的 Android。

####<a name="enable-gcm"></a>启用 Firebase 云消息

由于我们的最初目标 Google Android 平台，您必须启用 Firebase 云消息。 同样，如果您已面向 Microsoft Windows 设备，您将启用 WNS 支持。

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>配置发送推式请求使用 FCM 的移动应用程序后端

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>配置 Android Cordova 应用程序

在 Cordova 应用程序中，打开类和替换`Your_Project_ID`与数字从[Google 开发人员控制台]应用程序的项目 ID。

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

打开 index.js，更新代码以使用您的数字项目 id。

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>配置 Android 设备的 USB 调试

部署您的应用程序到 Android 设备之前，您需要启用 USB 调试。  在 Android 手机上执行以下步骤︰

1. 转到**设置** > **有关电话**，然后点击**内部版本号**直到开发人员模式下启用 （大约 7 倍）。

2. 在**设置** > **开发人员选项**启用**USB 调试**，然后连接到您开发通过 USB 线与 PC 的 Android 手机。

我们测试了这使用 Google 结点 5 X 设备运行 Android 6.0 （到底）。  但是，技术都是通用任何现代的 Android 版本。

#### <a name="install-google-play-services"></a>安装 Google 播放服务

推入插件依赖于 Android Google 播放服务的推式通知。  

1.  在**Visual Studio**中，单击**工具** > **Android** > **Android SDK 管理器**中，展开**显示额外内容**文件夹并选中复选框，以确保每个下面 Sdk 安装。
    * Android 2.3 或更高版本
    * 27 或更高版本的 Google 资源库修订
    * Google 播放服务 9.0.2 或更高版本

2.  单击**安装包**，并等待安装完成。

当前所需库都列在[推入插件 phonegap 安装文档]。

#### <a name="test-push-notifications-in-the-app-on-android"></a>在 Android 上的应用程序中测试推式通知

通过运行应用程序并将项插入 TodoItem 表中，您可以立即测试推式通知。 你可以从同一个设备或第二个设备，只要您使用的同一个后端。 测试 Cordova 在 Android 平台上的应用程序中的以下方法之一︰

- **在物理设备︰**  
将 Android 设备连接到开发计算机使用 USB 电缆。  而不是**Google Android 的仿真程序**，选择**设备**。 Visual Studio 将应用程序部署到该设备，然后运行它。  您然后可以与该设备上的应用程序进行交互。  
提高您的开发体验。  共享应用程序，如[Mobizen]的屏幕可以帮助您在开发 Android 应用程序通过将 web 浏览器到 Android 屏幕投影在您的 PC 上。

- **在 Android 的仿真程序︰**  
有在仿真器上运行时，需要额外的配置步骤。

    请确保您正在部署到或调试上了 Google Api 集作为目标，如下所示在 Android 虚拟设备 (AVD) 管理器的虚拟设备。

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    如果您想要使用更快的 x86 仿真程序，[安装 HAXM 驱动程序](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM)配置仿真程序，并使用它。

    通过单击**应用程序**到 Android 设备添加 Google 帐户 > **设置** > **添加帐户**，然后按照提示添加现有的 Google 帐户 （我们建议使用现有的帐户，而不是创建一个新） 的设备。

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    运行任务列表应用程序作为之前，插入一个新的 todo 项。 这一次，是在通知区域显示一个通知图标。 您可以打开通知抽屉查看通知的全文。

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>（可选）配置和运行 iOS 上

本部分适用于 iOS 设备上运行 Cordova 项目。 如果您不使用 iOS 设备，您可以跳过本节。

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>安装并运行 iOS remotebuild 代理 Mac 或云服务上

您可以使用 Visual Studio 的 iOS 上运行 Cordova 应用程序之前，经过安装并运行 remotebuild 代理[iOS 安装指南](http://taco.visualstudio.com/en-us/docs/ios-guide/)中的步骤。

请确保您可以生成的 iOS 应用程序。 安装指南中的步骤所需的 iOS 从 Visual Studio 生成。 如果您没有 Mac，可以生成与 MacInCloud 类似的服务上使用 remotebuild 代理的 iOS。 有关更多信息，请参见[运行 iOS 应用程序在云中](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/)。

>[AZURE.NOTE] 在 iOS 中使用推插件需要 XCode 7 或更高版本。

####<a name="find-the-id-to-use-as-your-app-id"></a>查找要用作应用程序 ID 的 ID

注册您的推式通知，Cordova 应用程序中打开类的应用程序之前先查找`id`属性值在构件元素中，并将其复制以供以后使用。 在下面的 XML 中，ID 为`io.cordova.myapp7777777`。

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

以后，苹果的开发人员门户上创建的应用程序 ID 时，才使用此标识符。 （如果您在开发人员门户上创建不同的应用程序 ID，并且想要使用的需要以后在本教程中将此 ID 在 config.xml 执行几个额外的步骤。 中构件元素的 ID 必须与应用程序开发人员门户上。）

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>注册为苹果的开发人员门户网站上的推式通知应用程序

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[观看视频显示相似的步骤](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>配置 Azure 发送推式通知

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>请验证您的应用程序 ID 匹配 Cordova 应用程序

如果您已经创建在苹果开发者帐户应用程序 ID 匹配的 config.xml 中的构件元素 ID，您可以跳过此步骤。 但是，如果 Id 不匹配，请执行以下步骤︰

1. 从项目中删除平台文件夹。

2. 从项目中删除的插件文件夹。

3. 从项目中删除 node_modules 文件夹。

4. 更新使用苹果开发者帐户中创建的应用程序标识的类中的构件元素的 id 属性。

5. 重新生成项目。

#####<a name="test-push-notifications-in-your-ios-app"></a>IOS 应用程序中测试推式通知

1. Visual Studio，以确保该**iOS**选中作为部署目标，然后选择要连接的 iOS 设备上运行的**设备**。

    您可以运行 iOS 设备连接到您的 PC 上使用 iTunes。 IOS 模拟器不支持推式通知。

2. 在 Visual Studio 生成项目并在 iOS 设备上，启动该应用程序按**f5 键**的**运行**按钮，然后单击**确定**以接受推式通知。

    >[AZURE.NOTE] 从您的应用程序，您必须显式地接受推式通知。 此请求仅发生第一次应用程序运行。

3. 在应用程序中，键入任务，然后单击加号 （+） 图标。

4. 请验证通知收到后，然后单击确定来关闭通知。

##<a name="optional-configure-and-run-on-windows"></a>（可选）配置和运行在 Windows 上

本部分适用于 Windows 10 （PhoneGap 推插件都支持 Windows 10） 的设备上运行 Apache Cordova 应用程序项目。 如果您不使用 Windows 设备，您可以跳过本节。

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>与 WNS 注册您的 Windows 应用程序的推式通知

若要在 Visual Studio 中使用的存储选项，Windows 目标从列表中选择的解决方案平台，如**Windows x64**或**Windows x86** （避免**Windows AnyCPU**出现推式通知）。

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[观看视频显示相似的步骤](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>为 WNS 配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>配置 Cordova 应用程序以支持 Windows 推式通知

打开配置设计器 （类和选择**视图设计器**上单击鼠标右键），选择**Windows**选项卡，然后选择**Windows 10**下**Windows 目标版本**。

>[AZURE.NOTE] 如果使用 Cordova 之前版本 Cordova 5.1.1 (6.1.1 建议)，您还必须为 true config.xml 中设置祝酒能够标志。

以支持推送通知在默认 （调试） 中的生成，打开 build.json 文件。 将"发布"配置复制到您的调试配置。

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

此更新之后，前面的代码应如下所示。

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

构建应用程序并验证您有任何错误。 您客户端应用程序应立即注册移动应用程序的后端通知。 为您的解决方案中的每个 Windows 项目重复本部分。

####<a name="test-push-notifications-in-your-windows-app"></a>在您的 Windows 应用程序中测试推式通知

在 Visual Studio 中，请确保选择 Windows 平台作为部署目标，例如**Windows x64**或**Windows x86**。 若要在托管 Visual Studio 窗口 10 PC 上运行应用程序，选择**本地计算机**。

按运行按钮以生成项目并启动应用程序。

在应用程序中，为新的 todoitem，键入一个名称，然后单击加号 （+） 图标来添加它。

验证在添加项目时，收到通知。

##<a name="next-steps"></a>下一步行动

* 阅读以了解有关推式通知的[通知集线器]。
* 如果您没有继续[添加]身份验证于 Apache Cordova 应用程序的教程。

了解如何使用 Sdk。

* [Apache Cordova SDK]
* [ASP.NET 服务器 SDK]
* [Node.js SDK 服务器]

<!-- URLs -->
[添加验证]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova 快速入门]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google 帐户]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google 开发人员控制台]: https://console.developers.google.com/home/dashboard
[phonegap-插件-推式安装文档]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio 社区 2015]: http://www.visualstudio.com/
[Apache Cordova visual Studio 工具]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[通知集线器]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET 服务器 SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js SDK 服务器]: app-service-mobile-node-backend-how-to-use-server-sdk.md
