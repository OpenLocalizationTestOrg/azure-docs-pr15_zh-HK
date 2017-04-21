
<properties
    pageTitle="疑难解答动态组的成员身份 |Microsoft Azure"
    description="在 Azure AD 中的故障排除技巧动态组的成员资格。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>疑难解答动态组的成员身份

**我对一组配置规则，但不到成员资格获取更新的组**<br/>验证在**配置**选项卡中的**启用委派组管理**设置被设置为**是**。 只有在 Azure 活动目录特优许可证分配给其的用户身份登录，您将看到此设置。 验证规则的用户属性的值︰ 有没有满足该规则的用户？

**我配置规则，但现在删除规则的现有成员**<br/>这是预期的行为。 在启用或更改规则中删除现有的组的成员。 返回从评估的该规则的用户作为成员添加到组。     

**我看不到成员身份变化时添加或更改规则，为什么不立即？**<br/>专用的成员资格评估是定期执行异步后台进程。 此过程花费的时间取决于在您的目录中的用户数量和规则创建的组的大小。 通常情况下，有少量用户的目录将在短短几分钟内看到组成员身份的更改。 具有大量用户的目录可能需要 30 分钟或更长时间来填充。

这些文章在 Azure Active Directory 提供附加信息。

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)
* [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
* [什么是 Azure 活动目录？](active-directory-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
