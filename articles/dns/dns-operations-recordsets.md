<properties
   pageTitle="通过使用 Azure 门户管理 DNS 记录集和记录 |Microsoft Azure"
   description="当承载在 Azure DNS 域管理 DNS 记录集和 Azure DNS 记录。 在记录集和记录的操作的所有 PowerShell 命令。"
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>使用 PowerShell 管理 DNS 记录和记录集



> [AZURE.SELECTOR]
- [Azure 门户](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



这篇文章演示了如何使用 Windows PowerShell 管理记录集和记录的 DNS 区域。

请务必了解 DNS 记录集和单个 DNS 记录之间的差异。 记录集是在一个区域中具有相同的名称和类型相同的记录的集合。 有关详细信息，请参阅[创建 DNS 记录集和记录使用 Azure 的门户](dns-getstarted-create-recordset-portal.md)。

要管理您的记录集和记录，您需要最新版本的 Azure 资源管理器 PowerShell cmdlet。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。 有关使用 PowerShell 的详细信息，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。



## <a name="create-a-new-record-set-and-a-record"></a>创建新的记录集和记录

若要创建使用 PowerShell 设置的记录，请参阅[创建 DNS 记录集和记录通过使用 PowerShell](dns-getstarted-create-recordset.md)。

## <a name="get-a-record-set"></a>获取记录集

若要检索现有的记录集，请使用`Get-AzureRmDnsRecordSet`。 指定该记录集相对名称、 记录类型，以及该区域。

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

与`New-AzureRmDnsRecordSet`，记录名称必须为相对名称，意味着它必须排除的区域名称。

通过使用区域名称和资源组的名称，或使用区域对象，您可以指定该区域︰

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`返回本地对象，它表示在 Azure DNS 中创建的记录集。

## <a name="list-record-sets"></a>列表中的记录集

您还可以使用`Get-AzureRmDnsRecordSet`如果忽略列表记录集为*– 名称*和/或*-RecordType*参数。

### <a name="to-list-all-record-sets"></a>若要列出所有的记录集

此示例返回所有的记录集，而不考虑名称或记录类型︰

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>对给定的记录类型的列表记录集

本示例返回所有匹配给定的记录类型的记录集。 在这种情况下，记录集的返回是"A"记录︰

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

通过使用任一*ZoneName-*和*-ResourceGroupName*参数 （如所示），或通过指定区域对象，可以指定该区域︰

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>将记录添加到记录集

您添加到记录集使用记录`Add-AzureRmDnsRecordConfig`cmdlet。 这是一个离线操作。 仅本地对象，它表示记录集将被更改。

将记录添加到记录集的参数的记录集的类型而异。 例如，当使用类型"A"记录集时，只能指定记录参数与*-IPv4Address*。

其他记录可以添加到其他调用来设置每个记录`Add-AzureRmDnsRecordConfig`。 对任何记录集，您可以添加最多 20 个记录。 但是，类型为"CNAME"的记录集可以包含至多一个记录，并且记录集不能包含两个相同的记录。 （用零记录） 的空记录集可以创建，但并不出现在 Azure DNS 名称服务器。

记录集包含所需的记录的集合后，您需要将其提交通过`Set-AzureRmDnsRecordSet`cmdlet。 记录集已被提交之后，它将替换设置 Azure DNS 中的现有记录。

### <a name="to-create-an-a-record-set-with-a-single-record"></a>若要创建与单个记录集的记录

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

创建记录的操作的顺序也可以*输送*，意味着您用的管道，而不是将其作为一个参数传递通过传递的记录集对象。 例如︰

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>其他记录类型的示例

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>修改现有的记录集

修改现有的记录集的步骤是类似于创建记录时所采取的步骤。 操作的顺序如下所示︰

1.  检索现有记录集，通过使用`Get-AzureRmDnsRecordSet`。

2.  修改记录集，或者添加记录、 删除记录、 更改记录参数，或更改该记录将时间设置为实时 (TTL)。 这是一个离线操作。 仅本地对象，它表示记录集将被更改。

3.  通过提交更改`Set-AzureRmDnsRecordSet`cmdlet。 这将替换设置 Azure DNS 中的现有记录。


### <a name="to-update-a-record-in-an-existing-record-set"></a>若要更新现有记录集中的记录

在此示例中，我们将更改现有"A"记录的 IP 地址︰

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

`Set-AzureRmDnsRecordSet` Cmdlet 使用 etag 检查以确保并发更改不会被覆盖。 使用*-覆盖*标志来禁止显示这些检查。 有关详细信息，请参阅[关于 etag 和标记](dns-getstarted-create-dnszone.md#tagetag)。

### <a name="to-modify-an-soa-record"></a>若要修改 SOA 记录

不能添加或删除在区域 apex 设置自动创建 SOA 记录的记录 (名称 = "@")。 但是，您可以修改任何 SOA 记录 （除"主机"） 中的参数，并且记录设置 TTL。

下面的示例演示如何将 SOA 记录的*电子邮件*属性更改︰

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>若要修改的区域顶点处的 NS 记录

不能添加、 删除或修改的记录自动创建 NS 记录集中的区域顶点处 (名称 = "@")。 允许的唯一更改是修改记录集 TTL。

下面的示例演示如何将 NS 记录集的 TTL 属性更改︰

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>若要将记录添加到一个现有的记录集

在此示例中，我们对现有的记录集添加两个额外的 MX 记录︰

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>从现有的记录集中删除记录

记录可从记录集使用`Remove-AzureRmDnsRecordConfig`。 要删除的记录的所有参数必须完全与现有记录匹配。 更改都必须提交使用`Set-AzureRmDnsRecordSet`。

从记录集中删除的最后一个记录不会删除该记录集。 详细信息，请参阅下面的[删除某一记录集](#delete-a-record-set)。


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

若要从记录集中删除记录的操作的顺序也被输送，意味着您用的管道，而不是将其作为一个参数传递通过传递的记录集对象。 例如︰

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>从记录集中删除 AAAA 记录

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>从记录集中删除的 CNAME 记录

由于 CNAME 记录集可能包含最大的一条记录，删除该记录离开空记录集。

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>从记录集中删除的 MX 记录

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>从记录集中删除的 NS 记录

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>从记录集中删除的 SRV 记录

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>从记录集中删除 TXT 记录

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>删除某一记录集

可以通过删除记录集`Remove-AzureRmDnsRecordSet`cmdlet。 您不能删除 SOA 和 NS 记录设置区域 apex (名称 ="@")创建区域时自动创建。 他们将被自动删除如果区域将被删除。

使用以下三种方法之一来删除记录集︰

### <a name="specify-all-the-parameters-by-name"></a>按名称指定所有参数

可选的*-强制*开关可用于取消确认提示。

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>指定记录集的名称和类型，并通过对象指定区域

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>指定记录集对象

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

指定记录集，使用对象时，它会使 etag 检查以确保并发更改不会被删除。 可选的*-覆盖*标志禁止这些检查。 有关详细信息，请参阅[Etag 和标记](dns-getstarted-create-dnszone.md#tagetag)。

记录集对象还可以输送而不是作为一个参数传递︰

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>下一步行动

有关 Azure DNS 的详细信息，请参阅[Azure DNS 概述](dns-overview.md)。 有关自动 DNS 信息，请参阅[使用.NET SDK 创建 DNS 区域和记录集](dns-sdk.md)。

有关反向 DNS 记录的详细信息，请参阅[如何管理您的服务使用 PowerShell 的反向 DNS 记录](dns-reverse-dns-record-operations-ps.md)。
