<properties
    pageTitle="Azure AD 连接同步︰ 如何管理 Azure AD 服务帐户 |Microsoft Azure"
    description="本主题介绍如何还原 Azure AD 服务帐户。"
    services="active-directory"
    keywords="AADSTS70002、 AADSTS50054，如何重置 Azure AD 连接同步连接器服务帐户的密码"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD 连接同步︰ 如何管理 Azure AD 服务帐户
Azure AD 连接器使用的服务帐户应该是免费的服务。 如果您需要重置其凭据，本主题适用于您。 例如，如果错误具有全局管理员重置使用 PowerShell 的服务帐户的密码。

## <a name="reset-the-credentials"></a>重置凭据
如果定义 Azure AD 连接器上的服务帐户不能联系 Azure 广告由于身份验证问题，可以重置密码。

1. 登录到 Azure AD 连接同步服务器并启动 PowerShell。
2. 运行`Add-ADSyncAADServiceAccount`。  
![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. 提供 Azure AD 全局管理员凭据。

此 cmdlet 重置服务帐户的密码，并在 Azure 的 AD 和同步引擎中对其进行更新。

## <a name="known-issues-these-steps-can-solve"></a>这些步骤可以解决的已知的问题
这一节是由已修复的 Azure AD 服务帐户上重设凭据的客户所报告的错误的列表。

-----------
事件 6900  
在处理密码更改通知时，服务器遇到错误︰  
AADSTS70002︰ 错误验证凭据。 AADSTS50054︰ 旧密码用于身份验证。

----------
事件 659  
检索密码策略同步配置时出错。 Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002︰ 错误验证凭据。 AADSTS50054︰ 旧密码用于身份验证。

## <a name="next-steps"></a>下一步行动

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
