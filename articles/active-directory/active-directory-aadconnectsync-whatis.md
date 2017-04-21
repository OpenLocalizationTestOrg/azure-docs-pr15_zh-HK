<properties
    pageTitle="Azure AD 连接同步︰ 了解并自定义同步 |Microsoft Azure"
    description="解释如何 Azure AD 连接的工作原理以及如何自定义同步。"
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
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD 连接同步︰ 了解并自定义同步
Azure 活动目录连接同步服务 （Azure AD 连接同步） 是 Azure AD 连接的一个主要组件。 它负责相关同步您的内部环境和 Azure 的广告之间的标识数据的所有操作。 Azure AD 连接同步是后续的目录同步，Azure AD 同步，并使用 Azure Active Directory 连接器配置 Forefront 标识管理器。

本主题是**Azure AD 连接同步**（也称为**同步引擎**） 的主，并列出了所有与之相关的其他主题的链接。 Azure AD 连接的链接，请参阅[集成内部标识使用 Azure 活动目录](active-directory-aadconnect.md)。

同步服务包括两个组件、 内部**Azure AD 连接同步**组件和服务端在 Azure 称为**Azure AD 连接同步服务**的广告。 服务是很常见的目录同步，Azure AD 同步和 Azure AD 连接。

## <a name="azure-ad-connect-sync-topics"></a>Azure AD 连接同步主题

主题 | 它涵盖了什么以及何时读取
----- | -----
**Azure AD 连接同步基础知识** |
[了解该体系结构](active-directory-aadconnectsync-understanding-architecture.md) | 对于那些刚接触同步引擎并且想要了解的体系结构和使用的术语。
[技术概念](active-directory-aadconnectsync-technical-concepts.md) | 短的体系结构主题并简要说明了使用的术语。
[Azure AD 拓扑连接](active-directory-aadconnect-topologies.md) | 介绍了不同的拓扑和同步引擎支持的方案。
**自定义配置** |
[再次运行安装向导](active-directory-aadconnectsync-installation-wizard.md) | 解释什么选项有可用，当您再次运行 Azure AD 连接安装向导。
[了解声明式的资源调配](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| 介绍了称为声明性快速部署的配置模型。
[了解资源调配的声明性表达式](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | 介绍用于声明性调配表达式语言语法。
[了解默认配置](active-directory-aadconnectsync-understanding-default-configuration.md)| 介绍了现成的规则和默认的配置。 此外介绍了这些规则如何协同工作的现成的方案工作。
[了解用户和联系人](active-directory-aadconnectsync-understanding-users-and-contacts.md) | 在上一个主题继续并描述了用户和联系人的配置原理在一起，特别是在多目录林环境中。
[如何对默认配置进行更改](active-directory-aadconnectsync-change-the-configuration.md) | 引导您完成使常见配置更改为属性流的方式。
[更改默认配置的最佳实践](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | 支持限制和对现成的配置进行更改。
[配置筛选](active-directory-aadconnectsync-configure-filtering.md) | 介绍如何限制哪些对象正在对 Azure AD 同步和分步如何配置这些选项的不同选项。
**功能和方案** |
[防止意外删除](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | 介绍了*防止意外删除*功能以及如何对其进行配置。
[计划程序](active-directory-aadconnectsync-feature-scheduler.md) | 描述内置的计划程序，该导入、 同步和导出数据。
[实施密码同步](active-directory-aadconnectsync-implement-password-synchronization.md) | 描述密码同步的工作原理、 如何实现，以及如何操作和故障排除。
[设备写回](active-directory-aadconnect-feature-device-writeback.md) | 描述设备写回在 Azure AD 连接的工作方式。
[目录扩展](active-directory-aadconnectsync-feature-directory-extensions.md) | 描述如何使用您自己的自定义属性的 Azure 的 AD 架构扩展。
**同步服务** |
[Azure AD 连接同步服务功能](active-directory-aadconnectsyncservice-features.md) | 介绍了同步服务端以及如何在 Azure AD 更改同步设置。
[重复的属性可恢复性](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | 描述如何启用和使用**范围内**，并**代理地址**重复的特性值的自我恢复能力。
**操作和用户界面** |
[同步服务管理器](active-directory-aadconnectsync-service-manager-ui.md) | 介绍了同步服务管理器用户界面，包括[操作](active-directory-aadconnectsync-service-manager-ui-operations.md)、[连接器](active-directory-aadconnectsync-service-manager-ui-connectors.md)、[元节设计器](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)，以及[元搜索](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)选项卡。
[操作任务和考虑事项](active-directory-aadconnectsync-operations.md) | 描述操作的问题，如灾难恢复。
**如何。。。** |
[重置的 Azure 的广告帐户](active-directory-aadconnectsync-howto-azureadaccount.md) | 如何重置用于从 Azure AD 连接同步连接到 Azure AD 服务帐户的凭据。
**更多的信息和参考资料** |
[端口](active-directory-aadconnect-ports.md) | 列出了您需要同步引擎内部目录和 Azure 广告之间打开的端口。
[同步到 Azure 活动目录的属性](active-directory-aadconnectsync-attributes-synchronized.md) | 列出所有广告和 Azure AD 之间内部正在同步的属性。
[功能参考](active-directory-aadconnectsync-functions-reference.md) | 列出中声明性资源调配提供的所有功能。

## <a name="additional-resources"></a>其他资源

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
