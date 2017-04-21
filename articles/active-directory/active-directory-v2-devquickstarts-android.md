<properties
    pageTitle="Azure 的 Active Directory 2.0 版 Android 应用程序 |Microsoft Azure"
    description="如何构建一个 Android 的应用程序，在与 Microsoft 帐户和工作或学校帐户和个人图形 API 调用的用户通过使用第三方函数库进行签名。"
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>签名中添加图形 API 使用 v2.0 终结点与使用第三方库的 Android 应用程序

Microsoft 身份平台使用开放标准，如 OAuth2 和 OpenID 连接。 开发人员可以使用任何他们想要与我们的服务集成在一起的库。 为了帮助开发人员与其他库使用我们的平台，我们编写了这个例子来演示如何配置以连接到 Microsoft 身份平台的第三方库的几个演练。 大多数实现[RFC6749 OAuth2 规范](https://tools.ietf.org/html/rfc6749)的库可以连接到 Microsoft 身份平台。

使用本演练中创建的应用程序，用户可以登录到他们的组织，然后搜索本身在其组织中使用图形 API。

如果您是新手，OAuth2 还是 OpenID 连接，此示例配置大部分可能没有意义给您。 我们建议您阅读[2.0 协议-OAuth 2.0 授权代码流](active-directory-v2-protocols-oauth-code.md)为背景。

> [AZURE.NOTE] 我们的平台在 OAuth2 或连接 OpenID 标准，如条件接收和 Intune 策略管理，具有表达式的某些功能需要您使用我们开源 Microsoft Azure 标识库。

V2.0 终结点不支持所有 Azure Active Directory 方案和功能。

> [AZURE.NOTE] 要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。


## <a name="download-the-code-from-github"></a>下载从 GitHub 代码
本教程中的代码[在 GitHub 上](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2)维护。  要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip)或克隆主干︰

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

您也可以下载示例并立即开始︰

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>注册应用程序
在[应用程序注册门户](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)，创建新的应用程序或按照如何[注册使用 2.0 版的终结点的应用程序](active-directory-v2-app-registration.md)的详细的步骤。  请确保︰

- 复制**应用程序 Id** ，因为您很快将需要分配给您的应用程序。
- 添加您的应用程序的**移动**平台。

> 注意︰ 应用程序注册门户提供**重定向 URI**值。 但是，在此示例中必须使用的默认值`https://login.microsoftonline.com/common/oauth2/nativeclient`。


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>下载 NXOAuth2 第三方库和创建工作区

对于本演练，您将使用从 GitHub，是基于 Google 的 OpenID 连接代码 OAuth2 库 OIDCAndroidLib。 它实现的本机应用程序配置文件，并支持用户的授权终结点。 这些是您将需要与 Microsoft 身份平台集成的所有事情。

克隆 OIDCAndroidLib repo 到您的计算机。

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>设置 Android Studio 环境

1. 创建一个新的 Android Studio 项目并接受向导中的默认值。

    ![在 Android Studio 中创建新项目](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Android 的目标设备](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![将活动添加到手机](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. 若要设置项目模块，将克隆的 repo 移到项目位置。 您还可以创建项目，然后克隆它直接到项目位置。

    ![项目模块](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. 通过使用上下文菜单或通过使用 Ctrl + Alt + Maj + S 快捷方式，请打开项目模块设置。

    ![项目模块设置](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. 因为您只希望项目容器设置，请删除默认的应用程序的模块。

    ![默认的应用程序的模块](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. 从克隆 repo 模块导入当前项目中。

    ![导入 gradle 项目](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![创建新模块页面](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. 重复上述步骤`oidlib-sample`模块。

7. Oidclib 依赖项检查`oidlib-sample`模块。

    ![oidclib oidlib 示例模块的依赖关系](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. 单击**确定**并等待 gradle 同步。

    您 settings.gradle 应如下所示︰

    ![Settings.gradle 的屏幕抓图](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. 构建示例应用程序，以确保正确运行此示例。

    您不能还与 Azure Active Directory 使用此。 我们需要先配置某些终结点。 这是为了确保我们开始自定义示例应用程序之前没有 Android Studio 问题。

10. 生成并运行`oidlib-sample`作为 Android Studio 中的目标。

    ![Oidlib 示例生成的进度](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. 删除`app `被保留当从项目中移除模块，因为 Android Studio 并不会删除它的安全的目录。

    ![文件结构，其中包含应用程序目录](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. 打开**编辑配置**菜单删除从项目中移除该模块时也保持运行的配置。

    ![编辑菜单配置](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![运行的应用程序的配置](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>配置终结点的示例

现在，您已经有`oidlib-sample`运行成功，让我们来编辑某些终结点来获取此使用 Azure Active Directory。

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>通过编辑 oidc_clientconf.xml 文件来配置您的客户端

1. 仅使用 OAuth2 流来获取标记并调用图形 API，因为设置客户机只进行 OAuth2。 OIDC 会在后面的示例。

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. 配置您收到注册门户的客户端 ID。

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. 将您重定向 URI 配置一下面。

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. 配置作用域所需访问图形 API。

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

`User.Read`中的值`oidc_scopes`允许您读取基本配置文件的用户签名。
您可以了解有关[Microsoft Graph 权限](https://graph.microsoft.io/docs/authorization/permission_scopes)范围的可用作用域的详细信息。

如果您想解释有关`openid`或`offline_access`作为在 OpenID 连接的作用域，请参阅[2.0 协议-OAuth 2.0 授权代码流](active-directory-v2-protocols-oauth-code.md)。

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>通过编辑 oidc_endpoints.xml 文件来配置客户端端点

- 打开`oidc_endpoints.xml`文件并进行以下更改︰

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

如果您使用的 OAuth2 作为协议绝对不要更改这些终结点。

> [AZURE.NOTE]
终结点为`userInfoEndpoint`和`revocationEndpoint`Azure Active Directory 目前不支持。 如果您保留这些默认 example.com 值，将会得到提示，它们并不具有在该示例中:-)


## <a name="configure-a-graph-api-call"></a>配置的图形 API 调用

- 打开`HomeActivity.java`文件并进行以下更改︰

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

这里简单的图形 API 调用返回我们的信息。

这些都是您需要做的所有更改。 运行`oidlib-sample`应用程序，然后单击**登录**。

您已经成功通过身份验证后，选择**请求受保护的资源**按钮以测试图形 API 调用。

## <a name="get-security-updates-for-our-product"></a>我们的产品中获得安全更新

我们鼓励您访问[安全技术中心](https://technet.microsoft.com/security/dd252948)和安全通报警报订阅获得有关安全事件的通知。
