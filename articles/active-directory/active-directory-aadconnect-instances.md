<properties
    pageTitle="Azure AD 连接︰ 同步服务实例 |Microsoft Azure"
    description="此页介绍了 Azure AD 实例的特殊注意事项。"
    services="active-directory"
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD 连接︰ 特殊注意事项实例
Azure AD 连接最常用的 Azure AD 全球实例和 Office 365。 但也有其他实例，这些具有不同的 Url 和其他特殊注意事项的要求。

## <a name="microsoft-cloud-germany"></a>Microsoft 云德国
[Microsoft 云德国](http://www.microsoft.de/cloud-deutschland)是 sovereign 云由德国数据受信者。

正在预览此云。 很多方案可以平时的自己，如验证域，必须由运营商。 请如何在预览中加入的详细信息，联系您当地的 Microsoft 代表联系。

在代理服务器中打开的 Url |
--- |
\*。 microsoftonline.de |
\*。 windows.net |
+ 证书吊销列表 |

当您登录到 Azure 的广告目录必须在 onmicrosoft.de 域中使用的帐户。

在 Microsoft 云德国目前不存在的功能︰

- Azure AD 连接的状况将不可用。
- 自动更新不可用。
- 不可用密码写回。

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure 政府云
[Microsoft Azure 政府云](https://azure.microsoft.com/features/gov/)是为美国政府云。

此云已被受早期版本中的目录同步。 从生成 1.1.180 的 Azure AD 连接下一个支持云的生成。 这一代使用仅美国基于的端点，并具有不同的 Url，以在您的代理服务器中打开列表。

在代理服务器中打开的 Url |
--- |
\*。 microsoftonline.com |
\*。 gov.us.microsoftonline.com |
+ 证书吊销列表 |

Azure AD 连接将不能自动检测 Azure 的广告目录位于政府云。 相反，您需要安装 Azure AD 连接时执行以下操作。

1. 启动 Azure AD 连接安装。
2. 一旦您看到第一页应该是以接受 EULA，不要继续但离开运行该安装向导。
3. 启动注册表编辑器，并更改该注册表项`HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance`的值`2`。
4. 请转回 Azure AD 连接安装向导中，接受最终用户许可协议并继续。 安装过程中，一定要使用**自定义配置**安装路径 （并且不快速安装）。 然后像往常一样继续安装。

当前不存在 Microsoft Azure 政府云中的功能︰

- Azure AD 连接的状况将不可用。
- 自动更新不可用。
- 不可用密码写回。

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
