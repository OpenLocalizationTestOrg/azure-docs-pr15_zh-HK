<properties
    pageTitle="Azure AD 连接同步︰ 技术概念 |Microsoft Azure"
    description="介绍了 Azure AD 连接同步的技术概念。"
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


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD 连接同步︰ 技术概念
这篇文章是主题的[了解体系结构](active-directory-aadconnectsync-technical-concepts.md)的摘要。

Azure AD 连接同步基础坚实的元目录同步平台。
以下各节介绍了元目录同步的概念。
基于 MIIS、 ILM 和 FIM，Azure Active Directory 同步服务提供用于连接到数据源、 数据源以及调配之间的数据同步和撤消身份的下一个平台。

![技术概念](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

以下各节提供了有关 FIM 同步服务的以下方面的更多详细信息︰

- 连接器
- 属性流
- 连接器空间
- 元节
- 资源调配

## <a name="connector"></a>连接器

用于连接目录与通信的代码模块称为连接器 （以前称为管理代理 (Ma)）。

运行同步 Azure AD 连接的计算机上安装这些。
连接器提供无代理能够进行对话而不是依靠专用代理的部署使用远程系统协议。 这意味着降低的风险和部署时间，尤其是当处理关键应用程序和系统。

在上图中，连接器连接器空间与同义，但涵盖了所有与外部系统通信。

连接器将负责所有的导入和导出功能到系统使开发人员无需了解如何连接到每个系统本身使用声明性资源调配以自定义数据转换时。

导入和导出时，才发生安排，从而进一步保温材料的更改发生在系统中，由于更改执行不会自动传播到连接的数据源。 此外，开发人员也可以创建自己接头，用于连接到几乎任何数据源。

## <a name="attribute-flow"></a>属性流

元节是所有联接标识从邻近的连接器空间的统一的视图。 在上图中，属性流带为入站和出站流箭头的线条描绘。 属性流是复制或转换数据从一个系统到另一个的过程，所有属性流 （入站或出站）。

属性流时发生连接器空间和元节之间双向同步 （完全或增量） 操作会按计划运行。

这些同步运行时，属性流才会出现。 在同步规则中定义属性流。 这些可以是入站 (ISR 如上图所示) 或出站 (OSR 如上图所示)。

## <a name="connected-system"></a>已连接的系统

已连接的系统 （亦即连接目录） 是指远程系统同步已连接到 Azure AD 连接和读取和写入标识数据，进出。

## <a name="connector-space"></a>连接器空间

每个连接的数据源都作为筛选的对象和连接器空间中的属性的子集。
这允许同步服务而无需访问远程系统同步对象时执行本地操作和限制于导入的交互以及仅导出。

当数据源和连接器具有能够提供更改 （增量导入） 的列表时，然后运营效率大大提高仅更改为自上次轮询周期进行交换。 连接器空间隔离开来更改会自动传播通过要求连接器日程安排导入和导出连接的数据的源。 此增加的保险授予您高枕无忧测试、 预览，或确认下一次更新时。

## <a name="metaverse"></a>元节

元节是所有联接标识从邻近的连接器空间的统一的视图。

标识链接在一起并颁发机构分配的各种特性，通过导入流映射后，中央的元节对象开始聚合信息来自多个系统。 与此对象属性流映射执行出站系统的信息。

当权威系统项目它们到了元节创建对象。 一旦所有的连接将被删除，删除的元节对象。

中元节对象不能直接编辑。 通过属性流，对象中的所有数据必须做出都贡献。 元节保持持久连接器与每个连接器空间。 这些连接器不需要重新计算每个同步运行。 这意味着 Azure AD 连接同步不需要每次找到匹配的远程对象。 这将避免代价高昂的代理，以防止对通常会负责相关对象的属性更改的需要。

发现新的数据源可能具有预先存在的对象，需要管理，Azure AD 连接同步使用称为联接规则的过程来评估潜在的候选人，用来建立一个链接。
建立链接后，这一评估不会不会再次出现，普通属性流可远程连接的数据源和元之间。

## <a name="provisioning"></a>资源调配

当权威来源项目到元节新的连接器空间对象的新对象可以创建表示下游连接的数据源的另一个接头。

这本质上是建立链接，和属性流可以双向进行。

只要规则确定，需要创建一个新的连接器空间对象，调用资源调配。 但是，因为此操作只发生在连接器空间中，它不会不会延续到已连接的数据源执行导出之前。

## <a name="additional-resources"></a>其他资源

* [Azure AD 连接的同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
