<properties
    pageTitle="Azure 政府文档 |Microsoft Azure"
    description="这为 Azure 政府开发应用程序提供功能和指导的比较。"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure 政府监视和管理

这篇文章概括介绍了监视和管理服务变化和 Azure 政府环境的考虑因素。

## <a name="automation"></a>自动化

自动化是在 Azure 政府正式提供。

### <a name="variations"></a>变体

以下的自动化功能目前不在 Azure 政府。

+ 创建的服务原则凭据进行身份验证

有关详细信息，请参阅[自动化公用文档](../automation/automation-intro.md)。

## <a name="log-analytics"></a>日志分析

日志分析是一种在 Azure 政府正式提供。

### <a name="variations"></a>变体

以下日志分析功能和解决方案目前不在 Azure 政府。

+ 在预览 Microsoft Azure 中的解决方案包括︰
  - 网络监控解决方案
  - 应用程序依赖项监视解决方案
  - Office 365 的解决方案
  - Windows 10 升级分析解决方案
  - 应用程序的见解解决方案
  - Azure 网络分析解决方案
  - Azure 的自动化分析解决方案
  - 密钥存储库分析解决方案
+ 解决方案和需要对内部部署软件更新的功能，包括︰
  - 与系统中心操作管理器 2016 （支持早期版本的操作管理器） 集成
  - 从系统中心配置管理器的计算机组
  - 曲面中心解决方案
+ 在公用 Azure，预览中的功能包括︰
  - 为电源 BI 数据的导出
+ Azure 指标和 Azure 的诊断
+ 操作管理套件移动应用程序

日志分析的 Url 是 Azure 政府中不同的︰

| Azure 公共 | Azure 的政府 | 备注 |
|--------------|------------------|-------|
| mms.microsoft.com | oms.microsoft.us | 日志分析门户 |
| *workspaceId*。 ods.opinsights.azure.com | *workspaceId*。 ods.opinsights.azure.us | [数据收集器 API](../log-analytics/log-analytics-data-collector-api.md) 
| \*。 ods.opinsights.azure.com | \*。 ods.opinsights.azure.us | 代理通讯的[配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |
| \*。 oms.opinsights.azure.com | \*。 oms.opinsights.azure.us | 代理通讯的[配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |
| \*。 blob.core.windows.net | \*。 blob.core.usgovcloudapi.net | 代理通讯的[配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |


以下日志分析功能在 Azure 政府行为以不同的方式︰

+ Windows 代理必须从 Azure 政府[日志分析门户](https://oms.microsoft.us)。
+ 若要连接到日志分析系统中心操作管理器管理服务器，您需要下载并导入更新的管理包。
  1. 下载并保存[更新的管理包](http://go.microsoft.com/fwlink/?LinkId=828749)。
  2. 解压缩已下载的文件。
  3. 将管理包导入操作管理器。 有关如何从磁盘导入管理包的信息，请参阅 Microsoft TechNet 网站上的[导入操作管理器管理包的方式](http://technet.microsoft.com/library/hh212691.aspx)。
  4. 要连接到日志分析的操作管理器，请按照[连接操作管理器日志分析](../log-analytics/log-analytics-om-agents.md)。


## <a name="frequently-asked-questions"></a>常见问题及的解答

+ 可以将数据迁移从 Microsoft Azure 中的日志分析到 Azure 政府？
  - 不。 不能将您的工作区或数据从 Microsoft Azure 移到 Azure 政府。
+ 可以切换 Microsoft Azure 和 Azure 政府之间在操作管理套件日志分析门户工作区？
  - 不。 Microsoft Azure 和 Azure 政府门户网站是独立的并且不共享信息。

有关详细信息，请参阅[日志分析公共文档](../log-analytics/log-analytics-overview.md)。

## <a name="next-steps"></a>下一步行动

有关的补充信息和更新，订阅<a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure 的政府博客。</a>
