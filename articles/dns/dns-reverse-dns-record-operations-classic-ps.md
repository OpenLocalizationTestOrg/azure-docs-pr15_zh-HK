<properties
   pageTitle="管理 Azure （经典） 服务使用 PowerShell 的反向 DNS 记录 |Microsoft Azure"
   description="如何管理反向 DNS 记录或在典型部署模型中使用 PowerShell 的 Azure 服务的 PTR 记录。 "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>如何管理您使用 Azure PowerShell 的 Azure 服务 （经典） 的反向 DNS 记录

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]了解如何[执行这些步骤，使用资源管理器模型](dns-reverse-dns-record-operations-ps.md)。

## <a name="validation-of-reverse-dns-records"></a>验证的反向 DNS 记录
为确保第三方无法创建反向映射到您的 DNS 域的 DNS 记录，Azure 只允许创建反向 DNS 记录以下任一项为真︰

- 反向 DNS FQDN 时为其指定了，云服务的名称或任何云服务名称，在相同的订阅中例如，反向 DNS"contosoapp1.cloudapp.net"。。
- 反向 DNS FQDN 向前解析为名称或 IP 的云服务，它已指定，或为任何云服务的名称或 IP 在相同的订阅中例如，反向 DNS 是"app1.contoso.com"。 这是 contosoapp1.cloudapp.net 的 CName 别名。

当云服务的反向 DNS 属性设置或修改，只会执行验证检查。 不执行定期重新验证。

## <a name="add-reverse-dns-to-existing-cloud-services"></a>向现有的云服务中添加反向 DNS
您可以添加到现有的云服务使用"集 AzureService"cmdlet 的反向 DNS 记录︰

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>使用反向 DNS 创建云服务
您可以指定使用"集 AzureService"cmdlet 的反向 DNS 属性添加新的云服务︰

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>查看现有的云服务的反向 DNS
您可以查看现有的云服务使用"Get AzureService"cmdlet 所配置的值︰

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>删除现有的云服务的反向 DNS
您可以删除现有的云服务使用"集 AzureService"cmdlet 的反向 DNS 属性。 这是通过将反向 DNS 属性值设置为空︰

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
