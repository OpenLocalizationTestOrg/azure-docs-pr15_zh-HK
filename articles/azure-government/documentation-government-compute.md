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
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Azure 政府计算

##  <a name="virtual-machines"></a>虚拟机

有关此服务以及如何使用它的详细信息，请参阅[Azure 虚拟机大小](../virtual-machines/virtual-machines-windows-sizes.md)。

### <a name="variations"></a>变体

以下的 VM Sku 是 Azure 政府机构普遍适用 (GA):

VM SKU|美国 Gov VA|美国 Gov IA|备注
---|---|---|---
一个|GA|GA|无
Dv1|GA|-|无
DSv1|GA|-|无
Dv2|GA|GA|15 即将登场
F|GA|GA|无
G|计划|-|无

###  <a name="data-considerations"></a>数据的注意事项

以下信息标识 Azure 政府边界的 Azure 虚拟机︰

| 允许调节控制的数据 | 不允许使用的管制控制数据 |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 数据输入、 存储和处理在一个虚拟机可以包含控制导出数据。 在 Azure 的虚拟机运行的二进制文件。 静态身份验证器，如密码和 Azure 平台组件访问智能卡 Pin。 用于管理 Azure 平台组件的证书的私钥。 SQL 连接字符串。  其他安全性信息/机密信息，例如证书、 加密密钥、 主密钥和 Azure 服务中存储的存储密钥。  | 元数据不能包含控制导出数据。 此元数据包括创建和维护您的 Azure 虚拟计算机时输入的所有配置数据。  不要在以下字段输入受管制/控制数据︰ 租户角色名称、 资源组、 部署名称、 资源名称、 资源标记  

## <a name="next-steps"></a>下一步行动

有关的补充信息和更新，订阅<a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure 的政府博客。</a>
