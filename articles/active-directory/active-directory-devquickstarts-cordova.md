<properties
    pageTitle="入门的 azure AD Cordova |Microsoft Azure"
    description="如何构建一个 Cordova 应用程序集成登录的 Azure AD 和调用 Azure 广告受保护的 Api 使用 OAuth。"
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>与 Apache 集成 Azure AD Cordova 应用程序

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova 使您能够开发 HTML5/JavaScript 应用程序可以运行在移动设备，成熟的本机应用程序。
对于 Azure AD，Cordova 应用程序可以添加企业级身份验证功能。 这要归功于在 iOS 中，Android、 Windows 应用商店和 Windows Phone 上环绕 Azure AD 本机 Sdk Cordova 插件可以增强您的应用程序支持用户的广告帐户号、 访问 Office 365 和 Azure API 和甚至保护对您自己的自定义 Web API 的调用。

在本教程中我们将使用 Apache Cordova 插件的活动目录身份验证库 (ADAL) 来提高一个简单的应用程序具有以下功能︰

-   只需几行代码，AD 用户进行身份验证并获取调用 Azure 广告图形 API 的标记。
-   使用该标记来调用图形 API 来查询该目录，并显示结果  
-   利用 ADAL 令牌缓存的最小化用户的身份验证提示。

为此，您需要︰

2. 向应用程序注册 Azure 的广告
2. 将代码添加到您的应用程序请求令牌
3. 添加代码以用于查询图形 API 使用该标记，并显示结果。
4. 创建您想要的目标，所有平台和 Cordova ADAL 插件 Cordova 部署项目，并在仿真程序中测试该解决方案。

## <a name="0--prerequisites"></a>*0。 系统必备组件*

若要完成本教程中，您将需要︰

- Azure AD 租户必须与应用程序开发的权限的帐户
- 开发环境配置为使用 Apache Cordova  

如果已经有两个设置，请直接到步骤 1。

如果您没有 Azure AD 租户，可以找到[说明如何获取此处](active-directory-howto-tenant.md)。

如果您没有在您的计算机上设置 Apache Cordova，请安装以下程序︰

- [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/)(您可以通过 NPM 程序包管理器可以轻松地安装︰ `npm install -g cordova`)

请注意，应在 PC 和 mac 上的工作

每个目标平台都有不同的系统必备组件。

- 生成并运行 Windows 的 Tablet PC 或电话应用程序版本
    - [对于 Windows 更新 2 或更高版本的 Visual Studio 2013 年](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8)(快速或另一个版本)。
- 生成并运行 iOS
    -   Xcode 5.x 或更高版本。 在 http://developer.apple.com/downloads 或[Mac 应用程序商店](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)下载
    -   [ios sim](https://www.npmjs.org/package/ios-sim) --使您可以启动到命令行从 iOS 模拟器的 iOS 应用程序 (可以通过终端轻松安装︰ `npm install -g ios-sim`)

- 生成并运行 Android 的应用程序
    - 安装[Java 开发工具箱 (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)或更高版本。 请确保`JAVA_HOME`（环境变量） 正确设置根据 JDK 安装路径 (例如 C:\Program Files\Java\jdk1.7.0_75)。
    - 安装[Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools)并添加`<android-sdk-location>\tools`位置 (例如，C:\tools\Android\android-sdk\tools) 为您`PATH`环境变量。
    - 打开 Android SDK 管理器 (例如，通过终端︰ `android`) 和安装
    - *Android 为 5.0.1 (API 21)*平台 SDK
    - *Android SDK 生成的工具*版本 19.1.0 或更高版本
    - *Android 支持存储库*（额外）

  Android sdk 并不提供任何默认仿真程序实例。 创建一个新通过运行`android avd`从终端，然后选择*创建...* （如果您想要在仿真器上运行 Android 的应用程序。 建议使用的*Api 级别*为 19 或更高版本，请参阅 [AVD 经理] (http://developer.android.com/tools/help/avd-manager.html)，Android 的仿真程序和创建选项的详细信息。


## <a name="1--register-an-application-with-azure-ad"></a>*1.使用 Azure AD 注册应用程序*

注︰ 此__步骤是可选的__。 本教程提供的预配置的值，您可以查看操作中的示例而无需进行任何资源调配您自己组织中。 但是建议您不要执行此步骤，并熟悉此过程中，将创建自己的应用程序时将需要。

Azure 的广告只会颁发令牌到已知的应用程序。 您可以从您的应用程序使用 Azure 广告之前，您需要在您的组织中为其创建一个条目。  在您组织中注册新的应用程序

-   登录到[Azure 的管理门户](https://manage.windowsazure.com)
-   在左侧导航中，单击**活动目录**
-   选择租户想要注册的应用程序。
-   单击**应用程序**选项卡，并单击**添加**在底部抽屉里。
-   跟随提示并创建新的**本机客户端应用程序**(尽管 Cordova 应用程序是基于 HTML，我们正在创建的本机客户端应用程序因此`Native Client Application`必须选择选项; 否则，应用程序将不起作用)。
    -   应用程序**名称**将介绍您给最终用户的应用程序
    -   **重定向 URI**是用来将令牌返回到您的应用程序的 URI。 输入`http://MyDirectorySearcherApp`。

完成注册后，AAD 会将您的应用程序分配一个唯一的客户机标识符。  您将需要在下一节中的此值︰ 您可以在新创建的应用程序的**配置**选项卡中找到它。

要运行`DirSearchClient Sample`，授予新创建的应用程序的权限在_Azure 广告图形 API_中查询︰
-   在**配置**选项卡上，找到"权限与其他应用程序"部分。  "Azure Active Directory"应用程序中，将添加**委派权限**下的**所在的目录中已登录的用户的访问**权限。  这将使您的应用程序用户在图形 API 中查询。

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2.克隆本教程所需的示例应用程序存储库*

从外壳或命令行，键入以下命令︰

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3.创建 Cordova 应用程序*

有创建 Cordova 应用程序的多种方法。 在本教程中我们将使用 Cordova 命令行界面 (CLI)。
从外壳或命令行，键入以下命令︰


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

将创建的文件夹结构和基架 Cordova 项目，在 www 子文件夹中复制的起始项目的内容。
移动到新的 DirSearchClient 文件夹。

    cd .\DirSearchClient

添加白名单插件所需的调用关系图的 API。

     cordova plugin add cordova-plugin-whitelist

接下来，添加所有您想要支持的平台。 为了使工作示例，您需要执行下面的命令中至少一个。 请注意，您将不能模拟 iOS 上窗口或 Windows/Windows Phone 上 mac。

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

最后，您可以到您的项目添加 Cordova 插件的 ADAL。

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3.添加代码来对用户进行身份验证并获得 AAD 的标记*

在本教程中开发的应用程序将提供的裸露骨目录搜索功能时，最终用户可以键入目录中的任何用户的别名，直观地显示一些基本特性。  起始项目包含定义的应用程序 （在 www/index.html) 的基本用户界面的基本应用程序事件连线基架周期，用户接口绑定和结果显示逻辑 （在 www/js/index.js)。 为您留的唯一的事情就是要添加标识任务实现的逻辑。

您需要做的第一件事是在代码中引入用于标识您的应用程序和您指定的目标的资源由 AAD 的协议值。 这些值将用于在以后构造令牌的请求。 Index.js 文件的顶部插入下面的代码段。

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

`redirectUri` ，`clientId`值应与描述您的应用程序中 AAD 的值相匹配。 为在步骤 1 中所述，在本教程中的前面部分中，您可以在 Azure 的门户中，找到那些从配置选项卡。
注意︰ 如果您选择不在您自己组织中注册新的应用程序，则可以仅粘贴上面的预配置的值时-，将允许您查看示例运行时，但应始终以用于生产应用程序中创建自己的条目。

接下来，我们需要添加实际令牌请求的代码。 插入下面的代码段之间`search `和`renderdata `定义。

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
让我们通过在其两个主要部分进行分解检查该函数。
此示例旨在处理的任何组织，而不是绑定到一个特定。 它使用"/ 常用"端点，它允许用户在身份验证时输入的任何帐户并将请求定向它属于租户。
第一部分的方法检查已存在存储的标记-，如果有，它使用来自用于重新初始化 ADAL 承租人的 ADAL 缓存。 这是需要避免额外的提示，使用"/ 常用"始终会要求用户输入一个新的帐户。
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
该方法的第二部分执行正确的 tokewn 请求。
`acquireTokenSilentAsync`方法要求对 ADAL 返回为指定的资源的标记，而不显示任何体验 如果缓存中已有存储，一个合适的访问令牌可以发生或者没有可以用于任何提示获取新的访问令牌，而 shwoing 的刷新令牌。
如果该尝试失败，我们求助`acquireTokenAsync`-其中明显将提示用户进行身份验证。
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
既然我们已有的标记，我们最后可以调用图形 API 而执行我们想要的搜索查询。 插入下面的代码段下方`authenticate`定义。

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
起始点文件提供用于在文本框中输入用户的别名的 barebone 用户体验。 此方法使用该值构造查询、 结合访问令牌，将其发送到关系图，并分析结果。 在起始点文件中，已有的 renderData 方法负责进行可视化结果。

## <a name="4-run"></a>*4.运行*
您的应用程序是最后准备好运行 ！ 运行它是非常简单︰ 一旦应用程序启动时，在文本框中输入想要查找的然后单击按钮的用户的别名。 系统将提示您进行身份验证。 成功的身份验证并成功搜索之后，将显示搜索的用户的属性。 随后再运行将执行搜索，而不显示任何提示，这要归功于以前获取的令牌缓存中存在。
运行应用程序的具体步骤因平台而异。

####<a name="windows-10"></a>Windows 10:

   Tablet/PC:`cordova run windows --archs=x64 -- --appx=uap`

   移动 （需要连接到 PC 的 Windows10 移动设备）︰`cordova run windows --archs=arm -- --appx=uap --phone`

   __注意__︰ 在第一次运行可能会要求您进行签名的开发人员许可证。 更多详细信息，请参阅[开发人员许可证](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx)。

####<a name="windows-81-tabletpc"></a>Windows 8.1 触电脑︰

   `cordova run windows`

   __注意__︰ 在第一次运行可能会要求您进行签名的开发人员许可证。 更多详细信息，请参阅[开发人员许可证](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx)。

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   在已连接的设备上运行︰`cordova run windows --device -- --phone`

   若要在默认模拟器上运行︰`cordova emulate windows -- --phone`

   使用`cordova run windows --list -- --phone`若要查看所有可用的目标和`cordova run windows --target=<target_name> -- --phone`若要在特定的设备或仿真器上运行的应用程序 (例如， `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`)。

####<a name="android"></a>Android:

   在已连接的设备上运行︰`cordova run android --device`

   若要在默认模拟器上运行︰`cordova emulate android`

   __注意__︰ 请确保您已经创建了使用*AVD 管理器*，因为它显示在*系统必备组件*部分的仿真程序实例。

   使用`cordova run android --list`若要查看所有可用的目标和`cordova run android --target=<target_name>`若要在特定的设备或仿真器上运行的应用程序 (例如， `cordova run android --target="Nexus4_emulator"`)。

####<a name="ios"></a>iOS:

   在已连接的设备上运行︰`cordova run ios --device`

   若要在默认模拟器上运行︰`cordova emulate ios`

   __注意__︰ 请确保您有`ios-sim`安装在仿真器上运行的包。 有关更多详细信息请参见*系统必备组件*一节。

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

使用`cordova run --help`查看附加的生成和运行选项。

为了便于参考，提供完整的示例 （无需您的配置值）[此处](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient)。  您可以立即将移动到更高级的 （好吧，和更有趣） 方案。  您可能想要尝试︰

[安全的 Node.js Web API 加装了 Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
