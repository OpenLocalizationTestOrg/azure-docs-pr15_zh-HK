<properties 
    pageTitle="发行说明数据管理网关为 |Azure 数据工厂" 
    description="数据管理网关文题目发行说明" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>发行说明，了解数据管理网关
现代数据集成的挑战之一是内部至云与无缝地移动数据。 数据工厂使这种集成与数据管理网关，它是您可以安装内部启用混合数据移动代理无缝。

请参阅以下文章获取有关数据管理网关以及如何使用它的详细信息︰ 

- [数据管理网关](data-factory-data-management-gateway.md)
- [内部之间移动数据和云使用 Azure 数据工厂](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>当前的版本 (2.2.6072.1)

- 设置 HTTP 代理网关使用网关配置管理器的支持。 如果配置，Azure Blob、 Azure 表、 Azure 数据湖和文档数据库可通过 HTTP 代理服务器。
- 支持标头处理格式将数据从到 Azure Blob，Azure 数据湖存储，复制时内部文件系统和内部 HDFS。
- 从追加 Blob 和页面 Blob 已经支持块斑点以及复制数据的支持。
- 引入了一个新的网关状态**在线 （有限）**，这表明除复制向导支持的交互操作，有效的网关的主要功能。
- 增强了网关注册使用注册密钥的可靠性。

## <a name="earlier-versions"></a>早期版本

## <a name="2160401"></a>2.1.6040.1

- DB2 驱动程序现在包括在网关安装程序包中。 您不需要单独安装它。 
- DB2 驱动程序现在支持 z/OS 和 DB2 的我 (AS / 400) 以及已所支持的平台 （Linux、 Unix 和 Windows）。 
- 对于内部数据存储区中使用 DocumentDB 作为源或目标的支持
- 支持将数据复制/冷/热到 blob 以及已经支持通用的存储帐户存储。 
- 允许您连接到本地 SQL Server 通过网关与远程登录权限。  

## <a name="2060131"></a>2.0.6013.1

- 您可以选择要手动安装过程中通过网关使用的语言/区域性。
- 当网关不能按预期方式时，您可以将过去七天内的日志，网关发送给 Microsoft 以帮助进行故障诊断的问题。 如果未连接到云服务网关，您可以选择保存和存档日志网关。  
- 网关配置管理器的用户界面改进︰
    - 在主页选项卡上进行网关状态更明显。
    - 重新组织和简化控件。
- 您可以从使用[代码自由复制预览工具](data-factory-copy-data-wizard-tutorial.md)存储复制数据。 一般情况下看到有关此功能的详细信息的[转移的副本](data-factory-copy-activity-performance.md#staged-copy)。 
- 您可以到 Azure 机器学习使用入站数据直接从本地 SQL Server 数据库的数据管理关。
- 性能的改进
    - 提高性能，无需编码的副本预览工具中查看针对 SQL Server 架构/预览。



## <a name="11259531"></a>1.12.5953.1
- 错误修复

## <a name="11159181"></a>1.11.5918.1

- 网关事件日志最大大小已从 1 MB 增加到了 40 MB。
- 在网关自动更新过程中需要重新启动的情况下，将显示一个警告对话框。 您可以选择重新启动权限然后或更高版本。 
- 出现故障时自动更新，网关安装程序重试自动更新三次最大。
- 性能的改进
    - 提高从无需编码的复制方案中的内部服务器加载大型表的性能。
- 错误修复

## <a name="11058921"></a>1.10.5892.1

- 性能的改进
- 错误修复

## <a name="1958652"></a>1.9.5865.2

- 零接触自动更新功能
- 使用网关状态指示器的新任务栏图标
- "立即更新"从客户端的功能
- 若要设置更新日程安排时间的能力
- 用于切换开/关自动更新 PowerShell 脚本
- JSON 格式的支持  
- 性能的改进
- 错误修复

## <a name="1858221"></a>1.8.5822.1

- 提高故障排除的经验
- 性能的改进
- 错误修复

### <a name="1757951"></a>1.7.5795.1

- 性能的改进
- 错误修复

### <a name="1757641"></a>1.7.5764.1

- 性能的改进
- 错误修复

### <a name="1657351"></a>1.6.5735.1

- 支持内部 HDFS 源/接收器
- 性能的改进
- 错误修复

### <a name="1656961"></a>1.6.5696.1

- 性能的改进
- 错误修复

### <a name="1656761"></a>1.6.5676.1

- 在配置管理器上的支持诊断工具
- 对 Azure 数据工厂表格式数据源支持表中的列
- 支持 SQL DW Azure 数据工厂
- 支持在 BlobSource 和文件源 Reclusive Azure 数据工厂
- 支持 CopyBehavior-MergeFiles、 PreserveHierarchy 和 FlattenHierarchy 在 BlobSink 和 FileSink Azure 数据工厂的二进制副本
- 支持复制活动的 Azure 数据工厂报告进度
- 支持数据源连接验证 Azure 数据工厂
- 错误修复


### <a name="1656721"></a>1.6.5672.1

- Azure 数据工厂的支持 ODBC 数据源的表名称
- 性能的改进
- 错误修复

### <a name="1656581"></a>1.6.5658.1

- 支持文件水池的 Azure 数据工厂
- 支持的 Azure 数据工厂保留二进制副本中的层次结构
- Azure 数据工厂的支持复制活动幂等性
- 错误修复

### <a name="1656401"></a>1.6.5640.1

- Azure 数据工厂 （ODBC、 OData） HDFS 支持 3 多个数据源
- Azure 数据工厂在 csv 分析器中引号字符支持
- 压缩的支持 (BZip2)
- 错误修复

### <a name="1556121"></a>1.5.5612.1

- Azure 数据工厂 （MySQL，PostgreSQL、 DB2、 Teradata 和 Sybase） 支持五个数据库
- 压缩支持 （Gzip 和 Deflate）
- 性能的改进
- 错误修复


### <a name="1455491"></a>1.4.5549.1

- 添加 Oracle 数据源支持的 Azure 数据工厂
- 性能的改进
- 错误修复

### <a name="1454921"></a>1.4.5492.1

- 统一的二进制文件，支持 Microsoft Azure 数据工厂和办公室 365 电源 BI 服务
- 改进了配置用户界面和注册过程
- Azure 数据工厂 – Azure 的入口和出口的支持 SQL Server 数据源

### <a name="1253031"></a>1.2.5303.1

-   解决超时问题，以支持更多的耗时的数据源连接。 
    
### <a name="1155268"></a>1.1.5526.8

- 在安装过程中，.NET Framework 4.5.1 要求的先决条件。

### <a name="1051442"></a>1.0.5144.2

- 没有影响 Azure 数据工厂方案的更改。 
