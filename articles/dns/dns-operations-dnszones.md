<properties
   pageTitle="管理 DNS 区域使用 PowerShell |Microsoft Azure"
   description="您可以管理使用 Azure Powershell 的 DNS 区域。 如何更新、 删除和创建在 Azure DNS 的 DNS 区域"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="how-to-manage-dns-zones-using-powershell"></a>如何管理使用 PowerShell 的 DNS 区域

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



本文将演示如何通过使用 PowerShell 管理您的 DNS 区域。 为了使用这些步骤，您将需要安装最新版本的 Azure 资源管理器 PowerShell cmdlet （1.0 或更高版本）。 有关安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。


## <a name="create-a-new-dns-zone"></a>创建新的 DNS 区域

若要创建 DNS 区域，请参阅[创建 DNS 区域使用 PowerShell](dns-getstarted-create-dnszone.md)。

## <a name="get-a-dns-zone"></a>得到一个 DNS 区域

若要检索的 DNS 区域，请使用`Get-AzureRmDnsZone`cmdlet。 此操作将返回到 Azure DNS 中现有的区域相对应的 DNS 区域对象。 对象包含有关 （例如，记录集的数量），该区域的数据，但不包含自身的记录集。

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>列表中的 DNS 区域

通过省略区域名称从`Get-AzureRmDnsZone`，您可以枚举的资源组中的所有区域。 此操作返回的区域对象的数组。

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>更新 DNS 区域

可通过使用 DNS 区域资源变为`Set-AzureRmDnsZone`。 这不会更新任何 DNS 区域中的记录集 （请参阅[如何管理 DNS 记录](dns-operations-recordsets.md)）。 它仅用于更新区域资源本身的属性。 这是当前限制到 Azure 资源管理器区域资源的标记。 有关详细信息，请参阅[Etag 和标记](dns-getstarted-create-dnszone.md#Etags-and-tags)。

使用下列方法之一更新 DNS 区域的两个方法︰

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>指定区域使用区域名称和资源组

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>指定使用 $zone 对象的区域

指定区域使用 $zone 对象从`Get-AzureRmDnsZone`。 当使用`Set-AzureRmDnsZone`与 $zone 对象，Etag 检查将用于确保并发更改不会被覆盖。 您可以使用可选的*-覆盖*开关来禁止这些检查。 有关详细信息，请参阅[Etag 和标记](dns-getstarted-create-dnszone.md#Etags-and-tags)。


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>删除 DNS 区域

可以使用删除 AzureRmDnsZone cmdlet 删除 DNS 区域。

在删除之前在 Azure DNS 的 DNS 区域，将需要删除所有记录集，在该区域的根目录创建区域时自动创建 NS 和 SOA 记录除外。

使用 DNS 区域中删除了以下两种方法之一︰

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>指定区域使用区域名称和资源组的名称

该操作有一个可选*-强制*开关它禁止提示您确认要删除 DNS 区域。

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>指定使用 $zone 对象的区域

指定区域使用 $zone 对象从`Get-AzureRmDnsZone`。 该操作有一个可选*-强制*开关它禁止提示您确认要删除 DNS 区域。 与`Set-AzureRmDnsZone`，指定使用 $zone 对象的区域启用 Etag 检查以确保并发更改不会被删除。 <BR>
可选的*-覆盖*标志禁止这些检查。 有关详细信息，请参阅[Etag 和标记](dns-getstarted-create-dnszone.md#Etags-and-tags)。

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



区域对象还可以输送而不是作为一个参数传递︰

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>下一步行动

创建 DNS 区域之后, 创建数据[记录集和记录](dns-getstarted-create-recordset.md)启动 Internet 域的名称解析。