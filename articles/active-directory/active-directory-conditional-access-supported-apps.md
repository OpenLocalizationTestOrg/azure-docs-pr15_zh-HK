<properties
    pageTitle="在 Azure Active Directory 中使用条件访问规则的应用程序 |Microsoft Azure"
    description="有条件的访问控制，Azure Active Directory 检查特定条件时对用户进行身份验证，并允许应用程序访问。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>在 Azure Active Directory 中使用条件访问规则的应用程序

在 Azure 活动目录 (AD Azure) 支持条件访问规则-连接的应用程序、 预集成的联合的软件作为服务 (SaaS) 应用程序，并使用密码单一登录 (SSO)、 业务线应用程序，并使用 Azure AD 应用程序代理的应用程序的应用程序。 可以使用条件访问应用程序的详细列表，请参阅[启用条件访问服务](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access)。 条件接收工作都具有移动和桌面应用程序，使用现代的身份验证。 在本文中，我们介绍移动和桌面应用程序中的条件如何访问工作。

您可以使用现代的身份验证的应用程序中使用 Azure 广告登录页。 用登录页面，提示用户输入多因素身份验证。 如果用户的访问被阻止显示一条消息。 需要对该设备进行身份验证使用 Azure 的广告，现代的身份验证，以便评估基于设备的条件访问策略。

请务必了解哪些应用程序可以使用的条件访问规则，并且您可能需要为其他应用程序的入口点的保护而采取的步骤。

## <a name="applications-that-use-modern-authentication"></a>使用现代的身份验证的应用程序

> [AZURE.NOTE] 如果 Office 365 中都有等效的 Azure AD 中有条件接收策略，配置这两个条件的访问策略。 这将应用，例如，对于 Exchange 联机或 SharePoint Online 的条件访问策略。

下面的应用程序支持条件访问 Office 365 和其他 Azure AD 连接的服务应用程序︰

| 目标服务  | 平台  | 应用程序                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange 联机 | Windows 10|邮件/日历/用户应用程序，Outlook 2016，Outlook 2013 （与现代身份验证），Skype 的业务 （与现代身份验证）|
|Office 365 Exchange 联机| Windows 8.1，Windows 7 |Outlook 2016，Outlook 2013 （与现代身份验证），Skype 的业务 （与现代身份验证）|
|Office 365 Exchange 联机|iOS Android|  Outlook 的移动应用程序|
|Office 365 Exchange 联机|Mac OS X| Outlook 2016 年为多因素身份验证和位置未来，Skype 未来计划的业务支持基于设备的策略支持计划|
|Office 365 SharePoint 在线|Windows 10| Office 2016 应用程序、 通用的 Office 应用程序，Office 2013 （使用现代身份验证），OneDrive （下一代同步客户端或 NGSC） 的业务应用程序支持的未来计划，计划未来，而未来计划的 SharePoint 应用程序支持 Office 组支持|
|Office 365 SharePoint 在线|Windows 8.1，Windows 7|（与现代身份验证） 的 Office 2013、 业务应用程序 （Groove 同步客户端） 的 OneDrive office 2016 应用程序|
|Office 365 SharePoint 在线|iOS Android|  办公室的移动应用程序 |
|Office 365 SharePoint 在线|Mac OS X| 多因素身份验证和位置; office 2016 应用程序基于设备的策略支持未来计划|
|Office 365 Yammer|Windows 10、 iOS 和 Android | Office Yammer 应用程序|
|Dynamics CRM|Windows 10、 Windows 8.1、 Windows 7，iOS 和 Android | Dynamics CRM 应用程序|
|PowerBI 服务|Windows 10、 Windows 8.1、 Windows 7，iOS 和 Android | PowerBI 应用程序|
|Azure 的远程应用程序服务|Windows 10、 Windows 8.1、 Windows 7，iOS、 Android 和 Mac OS X |Azure 的远程应用程序|
|我的应用程序的任何应用程序服务|Android 和 iOS|我的应用程序的任何应用程序服务 |


## <a name="applications-that-do-not-use-modern-authentication"></a>应用程序不使用现代的身份验证

目前，您必须使用其他方法来阻止的应用程序不使用现代的身份验证访问。 条件接收不会强制执行应用程序不使用现代的身份验证的访问规则。 这主要是交换和 SharePoint 访问的注意事项。 大多数较早版本的应用程序使用较旧的访问控制协议。

### <a name="control-access-in-office-365-sharepoint-online"></a>在 Office 365 SharePoint Online 控制访问
您可以通过设置 SPOTenant cmdlet 禁用 SharePoint 访问的旧协议。 使用此 cmdlet 以防止 Office 客户端使用访问 SharePoint Online 资源非现代身份验证协议。

**示例命令**︰    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>在 Office 365 在线交换控制访问

Exchange 提供两种主要的协议。 查看以下选项，然后选择最适合您的组织的策略。

-   **Exchange ActiveSync**。 默认情况下，不强制 Exchange ActiveSync 执行的多因素身份验证和位置的条件访问策略。 您需要通过配置 Exchange ActiveSync 策略直接，或通过使用 Active Directory 联合身份验证服务 (AD FS) 规则阻止 Exchange ActiveSync 保护对这些服务的访问。
-   **旧协议**。 您可以阻止使用 AD FS 的旧协议。 以前的 Office 客户端，如现代身份验证已启用，而 Office 早期版本的 Office 2013 到此阻止访问。


### <a name="use-ad-fs-to-block-legacy-protocol"></a>使用 AD FS 可以阻止旧协议

可以使用以下示例规则以阻止旧协议访问 AD FS 级别。 从两种常见的配置选择。

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>选择 1︰ 允许 Exchange ActiveSync 并允许旧式应用程序，但只能在 intranet 上

通过对 AD FS 配置成信赖方信任的 Microsoft Office 365 标识平台应用下面的三个规则，Exchange ActiveSync 通信和浏览器和现代身份验证通信，具有访问权限。 旧式应用程序阻止外部网。

##### <a name="rule-1"></a>规则 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>规则 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>规则 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>选项 2︰ 允许 Exchange ActiveSync 和阻止旧版应用程序

通过对 AD FS 配置成信赖方信任的 Microsoft Office 365 标识平台应用下面的三个规则，Exchange ActiveSync 通信和浏览器和现代身份验证通信，具有访问权限。 旧式应用程序被阻止从任何位置。

##### <a name="rule-1"></a>规则 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>规则 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>规则 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
