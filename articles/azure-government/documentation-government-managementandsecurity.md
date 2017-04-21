<properties
    pageTitle="Azure 政府文档 |Microsoft Azure"
    description="这为 Azure 政府开发应用程序提供功能和指导的比较"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure 的政府管理和安全性

## <a name="automation"></a>自动化

自动化是在 Azure 政府正式提供。

### <a name="variations"></a>变体

以下的自动化功能目前不在 Azure 政府。

+ 创建的服务原则凭据进行身份验证

有关详细信息，请参阅[自动化公用文档](../automation/automation-intro.md)。


##  <a name="key-vault"></a>密钥存储库
有关此服务以及如何使用它的详细信息，请参阅<a href="https://azure.microsoft.com/documentation/services/key-vault">Azure 密钥存储库公共文档。</a>
### <a name="data-considerations"></a>数据的注意事项
以下信息标识 Azure 政府边界 Azure 密钥存储库︰

| 允许调节控制的数据 | 不允许使用的管制控制数据 |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 使用 Azure 密钥存储库密钥加密所有数据可能都包含受管制/控制数据。 | Azure 的密钥存储库元数据不能包含控制导出数据。 此元数据包括创建和维护您的密钥存储库时输入的所有配置数据。  不要在以下字段输入受管制/控制数据︰ 资源组名、 密钥存储库名称、 订阅名称 |

密钥存储库是在 Azure 政府正式提供。 与大众，是没有扩展名，过程，因此密钥存储库仅可通过 PowerShell 和 CLI。
## <a name="log-analytics"></a>日志分析
日志分析是一种在 Azure 政府正式提供。 

### <a name="variations"></a>变体

以下日志分析功能和解决方案目前不在 Azure 政府。 这会更新列表时功能的状态 / 解决方案的更改。

+ 在公用 Azure，预览中的解决方案包括︰
  - 网络监控解决方案
  - 监视应用程序依赖项
  - Office 365 的解决方案
  - Windows 10 升级分析解决方案
  - 应用程序的见解
  - Azure 网络分析解决方案
  - Azure 的自动化分析
  - 密钥存储库分析
+ 解决方案和需要对内部部署软件更新的功能，包括
  - 与系统中心操作管理器 2016 （支持早期版本的操作管理器） 集成
  - 计算机组从系统中心配置管理器
  - 曲面中心解决方案
+ 在公用 Azure，预览中的功能包括
  - 导出的数据复制到 PowerBI
+ Azure 指标和 Azure 的诊断
+ OMS 手机应用程序

日志分析的 Url 是 Azure 政府中不同的︰

| Azure 公共 | Azure 的政府 | 备注 |
|--------------|------------------|-------|
| mms.microsoft.com | oms.microsoft.us | 日志分析门户 |
| *workspaceId*。 ods.opinsights.azure.com | *workspaceId*。 ods.opinsights.azure.us | [数据收集器 API](../log-analytics/log-analytics-data-collector-api.md) 
| \*。 ods.opinsights.azure.com | \*。 ods.opinsights.azure.us | 代理通讯的[配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |
| \*。 oms.opinsights.azure.com | \*。 oms.opinsights.azure.us | 代理通讯的[配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |
| \*。 blob.core.windows.net | \*。 blob.core.usgovcloudapi.net | 代理通讯的[配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |


以下日志分析功能在 Azure 政府具有不同的行为︰

+ Windows 代理必须从 Azure 政府[日志分析门户](https://oms.microsoft.us)。
+ 若要连接到日志分析系统中心操作管理器管理服务器，您需要下载并导入更新的管理包。
  1. 下载并保存[更新管理包](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. 解压缩已下载的文件
  3. 将管理包导入操作管理器。 有关如何从磁盘导入管理包的信息，请参阅 Microsoft TechNet 网站上[如何操作管理器管理包导入](http://technet.microsoft.com/library/hh212691.aspx)主题。
  4. 要连接到日志分析的操作管理器，请按照[连接到日志分析的运营经理](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>常见问题及的解答

+ 可以将数据迁移从日志分析中公用的 Azure 到 Azure 政府？
  - 不。 不能从公共 Azure 到 Azure 政府移动数据或您的工作区。
+ 可以切换从 OMS 日志分析门户的公用 Azure 和 Azure 政府工作区？
  - 不。 公共 Azure 和 Azure 政府门户网站是独立的并不共享信息。 

有关详细信息，请参阅[日志分析公共文档](../log-analytics/log-analytics-overview.md)。

## <a name="next-steps"></a>下一步行动

有关的补充信息和更新，订阅<a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure 的政府博客。</a>
 
