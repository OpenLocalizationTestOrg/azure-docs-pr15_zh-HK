<properties
    pageTitle="从目录同步和 Azure AD 同步升级 |Microsoft Azure"
    description="描述如何从目录同步和 Azure AD 同步升级到 Azure AD 连接。"
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


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>升级 Windows Azure Active Directory 同步 （"目录同步"） 和 Azure Active Directory 同步 （"Azure AD 同步"）
Azure AD 连接是使用 Azure AD 连接您的内部目录的最佳办法和 Office 365。 这是支持的因为这些工具现已被否决，并将到达最终在 4 月 13，2017年从 Windows Azure 活动目录同步 （目录同步） 或 Azure AD 同步升级到 Azure AD 连接的绝佳时机。

不建议使用两种标识同步工具已提供对于单个目录林的客户 （目录同步），对于多林和其他高级客户 （Azure AD 同步）。 这些较旧的工具已被替换为一个单一的解决方案，可用于所有方案︰ Azure AD 连接。 它提供了新的功能、 增强的功能以及对新方案的支持。 为了能够继续同步内部标识数据对 Azure AD 和 Office 365，我们强烈建议您升级到 Azure AD 连接。

7 月 2014 年发布的目录同步的最后一个版本并可能 2015 年发布 Azure AD 同步的最后一个版本。

## <a name="what-is-azure-ad-connect"></a>什么是 Azure AD 连接
Azure AD 连接是目录同步和 Azure AD 同步的后续产品。 它结合了所有方案支持这两种。 您可以阅读更多关于它[内部标识使用 Azure Active Directory](active-directory-aadconnect.md)集成。

## <a name="deprecation-schedule"></a>否决计划

日期 | 注释
 --- | ---
2016 年 4 月 13日， | Windows Azure 活动目录同步 （"目录同步"） 和 Microsoft Azure 活动目录同步 （"Azure AD 同步"） 被宣布为已弃用。
于 2017 2017年 4 月 13日， | 支持结束。 客户将不再能够无需首先升级到 Azure AD 连接开启支持案例。

## <a name="how-to-transition-to-azure-ad-connect"></a>如何转换到 Azure AD 连接
如果您正在运行目录同步有两种方法可以升级︰ 就地升级和并行部署。 就地升级建议对于大多数客户而言，如果您有最新操作系统和小于 50000 的对象。 在其他情况下建议不要并行部署目录同步配置到新的服务器运行 Azure AD 连接的移动位置。

如果您使用 Azure AD 同步建议就地升级。 如果您愿意，就可以并行安装的新的 Azure AD 连接服务器，然后执行从 Azure AD 同步服务器摆迁移到 Azure AD 连接。

解决方案 | 方案
----- | -----
[从目录同步升级](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>如果您有现有的目录同步服务器已在运行。</li>
[从 Azure AD 同步升级](active-directory-aadconnect-upgrade-previous-version.md)| <li>如果您正在从 Azure AD 同步移动。</li>

如果您想要了解如何从目录同步执行就地升级，Azure AD 连接，请参阅该频道 9 视频︰

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>常见问题
**问︰ 我收到电子邮件通知从 Azure 团队和/或从 Office 365 消息中心的消息，但我正在使用连接。**  
通知还发送到使用内部版本号 1.0 Azure AD 连接的客户。\*.0 （使用 pre 1.1 版本）。 Microsoft 建议客户及时与 Azure AD 连接版本。 1.1[自动升级](active-directory-aadconnect-feature-automatic-upgrade.md)功能将使它相当容易始终使用最新版本的 Azure AD 连接安装。

**问︰ 将目录同步/Azure AD 同步停止工作在 4 月 13，2017年上吗？**  
不。 当这些将不再能够与 Azure 广告通讯的日期将晚些时候宣布。 您将能够查找时使用本主题中的信息。

**问︰ 从可以升级哪些目录同步版本？**  
它支持从任何当前正在使用的目录同步版本升级。

**问︰ 怎样 FIM/MIM Azure AD 连接器？**  
Azure AD 连接器**为 FIM/MIM 已被**宣布为已弃用。 它位于**冻结功能**;添加任何新功能并接收无 bug 的修补程序。 Microsoft 建议客户使用它计划将其移动到 Azure AD 连接。 强烈建议启动时不使用任何新的部署。 该连接器将否决以后宣布。

## <a name="additional-resources"></a>其他资源

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
