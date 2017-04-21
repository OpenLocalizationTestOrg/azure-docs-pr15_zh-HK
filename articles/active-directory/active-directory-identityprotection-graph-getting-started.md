<properties
    pageTitle="开始使用 Azure 活动目录身份保护和 Microsoft Graph |Microsoft Azure"
    description="从 Azure Active Directory 提供风险事件和相关的信息的列表查询 Microsoft Graph 的简介。"
    services="active-directory"
    keywords="azure 的活动目录标识保护、 风险事件、 漏洞、 Microsoft Graph 的安全策略"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>开始使用 Azure 活动目录身份保护和 Microsoft Graph

Microsoft Graph 是 Microsoft 的统一的 API 端点和[Azure 活动目录身份保护的](active-directory-identityprotection.md)Api 的家。 我们第一组 API， **identityRiskEvents**，可查询 Microsoft Graph 的[风险事件](active-directory-identityprotection-risk-events-types.md)和相关的信息的列表。 这篇文章获取您开始查询此 API。 活动，深入介绍、 完整文档和访问图形浏览器，请参阅[Microsoft Graph 站点](https://graph.microsoft.io/)。

有身份保护数据访问 Microsoft Graph 通过三个步骤︰

1. 添加具有客户端密码的应用程序。 

2. 使用该密码和其他几条信息向 Microsoft Graph，收到了身份验证令牌进行身份验证。 

3. 使用此标记请求 API 端点并重新获取标识保护数据。

在开始之前，您需要︰

- 在 Azure AD 中创建应用程序的管理员权限
- 租户的域 (例如，contoso.onmicrosoft.com) 的名称



## <a name="add-an-application-with-a-client-secret"></a>添加具有客户端密码的应用程序


1. Azure 传统门户网站以管理员身份[登录](https://manage.windowsazure.com)。 

1. 在左侧的导航窗格中，单击**活动目录**。 

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 在菜单上，单击**应用程序**。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. 单击页面底部的**添加**。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. 在**您想要执行**对话框中，单击**添加应用程序正在开发我的组织**。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. 在**告诉我们有关您的应用程序**对话框中，请执行以下步骤︰

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    一。 在**名称**文本框中，键入您的应用程序的名称 (例如︰ AADIP 风险事件 API 应用程序)。

    b。 作为**类型**，选择**Web 应用程序和/或 Web API**。

    c。 单击**下一步**。


5. 在**应用程序属性**对话框中，请执行以下步骤︰

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    一。 在**登录 URL**文本框中，键入`http://localhost`。

    b。 在**应用程序 ID URI**文本框中，键入`http://localhost`。

    c。 单击**完成**。


现在，您可以配置您的应用程序。

![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>授予您使用 API 的应用程序权限


1. 在您的应用程序页上，在菜单上，单击**配置**。 

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. 在**其他应用程序的权限**部分中，单击**添加应用程序**。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. 在**其他应用程序的权限**对话框中，请执行以下步骤︰

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    一。 选择**Microsoft Graph**。

    b。 单击**完成**。


1. 单击**应用程序权限︰ 0**，然后选择**读取标识风险事件的所有信息**。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. 单击页面底部的**保存**。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>获取一个访问键

1. 在您的应用程序页上，**密钥**部分中，在中，选择持续时间为 1 年。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. 单击页面底部的**保存**。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. 在键部分中，复制您新创建的项的值，然后将其粘贴到一个安全的地方。

    ![创建应用程序](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] 如果此注册表项丢失，必须返回到本节并创建新的密钥。 此项保密︰ 它的人可以访问您的数据。


1. 在**属性**部分中，复制**客户机 ID**，，然后将其粘贴到一个安全的地方。 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>通过身份验证的 Microsoft Graph 并查询标识风险事件 API

此时，您应该︰

- 复制上面的客户端 ID

- 复制上面的键

- 租户的域的名称


要进行身份验证，发送 post 请求到`https://login.microsoft.com`与正文中的下列参数︰

- grant_type:"**client_credentials**"

- 资源:"**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] 您需要为**client_id**和**client_secret**参数提供值。

如果成功，这将返回一个身份验证标记。  
若要调用该 API，请使用以下参数创建页眉︰

    `Authorization`=”<token_type> <access_token>"


当身份验证，您可以找到标记类型和访问令牌返回标记中。

发送到下面的 API URL 请求此标头︰`https://graph.microsoft.com/beta/identityRiskEvents`

响应，如果成功，则标识风险事件和 OData JSON 格式，它可以分析并处理可以根据相关联的数据的集合。

下面是用于验证身份和调用使用 Powershell 的 API 的示例代码。  
只需添加您的客户端 ID 密钥，然后租户域。

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>下一步行动

祝贺您，您刚才所做的第一个调用 Microsoft Graph ！  
现在您可以查询标识风险事件并使用这些数据，但是您的习惯。

更多地了解 Microsoft Graph 以及如何构建应用程序使用图形 API，签出[文档](https://graph.microsoft.io/docs)和[Microsoft Graph 站点](https://graph.microsoft.io/)上的更多。 另外，请确保列出了所有可用图中标识保护 Api [Azure AD 身份保护 API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)页面加上书签。 我们添加新的方法来处理通过 API 的身份保护，您将看到它们在该页面上。


## <a name="additional-resources"></a>其他资源

- [Azure 的 Active Directory 身份保护](active-directory-identityprotection.md)

- [检测到的 Azure 活动目录身份保护的风险事件的类型](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Microsoft Graph 的概述](https://graph.microsoft.io/docs)

- [Azure 广告标识保护服务根](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
