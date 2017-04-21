<properties
    pageTitle="Azure AD 连接同步︰ 最佳做法更改默认配置 |Microsoft Azure"
    description="提供用于更改默认配置的 Azure AD 连接同步的最佳做法。"
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
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD 连接同步︰ 最佳做法更改默认配置
本主题的目的是为了说明支持和不支持到 Azure AD 连接同步的更改。

由 Azure AD 连接的配置适用于大多数的环境中使用 Azure AD 同步内部部署 Active Directory"按原样"。 但是，在某些情况下，有必要应用某些更改配置以满足特定需要或要求。

## <a name="changes-to-the-service-account"></a>更改服务帐户
Azure AD 连接同步安装向导所创建的服务帐户下运行。 此服务帐户将使用同步的数据库存储加密密钥。 创建与 127 个字符长的密码和密码设置为永不过期。

- 它不**支持**更改或重新设置服务帐户的密码。 这样做会破坏加密密钥，该服务将不能访问数据库和将不能启动。

## <a name="changes-to-the-scheduler"></a>对计划程序的更改
从开始从版本 1.1 版本 (2 月 2016) 可以配置[计划程序](active-directory-aadconnectsync-feature-scheduler.md)拥有比默认设置不同的同步周期 30 分钟。

## <a name="changes-to-synchronization-rules"></a>更改同步规则
安装向导提供了一种配置，应发挥作用的最常见的情况。 如果您需要对配置进行更改，您必须遵循这些规则仍有受支持的配置。

- 如果不适合贵组织的默认直接属性流，可以[更改属性流](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes)。
- 如果您想[不流特性](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute)并在 Azure AD 中删除任何现有的属性值，则需要为这种情况下创建规则。
- [禁用不需要的同步规则](#disable-an-unwanted-sync-rule)而不是删除它。 在升级过程中重新创建已删除的规则。
- 若要[更改一个现成的规则](#change-an-out-of-box-rule)，应制作原始规则的副本并禁用现成的规则。 同步规则编辑器提示，可以帮助您。
- 导出自定义同步规则使用同步规则编辑器。 编辑器中为您提供了 PowerShell 脚本可用来方便地重新创建这些在灾难恢复方案。

>[AZURE.WARNING] 现成的同步规则有指纹。 如果您对这些规则进行更改，则不再匹配的指纹。 将来当您尝试应用新的发行版的 Azure AD 连接时，可能会有问题。 只更改所描述的方式在这篇文章。

### <a name="disable-an-unwanted-sync-rule"></a>禁用不需要的同步规则
不要删除一个现成的同步规则。 它是在下一步升级过程中重新创建。

在某些情况下，安装向导提供了工作不为您的拓扑配置。 例如，如果您有帐户资源林拓扑，但具有扩展帐户目录林的架构和交换架构，交换规则是为帐户林和资源林创建。 在这种情况下，您需要禁用同步规则进行交换。

![禁用的同步规则](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

如上图所示安装向导发现旧 Exchange 2003 架构在帐户目录林。 之前在 Fabrikam 的环境中引入资源目录林中添加了此架构扩展。 若要确保不同步从旧 Exchange 实现任何属性，在显示同步规则应被禁用。

### <a name="change-an-out-of-box-rule"></a>更改一个现成的规则
如果您需要更改一个现成的规则，您应建立一份现成的规则并禁用原来的规则。 然后对克隆的规则进行更改。 同步规则编辑器可以帮助您通过这些步骤。 当您打开一个现成的规则时，会出现此对话框︰  
![从框中规则的警告](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

选择**是**以创建规则的副本。 然后打开复制的规则。  
![复制的规则](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

此克隆规则范围、 联接和转换到进行必要的更改。

## <a name="next-steps"></a>下一步行动

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
