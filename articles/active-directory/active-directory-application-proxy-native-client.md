<properties
    pageTitle="如何启用发布功能的本机客户端与代理服务器应用程序的应用程序 |Microsoft Azure"
    description="介绍如何启用本机客户端应用程序与 Azure AD 应用程序代理服务器连接器，以提供安全远程访问您的内部部署的应用程序进行通信。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>如何启用本机客户端应用程序与代理服务器应用程序进行交互

Azure 的活动目录应用程序代理广泛用于发布如 SharePoint、 Outlook Web Access 和自定义业务线应用程序的浏览器应用程序。 它还用于发布 web 应用程序与不同，因为它们安装在设备上本机客户端应用程序。 这是通过支持 Azure 的广告发布标准授权 HTTP 标头中发送的标记。

![最终用户、 Azure Active Directory 和发布的应用程序之间的关系](./media/active-directory-application-proxy-native-client/richclientflow.png)

若要发布此类应用程序的推荐的方法是使用 Azure 的 AD 身份验证库，负责所有身份验证麻烦并支持许多不同的客户端环境。 应用程序代理服务器适合于[本机应用程序与 Web API 方案](active-directory-authentication-scenarios.md#native-application-to-web-api)。 实现此目的的过程是，如下所示︰

## <a name="step-1-publish-your-application"></a>步骤 1︰ 将发布您的应用程序

发布代理应用程序，就像任何其他应用程序、 为用户分配和授予其高级或基本许可证。 有关详细信息，请参阅[应用程序代理的发布应用程序](active-directory-application-proxy-publish.md)。

## <a name="step-2-configure-your-application"></a>步骤 2︰ 配置您的应用程序

本机应用程序按如下配置︰

1. 登录到 Azure 的传统门户网站。
2. 选择左侧菜单上的 Active Directory 图标，然后选择您的目录。
3. 在菜单上，单击**应用程序**。 如果没有应用程序已添加到您的目录，此页将只显示**添加应用程序**链接。 单击该链接，或者您可以单击命令栏上的**添加**按钮。
4. 在**您想要执行**页上，单击添加**我的公司正在开发的应用程序**的链接。
5. **告诉我们有关您的应用程序**页上为您的应用程序指定一个名称，选择**本机客户端应用程序**。 单击箭头图标，以继续。
6. 在**应用程序信息**页上提供**重定向 URI**的本机客户端应用程序，然后单击复选标记来完成。

已添加您的应用程序，并且您将被带到快速启动页为您的应用程序。

## <a name="step-3-grant-access-to-other-applications"></a>步骤 3︰ 其他应用程序授予访问权限

启用本机应用程序暴露给其他应用程序在您的目录中︰

1. 在顶部的菜单中，单击**应用程序**，选择新的本机应用程序，然后单击**配置**。
2. 向下滚动到**到其他应用程序的权限**部分。 单击**添加应用程序**按钮选择您想要授予本机应用程序访问权限的代理应用程序并单击右下角中的复选标记。 从**委派权限**下拉菜单中，选择新的权限。

![添加到其他应用程序的屏幕快照的权限的应用程序](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>步骤 4︰ 编辑活动目录身份验证库

本机应用程序中编辑代码的验证上下文的活动目录身份验证库 (ADAL) 包括以下︰

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

变量替换，如下所示︰

- 右后"/ 目录 /"， **TenantId**可以找到在应用程序的**配置**页面，该 URL 中的 GUID。
- **前端 URL**是前端 URL 输入代理服务器应用程序和代理应用程序的**配置**页面上可以找到。
- 本机应用程序的**配置**页面上找不到的本机应用程序的**客户端 Id** 。
- 本机应用程序的**配置**页面上找不到**本机应用程序的重定向 URI** 。

![新的本机应用程序配置页屏幕抓图](./media/active-directory-application-proxy-native-client/new_native_app.png)

有关本机应用程序流的详细信息，请参见[本机应用程序与 web API](active-directory-authentication-scenarios.md#native-application-to-web-api)。


## <a name="see-also"></a>请参见

- [发布应用程序使用您自己的域名](active-directory-application-proxy-custom-domains.md)
- [启用条件访问](active-directory-application-proxy-conditional-access.md)
- [使用声明感知应用程序](active-directory-application-proxy-claims-aware-apps.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
