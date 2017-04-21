<properties
    pageTitle="如何使用 Apache Cordova 插件 Azure 的移动应用程序"
    description="如何使用 Apache Cordova 插件 Azure 的移动应用程序"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>如何对 Azure 的移动应用程序中使用 Apache Cordova 客户端库

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

本指南指导您执行使用最新的[Apache Cordova 插件用于 Azure 的移动应用程序]的常见方案。 如果您是新手 Azure 移动应用程序，第一个完成[Azure 移动应用程序快速开始]创建后端，创建一个表，并下载预构建的 Apache Cordova 项目。 在本指南中，我们将专注于客户端的 Apache Cordova 插件。

## <a name="supported-platforms"></a>支持的平台

此 SDK 支持 Apache Cordova v6.0.0，并在以后 iOS、 Android 和 Windows 设备。  平台支持如下所示︰

* Android 的 API 19-24 (KitKat 通过 Nougat)
* iOS 版本 8.0 版或更高版本。
* Windows Phone 8.0
* Windows Phone 8.1
* 通用的 Windows 平台

##<a name="Setup"></a>安装和系统必备组件

本指南假定您已使用的表创建后端。 本指南假定表中这些教程中的表相同的架构。 本指南还假设您已经将 Apache Cordova 插件添加到代码中。  如果您还没有这样做，您可能将 Apache Cordova 插件添加到您在命令行上的项目︰

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

创建[您的第一个 Apache Cordova 应用程序]的详细信息，请参阅其文档。

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>如何︰ 验证用户的身份

Azure 应用程序服务支持身份验证和授权应用程序用户使用各种外部身份提供程序︰ Facebook、 Google、 Microsoft 帐户和 Twitter。 可以对表来限制对特定操作只有已通过身份验证的用户设置权限。 此外可以使用已经过身份验证的用户的身份在服务器脚本中实现授权规则。 有关详细信息，请参阅[身份验证入门]教程。

在 Apache Cordova 应用程序中使用身份验证，必须可用以下 Cordova 插件︰

* [cordova 插件设备]
* [cordova-插件-inappbrowser]

支持两种身份验证流程︰ 服务器和客户端流。  服务器流提供了最简单的身份验证体验，因为它依赖于提供商的 web 身份验证接口。 客户端流如允许使用特定于设备的功能更深度的集成的单点登录因为它依赖于特定于提供程序的特定于设备的 Sdk。

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>如何︰ 配置应用程序服务移动的外部的重定向 Url。

几种类型的 Apache Cordova 应用程序使用环回功能处理 OAuth UI 流。  由于身份验证服务仅知道如何利用您的服务默认情况下，在本地主机上的 OAuth UI 流导致的问题。  有问题的 OAuth UI 流的示例包括︰

- 波纹仿真程序。
- 实时与 Ionic 的重新加载。
- 在本地运行移动后端
- 在不同的 Azure 应用程序服务于一种提供身份验证运行移动的后端。

按照这些说明来添加到配置本地设置︰

1. 登录到[Azure 门户]
2. 选择**所有资源**或**应用程序服务**，然后单击您的移动应用程序的名称。
3. 单击**工具**
4. 在遵守菜单上，单击**资源管理器**，然后单击**转到**。  打开新窗口或选项卡。
5. 展开**配置**、 为您的站点在左侧的导航栏中的**authsettings**节点。
6. 单击**编辑**
7. "AllowedExternalRedirectUrls"元素的外观。  它可能会设置为空值或值的数组。  将值更改为下面的值︰

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    替换为您的服务的 Url 的 Url。  例如"http://localhost:3000"（用于 Node.js 示例服务） 或"http://localhost:4400"（用于波纹服务）。  但是，这些 Url 是示例-您的具体情况，包括在示例中，所提到的服务可能会有所不同。
8. 单击屏幕右上角的**读/写**按钮。
9. 单击绿色**放置**按钮。

这些设置保存在这里。  不要关闭浏览器窗口，直到完成设置保存。
此外为您的应用程序服务的 CORS 设置添加这些环回 Url:

1. 登录到[Azure 门户]
2. 选择**所有资源**或**应用程序服务**，然后单击您的移动应用程序的名称。
3. 设置刀片式服务器将自动打开。  如果没有，请单击**所有设置**。
4. 单击**CORS** API 菜单下。
5. 输入您想要在框中添加的 URL 提供，然后按 enter 键。
6. 根据需要输入其他 Url。
7. 单击**保存**以保存设置。

花费大约 10-15 秒钟以新的设置才会生效。

##<a name="register-for-push"></a>如何︰ 注册推式通知

安装在[推入插件 phonegap]处理推式通知。  此插件可轻松添加使用`cordova plugin add`命令在命令行中，或通过在 Visual Studio 中 Git 插件安装程序。  下面的代码 Apache Cordova 应用程序中注册您的推送通知的设备︰

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

使用通知集线器 SDK 从服务器发送推式通知。  永远不会直接从客户端发送推式通知。 这样做可用于触发拒绝服务攻击通知集线器或企业型。  企业型可能禁止您的流量导致此类攻击。

<!-- URLs. -->
[Azure 门户]: https://portal.azure.com
[Azure 的移动应用程序快速启动]: app-service-mobile-cordova-get-started.md
[开始使用身份验证]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova Azure 的移动应用程序插件]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[第一个 Apache Cordova 应用程序]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-插件推]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova 插件设备]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-插件-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
