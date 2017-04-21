<properties
    pageTitle="Azure AD 连接同步︰ 理解用户和联系人 |Microsoft Azure"
    description="解释用户和 Azure AD 连接同步的联系人。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD 连接同步︰ 理解用户和联系人

有几个不同的原因，就会有多个活动目录林的原因，并有若干种不同的部署拓扑。 公共模型后开展兼并 / 收购包括帐户资源部署和 GAL sync'ed 林。 但即使有纯模型，以及混合模型是通用的。 Azure AD 连接同步中的默认配置不会采用任何特定的模型，但这取决于如何匹配的用户已选择的安装指南中，可以观察到不同的行为。

在本主题中，我们将转到默认的配置在某些拓扑结构中的行为方式。 我们将经过配置，可以使用同步规则编辑器来查看配置。

有几个配置假定的一般规则︰

- 无论我们从活动目录的源中导入的顺序，最终结果应始终是相同的。
- 为活动帐户始终提供的登录信息，包括**范围内**和**sourceAnchor**。
- 已禁用的帐户提供范围内和 sourceAnchor，除非它是链接的邮箱，如果没有活动的帐户来找。
- 链接邮箱的帐户永远不会将用于范围内和 sourceAnchor。 假定将稍后发现活动帐户。
- 作为联系人或用户，可能到 Azure AD 配置联系人对象。 真的不知道直到处理完所有的源活动目录林。

## <a name="contacts"></a>联系人

有表示不同的目录林中的用户的联系人后，将共同开展兼并 / 收购 GALSync 解决方案对两个或多个 Exchange 林桥接。 联系人对象总是加入从连接器空间使用邮件属性在元节。 如果已经有一个联系人对象或具有相同邮件地址的用户对象，这些对象连接在一起。 这规则中配置的**在从广告--联系人加入**。 另外，还有一个名为规则**在从广告 – 常用联系人**带到了元节属性流特性与常**联系**的**sourceObjectType** 。 此规则的优先级很低，如果任何用户对象加入到同一元节对象，则该规则**在从广告 – 用户通用**将分配给此属性的值用户。 对于此规则，此属性将具有值如果没有用户已联接的联系人和用户的值，如果已找到至少一个用户。

资源调配到 Azure 的 AD 对象， **AAD – 联系加入到出**站规则将创建联系人对象如果元节特性**sourceObjectType**设置为**与联系**。 如果此属性设置为**用户**，然后该规则**AAD – 用户加入到出**将创建用户对象。
很可能，对象从提升联系到用户当多个源活动目录导入和同步。

例如，在 GALSync 拓扑中我们会发现联系人对象每个人在第二个林中时我们导入第一个林。 这将暂存 AAD 连接器中的新联系人对象。 当我们稍后导入并同步另一个林时，我们将找到真实的用户并将它们加入到现有的元节对象。 然后，我们将删除 AAD 中的联系人对象，并改为创建新的用户对象。

如果您有一个拓扑结构，用户和表示为联系人，请确保您选择匹配上的安装指南中的邮件属性的用户。 如果您选择另一个选项，您将具有顺序取决于配置。 联系人对象将始终加入邮件属性，但是如果安装指南中选择了此选项，用户对象将仅加入邮件属性上。 您然后最终可能会得到相同的邮件属性与元节中的两个不同对象如果用户对象之前导入的联系人对象。 在导出到 Azure 的广告，将引发错误。 此行为是设计使然，则指示，拓扑结构已不能正确识别安装过程中或坏数据。

## <a name="disabled-accounts"></a>已禁用的帐户

已禁用的帐户也同步到 Azure 的广告。 已禁用的帐户是常用来表示资源交换，例如会议室中。 例外情况是用户链接邮箱;如前文所述，这些将永远不会设置到 Azure 的广告客户。

假设是，如果找到了一个被禁用的用户帐户，然后我们将找不到另一个活动帐户以后到 Azure 的范围内和 sourceAnchor 找到 AD 配置对象。 如果另一个活动帐户将加入同一元节对象，则将使用其范围内和 sourceAnchor。

## <a name="changing-sourceanchor"></a>更改 sourceAnchor

当对象已导出到 Azure 的广告则不允许再更改 sourceAnchor。 在导出对象时接受的 Azure AD **sourceAnchor**值设置元节属性**cloudSourceAnchor** 。 如果**sourceAnchor**更改，并且与**cloudSourceAnchor**不匹配，**出给 AAD – 用户加入**此规则将引发错误**sourceAnchor 属性已更改**。 在这种情况下，配置或数据必须更正了相同的 sourceAnchor 是中元节再次出现之前可以再次同步对象。

## <a name="additional-resources"></a>其他资源

* [Azure AD 连接的同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
