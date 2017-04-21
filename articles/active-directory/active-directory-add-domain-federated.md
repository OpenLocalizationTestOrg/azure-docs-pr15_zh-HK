<properties
    pageTitle="添加您的自定义域名和设置上到 Azure Active Directory 联合登录 |Microsoft Azure"
    description="如何将您公司的域名添加到 Azure Active Directory 和如何设置 Azure Active Directory 和内部部署联合解决方案之间的联盟登录。"
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
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>将您的自定义域名添加到 Azure 活动目录

您可以配置一个自定义的域名，如 'contoso.com'，以便 contoso.com 中的用户可以拥有联盟单一登录体验从您的企业网络。 如果您已经有了 Active Directory 联合身份验证服务 (AD FS) 或公司网络上运行的不同联合身份验证服务器，您可以配置 Azure 的广告，若要使用自定义域名使用 Azure AD 连接工具。 此外可以使用 Azure AD 连接部署一个新的 AD FS 环境，并为联盟单一登录到 Azure AD 中配置的。

如果没有，并且不打算部署 AD FS 或另一个联合服务器，请按照以下说明︰[添加到 Azure Active Directory 的自定义域名称](active-directory-add-domain.md)。

## <a name="add-a-custom-domain-name-to-your-directory"></a>自定义域名添加到目录

1. 登录到[Azure 的传统门户网站](https://manage.windowsazure.com/)使用 Azure 的广告目录作为全局管理员用户帐户。

2. 在**Active Directory**中，打开您的目录，然后选择**域**选项卡。

3. 在命令栏上，选择**添加**，然后输入您自定义的域，如 'contoso.com' 的名称。 请务必包括.com、.net 或其他顶级的扩展。

4. 选择**我计划配置为单一登录我本地 Active Directory 与此域**复选框。

5. 选择**添加**。

运行 Azure AD 连接工具来获取 Azure 广告将用于验证域的 DNS 条目。 您将看到在**Azure AD 域**步向导中的 DNS 条目。 您可以查看哪些内容该向导中的步骤如下所示[在这些指令中](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation)。 如果您没有 Azure AD 连接工具，您可以[从此处下载](http://go.microsoft.com/fwlink/?LinkId=615771)。

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>添加域域名注册的域的 DNS 条目

您的自定义域名使用 Azure AD 的下一步是更新域的 DNS 区域文件。 这使 Azure 的广告，以验证您的组织拥有自定义域名。

1. 登录到的域的域名的域名注册网站。 如果您没有执行此操作的权限，要求的个人或团队已完成步骤 2，并让您知道，当它完成时此访问您组织中。

2. 通过添加 DNS 条目的 Azure 广告提供给您更新域的 DNS 区域文件。 此 DNS 条目使 Azure 的广告，以验证您的域所有权。 DNS 条目不会更改任何行为如邮件路由或 web 托管。

有关此步骤的帮助，请参阅[添加 DNS 条目在流行的 DNS 注册机构的说明](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>验证使用 Azure AD 的域名

添加 DNS 条目后，您就可以验证的域名使用 Azure 的广告。

要验证域，请选择**下一步**在 Azure AD 连接向导的**Azure AD 域**的步骤。 Azure 的广告会查找 DNS 区域文件中的域的 DNS 条目。 Azure 的广告传播的 DNS 记录后仅验证的域名。 传播通常只需几秒钟，但它有时需要花一个小时或更多。 如果验证不起作用第一次，请稍后再试。

然后，继续 Azure AD 连接向导中的其余步骤。 这将同步用户从 Windows 服务器 AD Azure 的广告。 同步您为联盟配置为域中的用户将能够获得到 Azure 广告您的企业网络联盟单一登录体验。

## <a name="troubleshooting"></a>故障排除

如果您无法验证自定义域名，请尝试以下方法。 我们将开始与最常见并到最常见的工作。

1.  **等待一个小时**。 需要传播之前 Azure 广告可以验证域 DNS 记录。 这可能需要一个小时或更多。

2.  **确保输入的 DNS 记录，并且是正确**。 完成域的域域名注册网站上的这一步。 如果 DNS 条目不在 DNS 区域文件，或者如果未使用 DNS 条目完全匹配该 Azure 广告提供您，azure 的广告不能验证的域名。 如果您没有访问权限来更新域的域名注册域的 DNS 记录，DNS 条目向个人或团队组织中具有此访问权限，并要求他们要添加 DNS 条目。

3.  **删除在 Azure 广告中另一个目录中的域名**。 域名可以验证单个目录中。 如果域名以前已验证另一个目录中，必须那里删除之前它可以验证新目录中。 若要了解有关删除域名，阅读[管理自定义域名](active-directory-add-manage-domain-names.md)。

## <a name="add-more-custom-domain-names"></a>添加多个自定义域名

如果您的组织使用多个自定义域名称，如 'contoso.com' 和 'contosobank.com'，可以最多 900 域名来添加它们。 在这篇文章中使用相同的步骤添加每个您的域名。

## <a name="next-steps"></a>下一步行动

-   [管理自定义域名](active-directory-add-manage-domain-names.md)
-   [在 Azure 广告了解有关管理域的概念](active-directory-add-domain-concepts.md)
-   [显示您的公司的品牌在用户登录时](active-directory-add-company-branding.md)
-   [使用 PowerShell 在 Azure AD 管理域名称](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
