
<properties 
   pageTitle="对于.NET 2.7 和.NET 2.7.1 azure SDK 发行说明" 
   description="对于.NET 2.7 和.NET 2.7.1 azure SDK 发行说明" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>对于.NET 2.7 和.NET 2.7.1 azure SDK 发行说明

##<a name="overview"></a>概述

此文档包含.NET 2.7 版本的 Azure sdk 发行说明。 

文档还包含的发行说明的 Azure sdk 为.NET 2.7.1 释放。

在 Visual Studio 2015年和 Visual Studio 2013年才支持 azure SDK 2.7。 [Azure SDK 2.6](https://azure.microsoft.com/downloads/)是最后一个支持 Visual Studio 2012 SDK。

有关此版本的详细信息，请参阅[发布 Azure SDK 2.7 公告](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)和[Azure SDK 2.7.1 公告发布](http://go.microsoft.com/fwlink/?LinkId=623850)。

##<a name="azure-sdk-for-net-27"></a>使.NET 2.7 的 azure SDK

###<a name="sign-in-improvements-for-visual-studio-2015"></a>在 Visual Studio 2015年的改进登录

对于 Visual Studio 2015 azure SDK 2.7 支持 Visual Studio 2015年新身份管理功能。  这包括访问通过基于角色的访问控制、 云解决方案提供商，DreamSpark 和其他帐户和订阅类型的 Azure 帐户的支持。

只提供 Visual Studio 2015 Azure SDK 2.7 中包含的符号中改进了。 Azure 2.7.1 SDK 中包含 Visual Studio 2013年支持。


###<a name="mobile-sdk"></a>移动 SDK

更新以反映最新的[NuGet 程序包](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/)和安装程序过程的**移动应用程序**模板。

###<a name="service-bus"></a>服务总线 

常规错误修正和改进。 有关更新和功能的详细信息，请参阅发行说明的最新[服务总线 NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/)。

###<a name="hdinsight-tools"></a>HDInsight 工具 

在此版本中进行了以下更新程序。 这些更新将在预览。 有关详细信息，请参见[以下博客](http://go.microsoft.com/fwlink/?LinkId=619108)。

- 在 Tez 作业配置单元的配置单元图表
- 完全配置单元 DML IntelliSense 支持
- 猪的模板支持
- 风暴的 Azure 服务的模板

####<a name="breaking-changes"></a>重大更改

- 使用此版本的工具时，必须升级旧**风暴**项目。 有关详细信息，请参见[以下博客](http://go.microsoft.com/fwlink/?LinkId=619108)。
- 不再支持 Visual Studio Web 表示。 有关详细信息，请参见[以下博客](http://go.microsoft.com/fwlink/?LinkId=619108)。

###<a name="azure-app-service-tools"></a>Azure 应用程序服务工具

在此版本中对 Web 工具扩展进行了以下更新程序。 有关详细信息，请参见[以下](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)博客。 

- 对添加的 DreamSpark 帐户的支持
- 完全对 Azure 工具更改以支持新的 Azure 资源管理 Api
- 对 Azure 应用程序服务的支持添加到[云资源管理器](#cloud_explorer)

####<a name="known-issues"></a>已知的问题

Web 应用程序部署插槽节点出现在服务器资源管理器中的插槽节点和 Web 应用程序部署槽子节点不加载下云资源管理器。 此问题已解决，并做好下一步的 SDK 发行。 


###<a name="cloud_explorer"></a>云资源管理器中的 Visual Studio 2015

Azure SDK 2.7 包括云资源管理器中的 Visual Studio 2015，它使您能够查看您的 Azure 资源，检查它们的属性，并执行关键的开发人员在 Visual Studio 中的操作。 

云资源管理器支持以下功能︰

- Azure 资源的资源组和资源类型视图 
- 按名称 （可在资源类型视图） 中搜索资源
- 支持订阅和具有角色基于访问控制 (RBAC) 应用资源 
- 集成的操作面板显示了特定于所选的资源专注于开发人员的操作。 例如︰ 将远程调试器附加的虚拟机创建使用资源管理器堆栈中，查看虚拟机等的诊断数据。
- 集成的属性面板，它显示在开发/测试中通常所需的专注于开发人员的属性 
- 快速切换时要使用帐户枚举资源 （使用工具栏上的设置命令） 
- 筛选的订阅枚举的资源 （使用工具栏的设置命令） 时，使用 
- 深层链接到 Azure 门户管理资源和资源组 
 
 
###<a name="azure-resource-manager-tools"></a>Azure 的资源管理器工具 

已更新 Azure 资源管理器工具来处理角色基于访问控制 (RBAC) 和新的订阅类型。  包括使用这些更改是为了能够使用新的存储帐户，除了传统的存储，在部署过程中存储的项目。  

如果您使用 SDK 2.7 使用 Azure 资源组项目从早期版本的 SDK，新的部署脚本时需要使用新的存储帐户，而不传统的存储部署。  进行更改之前向项目中添加新的脚本，您将被提示。  将重命名旧的脚本，您将需要手动进行对新脚本进行任何修改。
 
 
###<a name="storage-explorer-tools"></a>存储资源管理器工具 

- 查看附加的 Blob 的支持。 在[此日志](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx)中的详细信息。 
- 查看通过服务器资源管理器的最优存储帐户的支持。 服务器资源管理器将只显示页面 blob 的最优存储帐户就是高级存储帐户的唯一受支持的类型。

###<a name="azure-data-factory-tools-for-visual-studio"></a>Visual Studio 的 azure 数据工厂工具 

对于 Visual Studio 推出**Azure 数据工厂工具**。 下面列出了启用的功能。 [此博客](http://go.microsoft.com/fwlink/?LinkId=617530)的详细信息，请参阅。

- **基于模板创作**︰ 使用大小写格式的选择基于模板、 数据移动模板或数据处理模板开始动手用数据工厂快速地部署的端到端数据集成解决方案。 
- **与解决方案资源管理器用于创作和部署数据工厂实体集成**︰ 创建和部署管线和相关的实体为 Visual Studio 项目。 
- **在创作时的视觉交互查看图与集成**︰ 以可视方式管线和帮助在关系图视图中的数据集。 
- **使用服务器资源管理器以进行浏览和使用已部署实体交互集成**︰ 利用服务器资源管理器中浏览已部署数据工厂和相应的图元。 部署的数据工厂或任何实体 （管道，链接服务，数据集） 导入到您的项目。 
- **JSON 使用架构验证和丰富智能感知编辑**︰ 有效地配置和编辑带有丰富的智能感知和架构验证数据工厂实体的 JSON 文档 
- **多环境发布**︰ 发布通过创建单独的配置文件为每个环境创作到开发、 测试或生产环境的管线。
- **基于数据处理支持小猪、 配置单元和.Net**︰ 对猪和配置单元数据工厂项目中的脚本的支持。 对管理.Net 引用 C# 项目的支持活动。

##<a name="azure-sdk-for-net-271"></a>使.NET 2.7.1 的 azure SDK

下一节包含有关.NET 2.7.1 释放了 Azure SDK 的更新。
###<a name="hdinsight-tools"></a>HDInsight 工具 

HDInsight 工具的更新的更多详细说明，请参见[以下博客](http://go.microsoft.com/fwlink/?LinkId=623831)。

- 配置单元作业操作员视图 （新功能）

    为了帮助您理解您的配置单元查询更好、 配置单元运算符视图添加了功能。 若要查看一个顶点中的所有运算符，请双击作业图的顶点上。 若要查看特定的运算符的更多详细信息，请将鼠标悬停在该运算符。
- 配置单元错误标记 （新功能）

    使您可以查看语法错误立即配置单元错误标记添加了功能。 此外，错误消息已增强，现在可以立即看到详细的语法错误 （直到此版本中，您必须提交到群集配置单元脚本并等待一些时间才会获取错误消息的详细信息）。  
- 冲击拓扑图 （新功能）

    可视化是非常重要的当您想要查看您的拓扑是否按预期方式工作。 在此发行版中我们添加了风暴图的可视化。 您可以为您的拓扑可视化的重要指标 （例如，一种颜色表示天气某些螺栓是否"忙"）。 您也可以双击螺栓/管口以查看更多详细信息。

- 在 Azure 门户 （修复 bug） 中创建的 HDInsight 群集的支持

    您现在可以使用 Visual Studio 查看并将作业提交到无论在其中创建群集的所有 HDInsight 群集。

- 更多的 IntelliSense 支持和元数据配置单元更快地加载 （改进）

    我们已经通过添加更多用户友好建议改进 IntelliSense。 例如，表的别名可以现在还将建议在 IntelliSense 中以便您可以更轻松地编写您的查询。 此外，我们已经改进，只需将花费几秒钟的时间，若要列出所有的数据库、 表和列的配置单元 metastore 加载的配置单元元数据。

HDInsight 工具的更新的更多详细说明，请参见[以下博客](http://go.microsoft.com/fwlink/?LinkId=623831)。

###<a name="improvements-in-visual-studio-2013"></a>在 Visual Studio 2013年的改进

- Azure SDK 2.7.1 使 Visual Studio 2013 访问 Azure 帐户和通过基于角色的访问控制、 云解决方案提供商和 Dreamspark 的订阅。
- Azure sdk 2.7.1，新的云资源管理器工具窗口现还在 Visual Studio 2013年。

###<a name="known-issues"></a>已知的问题

Azure SDK 2.6 或 2.7.1 为安装 Visual Studio 社区 2013年在非英语操作系统将显示 Visual Studio 英语和非英语资源可能不匹配的警告。 可以安全地关闭此警告。 它才会发生如果机器不包含的以前安装的 Visual Studio 社区 2013年并在非英语操作系统安装 SDK。 语言包应用到 Visual Studio，RTM 资源之后，但其应用更新 4 之前显示警告。 消除此警告将允许继续并完成应用语言包内容的更新 4 版本的语言包。

LightSwitch 项目并不是此版本中。 此问题将得到解决下一步的 SDK 发行版。

##<a name="also-see"></a>另请参阅
[Azure SDK 2.7.1 发布公告](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 发布公告](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[支持和 Azure SDK 以及.NET Api 的退休信息](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
