<properties
    pageTitle="未经授权的使用情况报告 |Microsoft Azure"
    description="未授权的使用报表可帮助识别正在使用的未经授权的用户支付 Azure 的广告功能。"
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>未经授权的使用情况报告

未授权的使用报表可帮助识别正在使用的未经授权的用户支付 Azure 的广告功能。 这样，您能够更好地利用您购买的许可证和标识您知道时您可能需要额外的许可证。 

此报告显示在过去的 30 天内活动使用的支付功能。 

## <a name="report-structure"></a>报告的结构
 
| 列名称          |    说明 |
| :--                  | :--         |
| 未授权的用户      |    用户的名称 |
| 功能              | 功能名称。 例如︰ 条件访问 |
| 访问应用程序 | 正在访问的功能的应用程序的名称。 例如︰ Office 365 SharePoint Online |

 
> [AZURE.NOTE] 如果用户帐户已被删除未授权用户列将随一个 ID，如 1003000090D8B285


## <a name="conditional-access-feature"></a>条件接收功能

访问具有条件的访问策略，如果它们没有 Azure AD 特优许可应用的服务时，未经授权的用户将被标记。 

这适用于 MFA / 位置策略以及设备策略使用 Intune。
 

## <a name="see-also"></a>请参见

- [使用 Office 365 和其他 Azure Active Directory 的条件访问连接应用程序](active-directory-conditional-access.md)
- [入门条件 Azure 广告权](active-directory-conditional-access-azuread-connected-apps.md) 


