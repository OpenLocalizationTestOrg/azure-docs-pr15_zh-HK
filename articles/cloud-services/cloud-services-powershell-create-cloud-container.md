<properties
   pageTitle="使用 PowerShell 创建云服务容器 |Microsoft Azure"
   description="本文介绍如何使用 PowerShell 创建云服务容器。 容器承载 web 和辅助的角色。"
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>使用 Azure PowerShell 命令来创建一个空的云服务容器
这篇文章解释了如何快速创建使用 Azure PowerShell cmdlet 的云服务容器。 请按照下列步骤操作︰

1. 从[Azure PowerShell 下载](http://aka.ms/webpi-azps)页上安装 Microsoft Azure PowerShell cmdlet。
2. 打开 PowerShell 命令提示符。
3. [添加 AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx)用于登录。

    > [AZURE.NOTE] 有关安装 Azure PowerShell cmdlet 和连接到 Azure 订阅的详细说明，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

4. 使用**New AzureService** cmdlet 来创建一个空的 Azure 云服务容器。

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. 按照调用该 cmdlet 本示例︰
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

有关创建 Azure 的云服务的详细信息，请运行︰
```
Get-help New-AzureService
```

### <a name="next-steps"></a>下一步行动

 * 要管理云服务部署，请参阅[获取 AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx)、[删除 AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)和[一组 AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx)命令。 您还可能指[如何配置云服务](cloud-services-how-to-configure.md)的详细信息。

 * 要发布到 Azure 的云服务项目，请[在 Azure 的云服务不断提供](cloud-services-dotnet-continuous-delivery.md)的**PublishCloudService.ps1**代码示例。
