<properties
   pageTitle="什么是 Azure Analysis Services |Microsoft Azure"
   description="在 Azure 中获得 Analysis Services 的大框架。"
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="what-is-azure-analysis-services"></a>Azure Analysis Services 是什么？
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

构建于经验证的分析引擎在 Microsoft SQL Server Analysis Services，Azure Analysis Services 提供企业级的数据建模在云中。

> [AZURE.IMPORTANT] 在**预览**中是 azure Analysis Services。 有一些只还不能工作。 请务必检查出[预览期望](#preview-expectations)本文中稍后介绍。 而且，一定要关注我们的[Azure Analysis Services 博客](https://go.microsoft.com/fwlink/?linkid=830920)的最新信息。

## <a name="built-on-sql-server-analysis-services"></a>基于 SQL Server Analysis Services
Azure Analysis Services 是同一 SQL Server 2016 Analysis Services 企业版已经知道与兼容。 Azure Analysis Services 支持 1200年兼容性级别表格模型。 所有支持 DirectQuery、 分区、 行级别安全性，双向的关系和翻译。

## <a name="use-the-tools-you-already-know"></a>使用您已经知道的工具
![BI 的开发人员工具](./media/analysis-services-overview/aas-overview-dev-tools.png)

时为 Azure Analysis Services 创建数据模型，您可以使用相同的工具与 SQL Server Analysis Services。 创作和使用最新版本的[SQL Server 数据工具 (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)或通过[SQL Server 管理 Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)中使用[Azure Powershell](../powershell-install-configure.md)和[Azure 资源管理器](../azure-resource-manager/resource-group-overview.md)模板部署表格模型。

## <a name="connect-to-data-sources"></a>连接到数据源
数据模型部署到 Azure 支持连接到数据源上部署您的组织中或在云中的服务器。 合并来自内部部署和云混合体 BI 解决方案的数据源。

![数据源](./media/analysis-services-overview/aas-overview-data-sources.png)

因为您的服务器是在云环境中，连接到云数据源是紧密相连的。 连接到内部数据源时，[内部数据网关](analysis-services-gateway.md)可确保快速、 安全的连接与 Analysis Services 服务器群中。  

 \*某些数据源在预览中尚不支持。 若要了解详细信息，请参阅本文后面的[预览的期望](#preview-expectations)。

## <a name="explore-your-data-from-anywhere"></a>从任何地方浏览您的数据
连接并从有关任何地方从服务器中[获取数据](analysis-services-connect.md)。 Azure Analysis Services 支持连接电源双桌面、 Excel、 自定义的应用程序和基于浏览器的工具。

![数据可视化效果](./media/analysis-services-overview/aas-overview-visualization.png)

 \*电源双嵌入式尚不支持预览中。

## <a name="secure"></a>安全

#### <a name="user-authentication"></a>用户身份验证
Azure 分析服务的用户身份验证是通过[Azure 活动目录 (AAD)](../active-directory/active-directory-whatis.md)处理的。 当尝试登录到 Azure Analysis Services 数据库，用户将使用有权访问它们尝试访问的数据库组织帐户标识。 这些用户的身份必须是默认的订阅 Azure Active Directory 的成员 Azure Analysis Services 服务器所在。 [目录集成](https://technet.microsoft.com/library/jj573653.aspx)AAD 和内部部署 Active Directory 之间是获得您的内部用户访问到 Azure Analysis Services 的数据库，一个好办法，但不是所必需的所有方案。

用户登录使用他们的帐户和密码的用户主体名称 (UPN)。 当与内部部署 Active Directory 同步，其组织的电子邮件地址通常是用户的 UPN。

通过将用户分配到 Azure 订购中的角色处理管理 Azure Analysis Services 服务器资源的权限。 默认情况下，订阅管理员 Azure 中具有对服务器资源的所有者权限。 可以通过使用 Azure 资源管理器中添加其他用户。

#### <a name="data-security"></a>数据安全性
Azure Analysis Services 利用 Azure Blob 存储以保持存储和 Analysis Services 数据库的元数据。 在 Blob 数据文件使用 Azure Blob 服务器端加密 (SSE) 进行加密。 使用直接查询模式时，仅将元数据存储;在查询时从数据源访问的实际数据。

#### <a name="on-premises-data-sources"></a>内部数据源
安全地访问数据，可以通过安装和配置[部署数据网关](analysis-services-gateway.md)实现驻留在部署您的组织中。 网关直接查询和内存中模式提供对数据的访问。 当 Azure Analysis Services 模型连接到内部数据源时，以及加密凭据的内部数据源创建查询。 网关云服务分析查询并将该请求推送到 Azure 服务总线。 内部网关进行轮询的 Azure 服务总线挂起的请求。 网关然后获取查询、 解密凭据，并连接到数据源进行执行。 回网关，然后到 Azure Analysis Services 数据库从该数据源，然后发送结果。

Azure Analysis Services 受[Microsoft Online Services 条款](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)和[Microsoft Online Services 的隐私声明](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx)。
若要了解有关 Azure 安全的详细信息，请参阅[Microsoft 信任中心](https://www.microsoft.com/trustcenter/Security/AzureSecurity)。

## <a name="get-help"></a>获取帮助
Azure Analysis Services 是简单的设置和管理。 您可以找到您需要创建并管理此服务器的所有信息。 创建要部署到服务器的数据模型，时非常相似是创建数据模型部署到本地服务器。 没有一个详尽的库的概念、 过程教程，并在[Analysis Services 在 MSDN 上](https://msdn.microsoft.com/library/bb522607.aspx)的参考文章。

在[Azure Analysis Services 第 9 频道上](https://channel9.msdn.com/series/Azure-Analysis-Services)，我们还提供大量有用的视频。

事情正在迅速改变。 您总是可以获得最新信息在[Azure Analysis Services 的博客](https://go.microsoft.com/fwlink/?linkid=830920)上。

## <a name="community"></a>社区
Analysis Services 有活跃的用户社区。 在[Azure Analysis Services 论坛](https://aka.ms/azureanalysisservicesforum)加入该对话。

## <a name="feedback"></a>反馈
有的建议或功能请求吗？ 请务必在[Azure Analysis Services 反馈](https://aka.ms/azureanalysisservicesfeedback)上留下您的意见。

有相关的文档的建议吗？ 您可以添加在每个文章的底部使用 Disqus 评论。

## <a name="preview-expectations"></a>预览的期望
正在预览 azure Analysis Services。 有几件事情您应该知道。

##### <a name="server-modes"></a>服务器模式
Azure Analysis Services 目前 1200年兼容性级别表格模型支持表格模式。 不支持多维和数据挖掘模式中，并且电源轴的 SharePoint 模式。

##### <a name="data-sources"></a>数据源
预览，表格 1200年模型部署到 Azure Analysis Services 服务器支持下列数据源。

| **云** | **内部部署** |
|--------------|------------|
| SQL 数据库 | SQL Server |
| SQL 数据仓库 | 接入点 |
|      | Oracle |
|       | Teradata |


### <a name="data-source-providers"></a>数据源提供程序
在 Azure Analysis Services 的数据模型可能需要不同的数据提供程序用来连接到比 SQL Server Analysis Services 中数据模型的数据源。 数据提供程序要求取决于数据源是在云上部署和类型的数据模型;内存中或直接查询。 若要了解详细信息，请参阅[数据源的连接](analysis-services-datasource.md)。


### <a name="client-connections"></a>客户端连接
电源双嵌入式尚不支持预览中。

不支持到 Azure Analysis Services 服务器并保存在 OneDrive 或 SharePoint Online 的活连接的 Excel 工作簿。



## <a name="next-steps"></a>下一步行动
现在，您知道更多关于 Azure Analysis Services，就可以开始。 了解如何在 Azure 和向其[部署表格模型](analysis-services-deploy.md)中[创建一个服务器](analysis-services-create-server.md)。
