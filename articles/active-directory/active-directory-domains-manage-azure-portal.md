<properties
    pageTitle="在 Azure Active Directory 预览自定义域名管理 |Microsoft Azure"
    description="管理概念和管理 Azure Active Directory 中的域名"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>管理自定义域名在 Azure Active Directory 预览

域名是许多目录资源标识符的一个重要部分︰ 它是用户组的地址的一部分用户名或电子邮件地址的一部分并可以是应用程序的应用程序 ID URI 的一部分。 在 Azure 活动目录 (AD Azure) 预览资源可以包含已验证包含该资源的目录拥有一个域名。 [在预览中是什么？](active-directory-preview-explainer.md) 只有全局管理员可以在 Azure AD 中执行域管理任务。

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>设置 Azure 的广告目录的主域名

当创建您的目录时，初始的域名，如 contoso.onmicrosoft.com，也是主域名。 主要域是默认域的名称为新用户创建新用户时。 这简化了管理员可以在门户中创建新用户的过程。 若要更改主域名︰

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**Azure Active Directory** ，然后选择**输入**。

    ![打开用户管理](./media/active-directory-domains-add-azure-portal/user-management.png)

3. ***目录名称***刀片式服务器，选择**域名**。

4. ***目录名称*的域名**刀片式服务器，选择您想要创建的主域名的域名称。

5.  在***域名***刀片式服务器 （即，刀片式服务器打开标题中具有新的域名），选择**设为主要**命令。 确认您的选择在提示时。

    ![使主域名](./media/active-directory-domains-manage-azure-portal/make-primary.png)

您可以更改您的目录将不联合任何经过验证的自定义域的主域名。 更改您的目录的主要域不会更改任何现有用户的用户名。

## <a name="add-custom-domain-names-to-your-azure-ad"></a>将自定义域名添加到 Azure 广告

对于每个 Azure 的广告目录可以达 900 自定义域名。 要添加[附加的自定义域名](active-directory-domains-add-azure-portal.md)的过程是相同的第一个自定义域名。

## <a name="add-subdomains-of-a-custom-domain"></a>添加自定义的域的子域

如果您想要添加到目录的三级域名如 europe.contoso.com，您应首先添加并验证第二层域，如 contoso.com。 该子域将自动验证通过 Azure 的广告。 若要查看您刚刚添加子域已经过验证，刷新页面在浏览器中所列出的域。

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>如果您更改为您自定义的域的名称的 DNS 注册，怎么办

如果更改自定义域名称的 DNS 注册，您可以继续您的自定义域名使用 Azure 广告本身无需中断和其他配置任务。 如果您使用您的自定义域名与 Office 365、 Intune 或依赖自定义域名在 Azure AD 中的其他服务，请参阅有关这些服务的文档。

## <a name="delete-a-custom-domain-name"></a>删除自定义域名

如果您的组织不能再使用该域名，或者您需要另一个 Azure 的广告中使用该域名，您可以从 Azure 广告中删除自定义域名。

若要删除一个自定义域名称，必须首先确保您的目录中没有资源依赖的域名。 如果不能从目录中删除的域名称︰

-   任何用户都用户名、 电子邮件地址或包含域名的代理服务器地址。

-   任何组具有一个电子邮件地址或包含域名的代理服务器地址。

-   Azure 广告的任何应用程序都有一个应用程序 ID URI 包含域名称。

必须更改或删除自定义域名之前 Azure 的广告目录中删除任何此类资源。

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>使用 PowerShell 或图形 API 来管理域名称

也可以使用 Microsoft PowerShell 或 （在公共预览） 使用 Azure 广告图形 API 以编程方式完成域名 Azure Active Directory 中的大多数管理任务。

-   [使用 PowerShell 在 Azure AD 管理域名称](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [使用图形 API 在 Azure AD 管理域名称](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>下一步行动

-   [添加自定义域名](active-directory-domains-add-azure-portal.md)
