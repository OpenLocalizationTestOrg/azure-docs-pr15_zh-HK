<properties
    pageTitle="了解在 BizTalk 服务版本中的功能 |Microsoft Azure"
    description="比较了 BizTalk 服务版本的功能︰ 释放，开发人员、 基本、 标准和特优。 MABS，WABS。"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk 服务︰ 版图表

Azure BizTalk 服务提供的多个版本。 本文介绍用于确定哪个版本最适合您的方案和业务需要。


## <a name="compare-the-editions"></a>比较版本

**释放 （预览）**

可以创建和管理混合连接。 混合连接是连接到内部部署的系统，如 SQL Server Azure 网站的简便方法。

**开发人员**

X12 和 AS2 包括混合连接、 EAI 和 EDI 消息处理与贸易合作伙伴管理门户中易于使用，并对常见的 EDI 架构的支持和丰富的 EDI 处理。 可以创建使用任何 HTTP/S、 余料、 FTP、 WCF 和 SFTP 的协议来读取和写入消息连接中云服务的常见 EAI 方案。  利用现成的 SAP、 Oracle 电子商务、 Oracle 数据库、 注册和 SQL Server 适配器连接到内部 LOB 系统。 使用 Visual Studio 工具使用开发人员中心环境，易于开发和部署。 限制为仅与任何服务级别协议 (SLA) 的开发和测试目的。

**基本**

在混合连接、 EAI 桥、 EDI 协议和 BizTalk 适配器包连接中包含的大多数开发人员功能增加。 此外提供了高可用性，并可以扩展与服务级别协议 (SLA)。

**标准**

在混合连接、 EAI 桥、 EDI 协议和 BizTalk 适配器包连接包括增加的所有基本功能。 此外提供了高可用性，并可以扩展与服务级别协议 (SLA)。

**高级**

在混合连接、 EAI 桥、 EDI 协议和 BizTalk 适配器包连接包括增加的所有标准功能。 此外包括存档、 高可用性和缩放与服务级别协议 (SLA) 的选项。


## <a name="editions-chart"></a>版本的图表
下表列出的差异。

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>释放 （预览）</th>
        <th>开发人员</th>
        <th>基本</th>
        <th>标准</th>
        <th>高级</th>
</tr>

<tr>
<td><strong>起始价格</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Azure BizTalk 服务定价</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure 定价计算器</a></td>
</tr>
<tr>
<td><strong>默认最小配置</strong></td>
<td>1 可用单元</td>
<td>1 开发者单位</td>
<td>1 个基本单位</td>
<td>1 个标准单位</td>
<td>1 高级单位</td>
</tr>
<tr>
<td><strong>小数位数</strong></td>
<td>无缩放比例</td>
<td>无缩放比例</td>
<td>是的每次 1 个基本单位</td>
<td>是的在 1 个标准单位的增量</td>
<td>是的在 1 个优质单位的增量</td>
</tr>
<tr>
<td><strong>最大允许向外扩展</strong></td>
<td>无缩放比例</td>
<td>无缩放比例</td>
<td>最多 8 个单元</td>
<td>最多 8 个单元</td>
<td>最多 8 个单元</td>
</tr>
<tr>
<td><strong>每个单位的 EAI 桥</strong></td>
<td>不包含</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI AS2</strong>
<br/><br/>
包括 TPM 协议</td>
<td>不包含</td>
<td>包括。 每个单位的 10 个协议。</td>
<td>包括。 每个单位的 50 协议。</td>
<td>包括。 每个单位的 250 协议。</td>
<td>包括。 每个单位的 1000年协议。</td>
</tr>
<tr>
<td><strong>每个单元的混合连接</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>每个单元的混合连接数据传输 (GB)</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk 适配器服务连接到内部 LOB 系统</strong></td>
<td>不包含</td>
<td>1 连接</td>
<td>2 连接</td>
<td>5 连接</td>
<td>25 日的连接</td>
</tr>
<tr>
<td align="left"><strong>受支持的协议系统︰</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>服务总线 (SB)</li>
<li>Azure 的斑点</li>
<li>REST Api</li>
</ul>
</td>
<td>不包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
</tr>
<tr>
<td><strong>高可用性</strong>
<br/><br/>
为服务级别协议 (SLA)，请参阅<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure BizTalk 服务定价</a>。
</td>
<td>不包含</td>
<td>不包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
</tr>
<tr>
<td><strong>备份和恢复</strong></td>
<td>不包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
</tr>
<tr>
<td><strong>跟踪</strong></td>
<td>不包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
</tr>
<tr>
<td><strong>归档</strong><br/><br/>
包括收据 (NRR) 和下载跟踪的消息的不可否认性</td>
<td>不包含</td>
<td>包含</td>
<td>不包含</td>
<td>不包含</td>
<td>包含</td>
</tr>
<tr>
<td><strong>使用自定义代码</strong></td>
<td>不包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
</tr>
<tr>
<td><strong>转换，其中包括自定义 XSLT 的使用</strong></td>
<td>不包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
<td>包含</td>
</tr>
</table>

> [AZURE.NOTE] 针对硬件故障的恢复能力，高可用性意味着有一个 BizTalk 单元内的多个虚拟机。


## <a name="faqs"></a>常见问题解答

#### <a name="what-is-a-biztalk-unit"></a>BizTalk 单位是什么？
"计价单位"是 Azure BizTalk 服务部署的原子级别。 每个版本附带有不同的计算能力和内存单位。 例如，基本单位有多个开发人员比计算，标准有更多比，计算等。 缩放时 BizTalk 服务，您的单位进行缩放。

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>BizTalk 服务和 Azure BizTalk 虚拟机之间的区别是什么？
BizTalk 服务提供一个真正的平台作为-服务 (PaaS) 体系结构构建集成解决方案在云中。 使用 PaaS 模型，重点完全放在应用程序逻辑和基础结构管理的所有保留到 Microsoft，包括︰

- 不是需要管理或修补程序的虚拟机。
- Microsoft 可以确保可用性。
- 您可以控制比例需通过只需请求更多或较低容量通过 Azure 的门户。

BizTalk Server Azure 虚拟机提供了一个基础结构作为-服务 (IaaS) 的体系结构。 创建虚拟机并将其配置与您的内部环境，便于运行现有的应用程序在云中使用不需要更改代码。 使用 IaaS，您是仍然负责配置虚拟机、 管理虚拟机 （例如，安装软件和操作系统补丁安装），以及构建应用程序的高可用性。

如果您在查看构建最小化您的基础结构管理工作的新的集成解决方案，使用 BizTalk 服务。 如果您希望快速迁移现有的 BizTalk 解决方案或寻找开发和测试 BizTalk Server 应用程序按需环境，使用 BizTalk Server Azure 虚拟机上。

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>BizTalk 适配器服务和混合连接之间的区别是什么？
Azure BizTalk 服务使用 BizTalk 适配器服务。 BizTalk 适配器服务使用 BizTalk 适配器包连接到内部的业务线 (LOB) 系统。 混合连接提供轻松便捷的方式连接到内部资源 Azure 应用程序，如在 Azure 应用程序服务和 Azure 移动服务，Web 应用程序功能。

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>"混合连接数据传输 (GB) 单位"是什么意思？ 这是每分钟/小时/天/周/月？ 当达到限制时，会发生什么情况？

每个单元的混合连接成本取决于 BizTalk 服务版本。 简单地说，成本取决于传输的数据数量。 例如，每天 10 GB 数据的传输成本更低，比每天传输 100GB。 使用 BizTalk 服务[定价计算器](https://azure.microsoft.com/pricing/calculator/?scenario=full)来确定特定的成本。 通常情况下，每日强制限制。 如果超过此限制，超额收取 $1 每 GB 的速率。

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>当我在 BizTalk 服务创建协议时，为什么 does 桥数上升由两个而不是一个？

每个协议包括两种不同的桥接器︰ 发送端的通信桥梁和接收端的通信桥梁。

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>我命中的桥或协议数量配额限制时，会发生什么情况？

您将不能部署任何新的桥接器，或创建任何新的协议。 若要部署的详细信息，您需要扩展到更多单位的 BizTalk 服务，或升级到较高版本。

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>如何迁移从 BizTalk 服务的一层到另一个？

免费版不能迁移或到另一个层，缩放并不能备份和还原到另一层。 如果您需要另一层，创建新的 BizTalk 服务使用新的层。 需要重新创建新的 BizTalk 服务中使用免费版，包括混合连接创建任何项目。 

剩余的版本中，使用的备份和还原到另一层从迁移您的项目。 例如，备份您的项目中的标准层，，然后将其还原到特优层。 [BizTalk 服务︰ 备份和还原](biztalk-backup-restore.md)描述支持的迁移路径，并列出哪些项目进行备份。 请注意，不备份混合连接。 备份和还原到新的存储层之后, 再重新创建混合连接。  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>是 BizTalk 适配器服务包括在该服务？ 如何接收软件？

是的使用 BizTalk 适配器包的 BizTalk 适配器服务还包括 Azure BizTalk 服务 SDK[下载](http://www.microsoft.com/download/details.aspx?id=39087)。

## <a name="next-steps"></a>下一步行动

若要创建在 Azure 门户 Azure BizTalk 服务，请转到[BizTalk 服务︰ 资源调配使用 Azure 的门户网站](biztalk-provision-services.md)。 若要开始创建应用程序，请转到[Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=235197)。

## <a name="additional-resources"></a>其他资源
- [BizTalk 服务︰ 资源调配使用 Azure 门户](biztalk-provision-services.md)<br/>
- [BizTalk 服务︰ 资源调配的状态图表](biztalk-service-state-chart.md)<br/>
- [BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk 服务︰ 备份和恢复](biztalk-backup-restore.md)<br/>
- [BizTalk 服务︰ 限制](biztalk-throttling-thresholds.md)<br/>
- [BizTalk 服务︰ 颁发者名称和颁发者的密钥](biztalk-issuer-name-issuer-key.md)<br/>
- [我如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
