<properties
    pageTitle="如何启用跨应用程序上使用 ADAL 的 iOS 的 SSO |Microsoft Azure"
    description="如何使用 ADAL SDK 的功能可以跨应用程序启用单一登录。 "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>如何启用跨应用程序上使用 ADAL 的 iOS 的 SSO


提供单一登录 (SSO)，以便用户只需一次输入凭据并自动将这些凭据使用跨应用程序现在应由客户。 如果用户快速的不满会导致不得不这样做不止一次为您的产品在小屏幕上，通常时间加上例如电话联络或 texted 代码，其他因子 (2FA) 中输入他们的用户名和密码的难度。

此外，如果要使用其他应用程序可能会使用如 Microsoft 客户或从 Office365 的工作帐户标识平台，客户期望这些凭据可用于跨供应商无论其应用程序。

Microsoft 身份平台，以及 Microsoft 标识 Sdk，为您完成所有此辛勤工作并为您提供您自己的应用程序套件中的任何一个户满意 SSO 客户的能力; 与我们的代理功能和跨整个设备身份验证器应用程序一样。

本演练将告诉您如何配置应用程序，以向客户提供这一优势中的我们 SDK。

本演练适用于︰

* Azure 的活动目录
* Azure 的 Active Directory B2C
* Azure 的 Active Directory B2B
* Azure 的 Active Directory 条件访问


请注意，下面的文档假定您了解如何[设置应用程序在 Azure Active Directory 的传统门户网站](./develop/active-directory-how-to-integrate.md)以及具有与[Microsoft 身份 iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc)集成在您的应用程序。

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>在 Microsoft 身份平台 SSO 概念

### <a name="microsoft-identity-brokers"></a>Microsoft 身份经纪人

Microsoft 提供了为每个移动平台的应用程序允许凭据桥跨不同供应商的应用程序以及允许特殊的增强功能，需要一个安全的地方从何处来验证凭据。 我们称这些**经纪人**。 在 iOS 和 Android 上这些通过提供可下载应用程序的客户可以独立安装或可以推送到该设备由一家公司负责管理部分或全部设备为其员工。 这些经纪人支持只为某些应用程序或基于 IT 管理员所需的整个设备的安全管理。 在 Windows 中提供此功能通过内置于操作系统已知的技术作为 Web 身份验证代理帐户选择器。

了解我们如何使用这些经纪人和您的客户如何可能会看到它们在其登录流程，Microsoft 身份平台上读取的详细信息。

### <a name="patterns-for-logging-in-on-mobile-devices"></a>在移动设备上登录的模式

访问凭据的设备上按照 Microsoft 身份平台的两种基本模式︰

* 非中介辅助的登录
* 代理协助的登录名

#### <a name="non-broker-assisted-logins"></a>非中介辅助的登录

非中介辅助的登录是发生嵌入到应用程序，该应用程序在设备上使用的本地存储的登录体验。 可能在应用程序之间共享此存储但凭据紧密绑定到应用程序或应用程序使用此凭据的一套。 这是您在其中输入用户名和密码，在应用程序本身中的很可能会遇到在许多移动应用程序的体验。

这些登录具有以下优点︰

-  用户体验的应用程序中完全存在。
-  由提供单一登录体验到您的应用程序套件的同一证书签名的应用程序可以共享凭据。
-  周围的登录体验的控制提供给应用程序之前和之后登录。

这些登录具有以下缺点︰

- 用户无法通过使用 Microsoft 的标识，只有跨这些应用程序拥有和已配置的 Microsoft 标识的所有应用程序体验单点登录。
- 您的应用程序不能使用更先进的业务功能，如条件接收或使用 InTune 套的产品。
- 您的应用程序不能支持业务用户基于证书的身份验证。

这里是与您的应用程序以启用 SSO 的共享存储 Microsoft 标识 Sdk 的工作方式表示︰

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>代理协助的登录名

中介辅助登录是代理应用程序中发生，并且使用的存储和安全代理程序在设备上的所有应用程序利用 Microsoft 身份平台间共享凭据的登录体验。 这意味着，您的应用程序将依赖于代理程序才可以登录用户。 在 iOS 和 Android 上这些通过提供可下载应用程序的客户可以独立安装或可以推送到该设备由一家公司负责管理其用户的设备。 这种类型的应用程序的一个示例是 iOS 的 Azure 身份验证器应用程序。 在 Windows 中提供此功能通过内置于操作系统已知的技术作为 Web 身份验证代理帐户选择器。
经验因平台而异，有时可能会对用户造成中断如果不正确管理。 如果您有安装的 Facebook 应用程序，并在另一个应用程序中使用 Facebook 登录功能，您就可能最熟悉这种模式。 Microsoft 身份平台利用相同的模式。

这将是一个"过渡"的 iOS 的动画在您的应用程序发送到 Azure 身份验证器应用程序后台说到前台用户可以选择他们想要登录的帐户。  

Android 和 Windows 帐户选择器将显示在您的应用程序是对用户的中断更少。

#### <a name="how-the-broker-gets-invoked"></a>获取调用代理程序的方式

如果兼容代理安装在设备上，Azure 身份验证器的应用程序，如 Microsoft 标识 Sdk 将自动进行用户表示他们想要使用从 Microsoft 身份平台的任何帐户登录时调用您的经纪人的工作。 这可能是个人的 Microsoft 客户、 工作或学校科目或您提供的帐户和使用我们的 B2C 和 B2B 产品 Azure 中的主机。 通过使用极其安全的算法和加密我们确保凭据要求以及安全地发送回您的应用程序。 这些机制的具体的技术细节未发布，但苹果和 Google 开发与协作。

**如果 Microsoft 标识 SDK 调用中介或使用非中介辅助的流，开发人员必须的选择。** 但是如果开发人员选择不使用中介辅助流他们失去利用 SSO 凭据，该用户可能已添加的设备上以及防止其应用程序正在使用 Microsoft 提供的条件访问，Intune 管理功能，如客户的业务功能和基于证书的身份验证的优点。

这些登录具有以下优点︰

-  用户体验跨供应商无论其应用程序的 SSO。
-  您的应用程序可以利用等条件的访问更高级的业务功能或使用 InTune 套的产品。
-  为商务用户，您的应用程序可以支持基于证书的身份验证。
- 代理应用程序提供额外的安全算法和加密验证应用程序和用户的身份更安全登录体验。

这些登录具有以下缺点︰

- 在 iOS 用户时选择的凭据从应用程序的体验转换。
- 管理您的客户在您的应用程序的登录体验能力的损失。



还有的表示形式与代理应用程序以启用 SSO Microsoft 标识 Sdk 的工作方式︰

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

有了这些背景信息，您应该能够更好地理解和使用 Microsoft 身份平台和 Sdk 应用程序内实现 SSO。


## <a name="enabling-cross-app-sso-using-adal"></a>启用跨应用程序 SSO 使用 ADAL

这里我们将使用 ADAL iOS SDK 为︰

- 打开非中介辅助 SSO 应用程序套件
- 打开中介辅助 SSO 的支持

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>非代理启用 SSO 辅助 SSO

为跨应用程序的非中介辅助 SSO Microsoft 标识 Sdk 管理 SSO 的复杂性大部分为您。 这包括在缓存中查找适当的用户和维护为您查询已登录的用户的列表。

跨应用程序启用 SSO 您自己您需要执行下列操作︰

1. 确保所有的应用程序的用户相同的客户端 ID 的应用程序 id。
* 确保所有应用程序共享来自苹果的同一个签名证书，以便您可以共享 keychains
* 请求每个应用程序相同的钥匙链的权利。
* 请告知 Microsoft 标识 Sdk 共享钥匙链您希望我们使用。

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>使用相同的客户端 ID / 应用程序的应用程序套件中的所有应用程序 ID

为了让 Microsoft 身份平台，知道有允许多个应用程序共享令牌，每个应用程序将需要为共享同一个客户机 ID 或应用程序 id。 这是在门户中注册您的第一个应用程序时提供给您的唯一标识符。

您可能想知道如何将识别为 Microsoft 标识服务的不同应用程序是否使用相同的应用程序 id。 答案是与**重定向 Uri**。 每个应用程序可以有多个注册服务门户中的重定向 Uri。 在套件中的每个应用程序将具有不同的重定向 URI。 下面是效果的一个示例︰

App1 发出重定向 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 重定向 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 重定向 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

这些嵌套在同一客户机 ID 下 / 应用程序 ID 和查找基于重定向 URI SDK 配置中返回给我们。

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*请注意下面详细说明这些重定向 Uri 的格式。您可以使用任何重定向 URI 除非您希望支持代理程序，在这种情况下他们必须类似于以上*



#### <a name="create-keychain-sharing-between-applications"></a>创建应用程序之间共享的钥匙链

启用共享钥匙链超出了本文的范围，受苹果在其文档[中添加功能](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html)。 重要的是决定要调用您钥匙链和跨所有应用程序中添加该功能。

当您具有权利设置正确，则应该看到标题为您的项目目录中的文件`entitlements.plist`，其中包含类似于下面的内容︰

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

一旦您有在您的应用程序的每个已启用的钥匙链权利并准备好使用 SSO，请告知 Microsoft 标识 SDK 您钥匙链通过使用中的以下设置您`ADAuthenticationSettings`具有以下设置︰

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
当您在您的应用程序之间共享钥匙链的任何应用程序可以删除用户，或更糟的是在应用程序中删除所有标记。 这是如果您的应用程序依赖于这些标记以进行背景工作特别是灾难性的。 共享钥匙链意味着，您必须非常小心，在所有删除操作通过 Microsoft 标识 Sdk。

就是这样！ 现在，Microsoft 标识 SDK 将跨所有应用程序共享凭据。 用户列表也将在应用程序实例之间共享。

### <a name="turning-on-sso-for-broker-assisted-sso"></a>对于代理启用 SSO 辅助 SSO

应用程序可以使用任何代理，它安装在设备上的能力是**默认关闭的**。 为了向代理程序使用您的应用程序必须执行一些额外的配置，并向应用程序添加一些代码。

要遵循的步骤如下︰

1. 启用应用程序代码调用 MS SDK 中的中介模式。
2. 建立新的重定向 URI 并提供给应用程序，您的应用程序注册。
3. 正在注册 URL 方案。
4. iOS9 支持︰ 添加到 info.plist 文件的权限。


#### <a name="step-1-enable-broker-mode-in-your-application"></a>步骤 1︰ 启用在您的应用程序的代理模式
创建"上下文"或验证对象的初始设置时，应用程序可以使用该中介的功能被打开的。 通过在代码中设置凭据类型中执行此操作︰

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO`设置将允许 Microsoft 标识 SDK 尝试经纪人向外调用`AD_CREDENTIALS_EMBEDDED`将会阻止调用中介 Microsoft 标识 SDK。

#### <a name="step-2-registering-a-url-scheme"></a>步骤 2︰ 注册 URL 方案
Microsoft 身份平台使用 Url 调用代理程序，然后将控制返回到您的应用程序。 若要完成该往返需要注册 Microsoft 身份平台将了解应用程序的 URL 方案。 这可能是除了您可能会使用您的应用程序以前注册的任何其他应用程序方案。

> [AZURE.WARNING]
我们建议使 URL 方案非常独特，以最小化的另一个应用程序使用相同的 URL 方案的机会。 苹果不强制唯一性的已注册应用程序存储区中的 URL 方案。

下面是如何显示该内容在您的项目配置的一个示例。 您可能也这样做在 XCode 以及︰

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>第 3 步︰ 建立新的重定向 URL 方案的 URI

为了确保我们始终返回到正确的应用程序的凭据标记，我们需要确保我们回拨到您的应用程序可以验证 iOS 操作系统的方式。 IOS 操作系统报告给 Microsoft 代理应用程序调用它的应用程序的绑定 ID。 这不能被欺骗恶意应用程序。 因此，我们会利用这和我们的代理应用程序以确保令牌返回到正确的应用程序的 URI。 我们需要建立此唯一重定向 URI 这两个应用程序中，并设置为我们开发人员门户中重定向 URI。

您重定向 URI 必须以适当的形式︰

`<app-scheme>://<your.bundle.id>`

ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*

需要在您的应用程序注册使用[Azure 的传统门户网站](https://manage.windowsazure.com/)中指定此重定向 URI。 Azure AD 的应用程序注册的详细信息，请参阅[使用 Azure Active Directory 集成](./develop/active-directory-how-to-integrate.md)。


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>第 3a 步︰ 添加重定向 URI 中的应用程序和开发门户网站以支持基于证书的身份验证

为支持证书根据第二个"msauth"需要在您的应用程序和[Azure 的传统门户网站](https://manage.windowsazure.com/)处理证书身份验证，如果您想要添加支持的应用程序中注册的身份验证。

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

例如︰ *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>步骤 4: iOS9︰ 向您的应用程序中添加一个配置参数

ADAL 使用 – canOpenURL︰ 检查代理程序是否已安装在设备上。 在 iOS 中 9 苹果锁定方案，应用程序可以查询。 您将需要的 LSApplicationQueriesSchemes 部分中添加"msauth"您`info.plist file`。

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>您已配置 SSO ！

现在 Microsoft 标识 SDK 将自动同时在您的应用程序之间共享凭据并调用中介是否出现在其设备上。
