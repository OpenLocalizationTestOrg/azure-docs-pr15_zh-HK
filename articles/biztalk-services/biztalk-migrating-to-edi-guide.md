<properties   
    pageTitle="迁移到 BizTalk 服务技术指南的 BizTalk Server EDI 解决方案 |Microsoft Azure"
    description="将 EDI 迁移到 MABS;Microsoft Azure BizTalk 服务"
    services="biztalk-services"
    documentationCenter="na"
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags 
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>将 BizTalk Server EDI 解决方案迁移到 BizTalk 服务︰ 技术指南

作者︰ Tim Wieman 和 Nitin Mehrotra

审阅者︰ Karthik Bharthy

编写使用︰ Microsoft Azure BizTalk 服务--2 月 2014年释放。

## <a name="introduction"></a>介绍

电子数据交换 (EDI) 是一种通过业务交换数据的最普遍方式以电子方式，也称为作为企业到企业或 B2B 交易记录。 BizTalk Server 已超过十年，因为最初的 BizTalk 服务器版本的 EDI 支持。 使用 BizTalk 服务，Microsoft 在 Microsoft Azure 平台上继续对 EDI 解决方案的支持。 B2B 事务主要是向组织，外部，因此很容易实现，如果它在云平台上实现。 Microsoft Azure 提供了此功能通过 BizTalk 服务。

有些客户查看 BizTalk 服务作为新的 EDI 解决方案"进行绿地"平台，而许多客户具有当前他们可能想要迁移到 Azure 的 BizTalk Server EDI 解决方案。 因为 BizTalk 服务 EDI 的体系结构基于同一关键实体为 BizTalk Server EDI 体系结构 （贸易伙伴、 实体、 协议），它有可能将 BizTalk Server EDI 项目迁移到 BizTalk 服务。

本文将讨论一些迁移 BizTalk Server EDI 项目涉及到 BizTalk 服务的差异。 本文档假定 BizTalk Server EDI 处理和贸易伙伴协议的工作知识。 BizTalk Server EDI 的详细信息，请参阅[贸易合作伙伴管理使用 BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx)。

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>哪个版本的 BizTalk Server EDI 工件可以迁移到 BizTalk 服务？

BizTalk Server 2010 中，BizTalk Server EDI 模块大大增强型，当它被改为包括合作伙伴、 配置文件和协议。 BizTalk 服务使用相同的模型来组织贸易合作伙伴和业务部门内的贸易伙伴。 因此，从 BizTalk Server 2010 及更高版本的 EDI 项目迁移到 BizTalk 服务中，是一个更直接的过程。 若要迁移与 BizTalk Server 2010 版本关联的 EDI 项目，必须先升级到 BizTalk Server 2010，然后将 EDI 项目迁移到 BizTalk 服务。

## <a name="scenariosmessage-flow"></a>方案/消息流

作为与 BizTalk Server，EDI 处理在 BizTalk 服务是围绕建立贸易合作伙伴管理 (TPM) 解决方案。 TPM 解决方案包括下列关键组件︰

- 贸易合作伙伴，代表在 B2B 事务的组织。
- 配置式，代表部门的贸易伙伴。
- 贸易伙伴协议 （或协议），它表示每个配置两个伙伴之间的业务协议。

下图演示了相似之处，以及 BizTalk Server EDI 解决方案和 BizTalk 服务 EDI 解决方案之间的差异︰

![][EDImessageflow]

关键的差异和相似之处，在 BizTalk Server EDI 解决方案流和 BizTalk 服务之间是︰

- 就像 BizTalk Server 使用 EDIReceive 管道接收 EDI 消息和 EDISend 管道发送 EDI 消息，BizTalk 服务使用 EDI 接收桥接器接收和发送 EDI 桥 EDI 消息发送。 在 BizTalk Server 管线与协议通过使用发送或接收端口。 在 BizTalk 服务协议本身表示发送或接收桥。
- 在 BizTalk Server EDIReceive 管道处理 EDI 消息后消息转储到一个 SQL Server 数据库。 EdiSend 管道然后挑选的 SQL Server 数据库中的信息，其进行处理，然后将其发送给贸易伙伴。

    在 BizTalk 服务的 EDI 接收后桥接器处理 EDI 消息，将消息路由到外部进程。 可以内部 Microsoft Azure 上运行程序的外部进程。 外部进程应将邮件路由到 EDI 发送桥;发送桥本身不拉消息。 在处理消息之后, EDI 发送桥将消息路由到的贸易伙伴。

BizTalk 服务提供了一种易于使用配置体验可以快速创建和部署之间没有配置任何 Microsoft Azure 计算实例 （Web 或辅助角色） 贸易合作伙伴，任何 Microsoft Azure 的 SQL 数据库或任何 Microsoft Azure 存储帐户的 B2B 协议。 更复杂的方案将需要将其他服务处理或工作流中""的周边贸易伙伴协议，也就是说，在贸易伙伴协议 EDI 桥接器处理前后。 详细地，以下序列的事件发生期间在 BizTalk 服务处理 EDI 消息。

1. EDI 消息来自贸易伙伴，Fabrikam。  从贸易伙伴接收 EDI 消息，BizTalk 服务支持的传输协议，如 FTP、 SFTP，AS2 和 HTTP/s。

2. 贸易伙伴协议接收端处理分解为 XML 格式的 EDI 消息。  您可以到服务总线中继终结点、 服务总线主题、 服务总线队列或 BizTalk 服务桥等服务总线端点路由分解的 EDI 消息 （以 XML 格式）。

3. 分解的 XML 消息然后会接收到从以进行进一步的自定义处理的终结点。  无法通过内部组件或 Microsoft Azure 计算实例进行进一步处理消息在 Windows 工作流 (WF) 或 Windows 通讯基础 (WCF) 服务，例如处理这些终结点。

4. "发送端处理"的贸易伙伴协议然后汇编到 EDI 格式的 XML 消息并发送到 Contoso 的贸易合作伙伴。  为了将 EDI 消息发送到的贸易伙伴，BizTalk 服务支持与接收 EDI 消息使用相同的协议。

本文进一步提供概念性指导上一些不同的 BizTalk Server EDI 项目迁移到 BizTalk 服务。

## <a name="sendreceive-ports-to-trading-partners"></a>发送/接收端口到贸易伙伴

在 BizTalk Server 设置接收位置和接收端口的贸易伙伴，从接收 EDI/XML 消息并发送端口设置将 EDI/XML 消息发送给贸易伙伴。 然后阻碍这些端口到贸易伙伴协议使用 BizTalk Server 管理控制台。 在 BizTalk 服务中，从贸易伙伴接收邮件的位置和您发送邮件到贸易伙伴配置为贸易合作伙伴协议本身 （作为传输设置的一部分） 中的 BizTalk 服务门户的一部分。  因此您确实没有"发送端口"和"接收位置"，这一概念本身，在 BizTalk 服务。 有关详细信息，请参阅[创建协议](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx)。

## <a name="pipelines-bridges"></a>管道 （桥）

BizTalk Server edi，管道是还可以包括自定义特定的处理功能，根据需要由应用程序逻辑的消息处理实体。 对于 BizTalk 服务，相当于将 EDI 桥。 但是在 BizTalk 服务，现在，EDI 桥是"关闭"。  也就是说，不能到 EDI 桥添加您自己的自定义活动。 任何自定义的处理必须完成外 EDI 桥在您的应用程序之前或之后邮件进入桥配置为贸易合作伙伴协议的一部分。 EAI 桥可以选择以执行自定义处理。 如果您希望自定义处理，您可以使用 EAI 桥之前或之后通过 EDI 桥处理消息。 有关详细信息，请参阅[如何在桥中包括自定义代码](https://msdn.microsoft.com/library/azure/dn232389.aspx)。

您可以插入自定义代码和/或使用服务总线消息队列和主题，贸易伙伴协议接收邮件之前, 或之后该协议处理消息并将其路由到服务总线端点的发布/订阅流量。

本主题对邮件流模式中看到**方案/消息流**。

## <a name="agreements"></a>协议

如果您熟悉 BizTalk 服务器 2010年交易伙伴协议进行 EDI 处理，BizTalk 服务贸易伙伴协议一定很熟悉。 大多数协议的设置相同，并且使用相同的术语。 在某些情况下，协议设置要简单得多相比在 BizTalk Server 相同的设置。 Microsoft Azure BizTalk 服务支持 X12、 EDIFACT、 和 AS2 传输。

Microsoft Azure BizTalk 服务还提供了一种**TPM 数据迁移**工具将贸易合作伙伴和协议从 BizTalk Server 贸易合作伙伴模块迁移到 BizTalk 服务门户。 TPM 数据迁移工具作为工具软件包的一部分，它可以从下载[MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)。 包还包括自述文件说明如何使用该工具的工具和基本的故障排除信息。

## <a name="schemas"></a>架构

BizTalk 服务提供了可以在 BizTalk 服务解决方案中使用 EDI 架构。  此外，BizTalk Server EDI 架构还可与 BizTalk 服务由于 EDI 架构的根节点在 BizTalk Server 以及 BizTalk 服务是同一个。 因此，您将能够直接采取 BizTalk Server EDI 架构并在您开发使用 BizTalk 服务的 EDI 解决方案中使用它们。 您还可以从[MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)下载架构。

## <a name="maps-transforms"></a>映射 （转换）

BizTalk Server 中的映射在 BizTalk 服务称为转换。 将映射从 BizTalk Server 迁移到 BizTalk 服务可能是一个更复杂的任务，以实现 （具体取决于映射复杂）。 映射工具，使用 BizTalk 服务的是不同于 BizTalk 映射器。 尽管映射程序通常具有相同的外观，基础地图格式是不同的。 用户可以使用 functoid （称为**映射操作**中 BizTalk 服务） 也是不同的。  实际上，不能直接在 BizTalk 服务使用 BizTalk 映射。 此外，并非所有可用在 BizTalk Server functoid 均显示为 BizTalk 服务中的映射操作。

### <a name="new-transform-operations"></a>新的转换操作

尽管可用的转换的映射操作的列表可能看起来截然不同的 BizTalk Server 映射器，BizTalk 服务将转换具有新的途径来完成相同的任务。 例如，BizTalk 服务将转换具有**列表操作**可用。 这不是在 BizTalk 映射器中可用。  **列表操作**使您可以创建和运行"列表"，在其中一个列表是一组项 （也称为"行"），且每个项可以有多个成员 （也称为"列"）。  可以对列表进行排序，选择基于条件等项目。

在 BizTalk 服务将转换中的新功能的另一个例子是**循环操作**。  很难在 BizTalk Server 映射器中创建嵌套的循环。  因此，为 BizTalk 服务将转换添加循环映射操作。

还有另一个例子是**如果那时其他**表达式映射操作。  在执行 if then 其他操作，可能会在 BizTalk 映射器，但它所需的多个功能组件来完成一个看似简单的任务。

### <a name="migrating-biztalk-server-maps"></a>BizTalk Server 迁移映射

Microsoft Azure BizTalk 服务提供了一种工具来迁移 BizTalk Server 将映射到 BizTalk 服务转换。 **BTMMigrationTool**可作为**与[BizTalk 服务 SDK 下载](http://go.microsoft.com/fwlink/p/?LinkId=235057)中提供的工具包**的一部分。 有关该工具的详细信息，请参阅[转换 BizTalk 映射到 BizTalk 服务转变](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx)。

您还可以查看 Sandro.佩雷拉，BizTalk MVP 的如何[迁移到 BizTalk 服务转换的 BizTalk Server 映射](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx)的示例。

## <a name="orchestrations"></a>业务流程

如果您需要迁移处理到 Microsoft Azure 的 BizTalk Server 业务流程，业务流程需要被重写，因为 Microsoft Azure 不支持运行 BizTalk Server 业务流程。  您可以重写在 Windows 工作流基础 4.0 (WF4) 服务的业务流程功能。  因为当前没有迁移从 BizTalk Server 业务流程到 WF4，这将是完全的重写。 下面是一些资源的 Windows 工作流︰

- 通过 Paolo Salvatori[*如何集成 WCF 工作流服务与服务总线队列和主题*](https://msdn.microsoft.com/library/azure/hh709041.aspx)。 

- 从生成 2011年会议[*窗口基础工作流和 Azure 构建应用程序*会话](http://go.microsoft.com/fwlink/p/?LinkId=237314)。

- 在 MSDN 上的[*Windows 工作流基础开发中心*](http://go.microsoft.com/fwlink/p/?LinkId=237315)。

- 在 MSDN 上的[*Windows 工作流基础 4 (WF4) 文档*](https://msdn.microsoft.com/library/dd489441.aspx)。

## <a name="other-considerations"></a>其他考虑事项

下面是您必须在使用 BizTalk 服务时进行的几个注意事项。

### <a name="fallback-agreements"></a>备用协议

BizTalk Server EDI 处理有"备用协议"的概念。  Does BizTalk 服务**不**到目前为止有一个备用协议的概念。  在 BizTalk Server 如何使用备用协议请参阅 BizTalk 文档[处理 EDI 的协议角色](http://go.microsoft.com/fwlink/p/?LinkId=237317)和[全局配置或备用协议属性](https://msdn.microsoft.com/library/bb245981.aspx)信息的主题。

### <a name="routing-to-multiple-destinations"></a>路由到多个目的地

BizTalk 服务的桥梁，在现阶段不支持将邮件路由到多个目标使用发布-订阅模型。 而您无法路由从 BizTalk 服务桥到服务总线主题，然后可以为多个订阅接收多个终结点上的邮件消息。

## <a name="conclusion"></a>结论

在常规的里程碑，以添加更多特性和功能更新 Microsoft Azure BizTalk 服务。 每次更新中，我们期待着支持增强的功能，以便创建使用 BizTalk 服务和其他 Azure 技术的端到端解决方案。

## <a name="see-also"></a>请参见

[使用 Azure 开发企业应用程序](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
