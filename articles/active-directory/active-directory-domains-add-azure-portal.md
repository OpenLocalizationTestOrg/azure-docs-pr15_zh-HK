<properties
    pageTitle="将您的自定义域名添加到 Azure Active Directory 预览 |Microsoft Azure"
    description="如何将您公司的域名添加到 Azure Active Directory，以及如何验证的域名。"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>将自定义域名添加到 Azure Active Directory 预览

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-domains-add-azure-portal.md)
- [Azure 的传统门户网站](active-directory-add-domain.md)

您有一个或多个域名，您的组织使用的业务，和您的用户登录到公司网络使用您公司域的名称。 使用 Azure 活动目录 (AD Azure) 预览，您可以到 Azure AD 也添加您公司域的名称。 [在预览中是什么？](active-directory-preview-explainer.md) 这使您可以指定目录中的用户名称所熟悉的用户，如‘alice@contoso.com.’过程很简单︰

1. 自定义域名添加到目录
2. 添加域的域名注册的域名的 DNS 条目
3. 在 Azure AD 验证的自定义域名称

## <a name="how-do-i-add-a-domain-name"></a>如何添加域名？

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**Azure Active Directory** ，然后选择**输入**。

    ![打开用户管理](./media/active-directory-domains-add-azure-portal/user-management.png)

3. ***目录名称***刀片式服务器，选择**域名**。

4. ***目录名称*的域名**刀片式服务器，选择**添加**命令。

  ![选择添加命令](./media/active-directory-domains-add-azure-portal/add-command.png)

5. 在**域名**刀片式服务器，在 'contoso.com' 对话框中输入您自定义的域的名称，然后选择**添加域**。 请务必包括.com、.net 或其他顶级的扩展。

6. 在***域名***刀片式服务器 （即，刀片式服务器打开标题中具有新的域名），获得 Azure 广告将使用它来验证您的组织拥有自定义域名的 DNS 条目信息。

  ![获取 DNS 条目的信息](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

现在，您已经添加的域名，Azure 广告必须验证您的组织拥有的域名。 Azure 的广告执行此验证之前，必须添加 DNS 条目的域名的 DNS 区域文件中。 域的域名的域名注册网站上执行此任务。

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>添加域域名注册的域的 DNS 条目

您的自定义域名使用 Azure AD 的下一步是更新域的 DNS 区域文件。 这使 Azure 的广告，以验证您的组织拥有自定义域名。

1.  登录到的域的域名称注册器。 如果您没有权限更新 DNS 条目，询问的人或已完成步骤 2，并让您知道，当它完成时此访问权限的团队。

2.  通过添加 DNS 条目的 Azure 广告提供给您更新域的 DNS 区域文件。 此 DNS 条目使 Azure 的广告，以验证您的域所有权。 DNS 条目不会更改任何行为如邮件路由或 web 托管。

此添加 DNS 条目的帮助，请参阅[添加 DNS 条目在流行的 DNS 注册的说明](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>验证使用 Azure AD 的域名

添加 DNS 条目后，您就可以验证的域名使用 Azure 的广告。

传播的 DNS 记录后，才可以验证域名。 此传播通常只需几秒钟，但它有时需要花一个小时或更多。 如果验证不起作用第一次，请稍后再试。

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**浏览**，在文本框中，输入用户管理，然后选择**输入**。

    ![打开用户管理](./media/active-directory-domains-add-azure-portal/user-management.png)

3. 在**用户管理-域名**刀片式服务器，选择您想要验证的未经验证的域名称。

4. 在***域名***刀片式服务器 （即，刀片式服务器打开标题中具有新的域名），选择**验证**以完成验证。

现在，您可以[指定用户名称，包括您的自定义域名](active-directory-users-create-azure-portal.md)。

## <a name="troubleshooting"></a>故障排除

如果您无法验证自定义域名，请尝试以下方法。 我们将开始与最常见并到最常见的工作。

1.  **等待一个小时**。 需要传播之前 Azure 广告可以验证域 DNS 记录。 这可能需要一个小时或更多。

2.  **确保输入的 DNS 记录，并且是正确**。 完成域的域域名注册网站上的这一步。 如果 DNS 条目不在 DNS 区域文件，或者如果未使用 DNS 条目完全匹配该 Azure 广告提供您，azure 的广告不能验证的域名。 如果您没有访问权限来更新域的域名注册域的 DNS 记录，DNS 条目向个人或团队组织中具有此访问权限，并要求他们要添加 DNS 条目。

3.  **删除在 Azure 广告中另一个目录中的域名**。 域名可以验证单个目录中。 如果域名以前已验证另一个目录中，必须那里删除之前它可以验证新目录中。 若要了解有关删除域名，阅读[管理自定义域名](active-directory-domains-manage-azure-portal.md)。    

## <a name="add-more-custom-domain-names"></a>添加多个自定义域名

如果您的组织使用多个自定义域名称，如 'contoso.com' 和 'contosobank.com'，可以最多 900 域名来添加它们。 在这篇文章中使用相同的步骤添加每个您的域名。

## <a name="next-steps"></a>下一步行动

[管理自定义域名](active-directory-domains-manage-azure-portal.md)
