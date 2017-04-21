<properties
   pageTitle="Azure AD 连接同步︰ 目录扩展 |Microsoft Azure"
   description="本主题介绍在 Azure AD 连接目录扩展功能。"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD 连接同步︰ 目录扩展
目录扩展允许您从内部部署 Active Directory 在 Azure AD 具有自己的特性扩展架构。 此功能允许您生成消耗的特性继续管理内部 LOB 应用程序。 这些特性可以通过[Azure 广告图形目录扩展](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)或[Microsoft Graph](https://graph.microsoft.io/)消耗。 您可以查看可用分别使用[Azure 广告图形资源管理器](https://graphexplorer.cloudapp.net)和[Microsoft Graph 资源管理器中](https://graphexplorer2.azurewebsites.net/)的属性。

目前没有 Office 365 的工作负荷消耗这些特性。

配置您想要同步自定义设置路径在安装向导中的其他属性。
![扩展架构向导](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)安装显示以下属性的有效候选对象︰

- 用户和组对象类型
- 单值属性︰ 字符串、 布尔值、 整数、 二进制文件
- 多值的属性︰ 字符串、 二进制

从 Azure AD 连接的安装过程中创建的缓存中读取的属性列表。 如果您已扩展 Active Directory 架构具有其他属性，[必须刷新架构](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema)之前这些新特性是可见的。

一个对象可以有最多 100 个目录扩展属性。 最大长度为 250 个字符。 如果属性值的长度，则将被截断，同步引擎。

在安装期间 Azure AD 连接，这些特性可注册应用程序。 您可以看到此应用程序在 Azure 的门户。  
![架构扩展应用程序](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

现在，这些特性是通过图形可用︰  
![关系图](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

扩展为前缀的属性\_{AppClientId}\_。 AppClientId Azure 的广告目录中具有相同值的所有属性。

## <a name="next-steps"></a>下一步行动
了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
