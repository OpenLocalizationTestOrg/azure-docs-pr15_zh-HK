<properties 
   pageTitle="用于.NET 2.6 azure SDK 发行说明" 
   description="用于.NET 2.6 azure SDK 发行说明" 
   services="app-service/web" 
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

 
# <a name="azure-sdk-for-net-26-release-notes"></a>用于.NET 2.6 azure SDK 发行说明

此文档包含.NET 2.6 版本的 Azure sdk 发行说明。 

使用 Azure SDK 2.6 可以开发云服务应用程序 (PaaS) 面向.NET 4.5.2 或.NET 4.6，前提在云服务角色上手动安装.NET Framework 的目标。 请参阅[安装.NET 在云服务角色](http://go.microsoft.com/fwlink/?LinkID=309796)。


##<a name="service-bus-updates"></a>服务总线的更新

- 事件集线器︰ 

    - 通过事件集线器的公开其它发布服务器终结点发送事件时，现在允许目标的访问控制。
    - 额外的稳定性和添加到事件集线器功能的改善。
    - 添加通过 WebSocket Amqp 协议的支持，消息和事件集线器。

##<a name="hdinsight-tools-for-visual-studio-updates"></a>Visual Studio 的 HDInsight 工具更新

- **IntelliSense 增强**︰ 远程元数据的建议

    现在 HDInsight Visual Studio 工具支持获取远程元数据编辑您的配置单元的脚本时。 例如，您可以在其中键入**选择*发** 所有表名都将显示。 此外，指定表后，将显示列名称。

- **HDInsight 仿真器支持**

    现在 HDInsight Visual Studio 工具支持连接到仿真程序 HDInsight，以便您可以本地开发配置单元脚本，而不会引入任何成本，然后执行这些脚本针对 HDInsight 群集。 

    有关详细信息，请参阅[本手册](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)。

- **HDInsight Visual Studio 工具支持泛型的 Hadoop 群集**（预览）

    现在 HDInsight Visual Studio 工具支持泛型的 Hadoop 群集，以便可以使用 Visual Studio 的 HDInsight 工具执行以下︰

    - 连接到群集， 
    - 编写配置单元查询与增强支持 IntelliSense/自动完成， 
    - 在群集中使用直观的用户界面查看所有的作业。 

    有关详细信息，请参阅[本手册](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)。

##<a name="in-role-cache-updates"></a>在角色缓存更新

- **在角色缓存**被更新为使用**Microsoft Azure 存储 SDK**版本 4.3。 到目前为止，**在角色缓存**使用 Azure 存储 SDK 版本 1.7。

    客户使用 Azure SDK 2.5 或以下应更新到 Azure SDK 2.6 并移到 Azure 存储 SDK 的新版本。 

    这一次 Azure 存储版本 2011年-08-18 计划要移除 2016，8 月 1。 在角色缓存从 Azure SDK 2.5 或以下任何迁移到 2.6 必须完成此时间。 退出使用 Azure 存储版本 2011年-08-18 的详细信息，请参阅[Microsoft Azure 存储服务版本删除更新︰ 到 2016年扩展名](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)。

>[AZURE.IMPORTANT]我们在宣布 2016，11 月 30，退休 Azure 托管缓存服务和 Azure 角色缓存。 我们建议您在此退休准备迁移到 Azure Redis 缓存。 有关日期和迁移指南的详细信息，请参阅[的 Azure 缓存产品最适合我？](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Azure 应用程序服务工具

更新了以下各项在 Azure SDK 2.6 版本中。

- Azure 发布增强包括 Azure API 应用程序作为部署目标。
- API 的应用程序资源调配功能使用 API 的应用程序创建和配置功能使用户。
- 服务器资源管理器更改以反映新的应用程序服务节点，与按资源组分组 Web、 移动设备、 和 API 的应用程序。
- 添加添加到大多数 C# 项目，以使自动生成的 Swagger 启用 API 应用程序运行在用户的 Azure 订阅的 Azure 的 API 的应用程序客户端笔势。
- API 应用程序工具和应用程序服务在服务器资源管理器中的节点可在 Visual Studio 2013年只。 

##<a name="azure-resource-manager-tools-updates"></a>Azure 的资源管理器工具更新

已更新的 Azure 的资源管理器工具中要包含的虚拟机、 网络和存储的模板。 编辑体验 JSON 已更新以包括新大纲视图模板和编辑使用 JSON 代码段模板的能力。 从 Visual Studio 部署模板使用 PowerShell 脚本附带项目，以便对该脚本所做的任何更改将由 Visual Studio。

##<a name="diagnostics-improvements-for-cloud-services"></a>云服务的诊断程序改进

Azure SDK 2.6 回将收集在 Azure 计算仿真程序的诊断日志，并将其传输到开发存储的支持。 任何诊断日志 （包括应用程序跟踪日志、 事件日志和 windows 事件日志跟踪 (ETW) Windows 日志、 性能计数器、 基础结构） 生成应用程序在仿真程序中的运行时可以将转移到开发存储，以验证您的诊断日志记录在您的本地计算机上工作。 

现在可以更方便地对不同的环境中使用不同诊断存储帐户的服务配置 (.cscfg) 文件中指定诊断存储帐户。 有连接字符串从事 Azure SDK 2.4 和 Azure SDK 2.6 之间存在显著差异。 有关如何使用诊断存储连接字符串和它如何影响您的项目请参见[Azure 云服务配置诊断](http://go.microsoft.com/fwlink/?LinkID=532784)。

##<a name="breaking-changes"></a>重大更改

###<a name="azure-resource-manager-tools"></a>Azure 的资源管理器工具 

- 已重命名**云部署项目**的项目类型可以在 Azure SDK 2.5 到**Azure 的资源组**。
- 可以使用**云部署项目**类型的项目创建在 Azure SDK 2.5 2.6 中但从 Visual Studio 模板部署将失败。 但是，使用 PowerShell 脚本部署仍然起作用像以前那样。  如需有关如何使用**云部署项目**2.6 中阅读[张贴](http://go.microsoft.com/fwlink/?LinkID=534086)此。
 
##<a name="known-issues"></a>已知的问题

- 收集在仿真器中的诊断日志需要一个 64 位的操作系统。 在 32 位操作系统上运行时，将不会收集诊断日志。 这不会影响仿真程序的任何其他功能。 

- 在 4/29/2015年发布 azure SDK 2.6 有两个问题︰ 

    - Azure SDK 2.6 安装在计算机上时，不能在 Visual Studio 2015年加载通用的应用程序。
    - 在 Visual Studio 2013年和 Visual Studio 2015，Visual Studio 将变成无响应和崩溃时显示消息"仿真程序的配置诊断"对话框中，调试一个云服务项目会失败。
    
    5/18/2015年发布 Azure SDK 2.6 的更新。 更新后的版本是 2.6.30508.1601;它包含针对上述两个问题的修补程序。 您可以标识从控制面板的 sdk 生成-> 程序和功能-> Microsoft Azure 工具为 Microsoft Visual Studio 2013-v 2.6。 版本列将显示生成号。

    如果您仍面临上述问题， [VS 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409)、 [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)或[VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)安装最新版本的 Azure 2.6 SDK。
 
##<a name="see-also"></a>请参见

[支持和 Azure SDK 以及.NET Api 的退休信息](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
