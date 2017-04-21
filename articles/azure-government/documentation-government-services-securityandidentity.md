<properties
    pageTitle="Azure 政府文档 |Microsoft Azure"
    description="这为 Azure 政府开发应用程序提供功能和指导的比较"
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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure 政府安全和身份

##  <a name="key-vault"></a>密钥存储库
有关此服务以及如何使用它的详细信息，请参阅<a href="https://azure.microsoft.com/documentation/services/key-vault">Azure 密钥存储库公共文档。</a>

### <a name="data-considerations"></a>数据的注意事项
以下信息标识 Azure 政府边界 Azure 密钥存储库︰

| 允许调节控制的数据 | 不允许使用的管制控制数据 |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 使用 Azure 密钥存储库密钥加密所有数据可能都包含受管制/控制数据。 | Azure 的密钥存储库元数据不能包含控制导出数据。 此元数据包括创建和维护您的密钥存储库时输入的所有配置数据。  不要在以下字段输入受管制/控制数据︰ 资源组名、 密钥存储库名称、 订阅名称 |

密钥存储库是在 Azure 政府正式提供。 与大众，是没有扩展名，过程，因此密钥存储库仅可通过 PowerShell 和 CLI。

## <a name="next-steps"></a>下一步行动

有关的补充信息和更新，订阅<a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure 的政府博客。</a>
