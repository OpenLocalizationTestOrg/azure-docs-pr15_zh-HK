<properties
    pageTitle="在 Azure 堆栈中的新增功能 |Microsoft Azure"
    description="在 Azure 堆栈中的新增功能"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>在 Azure 堆栈技术预览 2 中的新增功能
此发行版提供了承租人和管理员的新功能。

## <a name="network"></a>网络   
   - [idn，则](azure-stack-understanding-dns-in-tp2.md)提供内部网络名称注册和没有附加 DNS 基础结构的域名系统 (DNS) 解析。
   - [虚拟网络网关](azure-stack-create-vpn-connection-one-node-tp2.md)提供了 Azure 或内部资源到 VPN 的连接选项。
   - 用户定义的路由可以路由网络通信通过防火墙、 安全、 其他电器，以及其它服务。
   - 您可以从市场上创建网络资源。   

## <a name="storage"></a>存储
 - [Azure 队列](https://msdn.microsoft.com/library/dd179353.aspx)启用可靠和持久的服务消息。
 - [存储分析](https://msdn.microsoft.com/library/azure/hh343270.aspx)捕获存储性能数据。 可以使用此数据来跟踪请求，分析使用情况趋势、 并诊断问题与您的存储帐户。
 - Blob 存储支持[追加块操作](https://msdn.microsoft.com/library/azure/mt427365.aspx)。
 - 高级存储 API 客户支持。
 - 租户常见工具和 Sdk，如 Azure CLI、 PowerShell、.NET、 Python 和 Java SDK 的存储服务的支持。 
 - [存储帐户共享访问签名](https://msdn.microsoft.com/library/azure/mt584140.aspx)而无需共享完整帐户密钥启用访问存储服务的精确委派。  
 - 存储服务现在使用低管理开销的强安全性使用[托管服务帐户的组](https://technet.microsoft.com/library/hh831477.aspx)。
 - 您可以回收未使用的租户资源点播。
 - 已删除的存储帐户保留长度是可配置的。
 - 您可以恢复已删除的租户存储帐户。

## <a name="compute"></a>计算
- 您可以取消分配的虚拟机。
- 您可以重新部署虚拟机扩展故障诊断和配置管理的目的。
- 您可以调整虚拟机磁盘的大小。
- 虚拟机可以具有多个网络接口。

### <a name="portal-experience"></a>门户体验
 - Azure 堆栈区域是规模和管理 Azure 堆栈中的逻辑单元。 在此预览中，可以在服务，如计算、 网络和存储区域来查看信息。
 - 现在，您可以预览 （更新） [azure 堆栈 updates.md] 接口。

## <a name="key-vault"></a>密钥存储库
- [Azure 堆栈中的密钥存储库](azure-stack-kv-intro.md)提供键和云应用程序的密码的安全管理。
- 您可以审核和监视的应用程序和虚拟机的密钥用法。

## <a name="billing-and-usage"></a>帐单邮寄地址和用法
 - [帐单邮寄地址和消耗的 Api](azure-stack-billing-and-chargeback.md)公开上如何使用您的服务的数据。  
 - 委派的提供程序使转销商向其客户提供堆栈 Azure 服务。

## <a name="monitoring-and-health"></a>监视和运行状况
 - 新监视门户和 Api 中可用的功能，可以预先查看和管理您的环境的通知。  

## <a name="next-steps"></a>下一步行动
- [了解 Azure 堆栈 POC 体系结构](azure-stack-architecture.md)      
- [了解部署系统必备组件](azure-stack-deploy.md)
- [部署 Azure 堆栈](azure-stack-run-powershell-script.md)

  
