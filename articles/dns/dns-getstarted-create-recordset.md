<properties
   pageTitle="创建记录集和记录的 DNS 区域使用 PowerShell |Microsoft Azure"
   description="如何创建用于 Azure DNS 主机记录。设置记录设置和记录使用 PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>通过使用 PowerShell 创建 DNS 记录集和记录


> [AZURE.SELECTOR]
- [Azure 门户](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

本文将引导您通过使用 Windows PowerShell 创建记录和记录集的过程。 创建 DNS 区域之后, 您添加为您的域的 DNS 记录。 为此，首先需要了解 DNS 记录和记录集。

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>请确保您有最新版本的 PowerShell

验证已安装最新版本的 Azure 资源管理器 PowerShell cmdlet。 有关安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。

## <a name="create-a-record-set-and-record"></a>创建一个记录集和记录

本部分介绍如何创建一个记录集和记录。


### <a name="1-connect-to-your-subscription"></a>1.连接到您的订阅

打开 PowerShell 控制台并连接到您的帐户。 使用下面的示例可帮助您连接︰

    Login-AzureRmAccount

请检查帐户订阅。

    Get-AzureRmSubscription

指定要使用的订阅。

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

有关使用 PowerShell 的详细信息，请参阅[使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)。


### <a name="2-create-a-record-set"></a>2.创建记录集

通过使用创建记录集`New-AzureRmDnsRecordSet`cmdlet。 在创建记录集时，您需要指定该记录集的名称、 该区域、 生存时间 ttl () 和记录类型。

若要创建记录集，在该区域的顶点 (在这种情况下，"contoso.com")，使用记录名称"@",包括引号。 这是一个常见的 DNS 约定。

下面的示例创建记录在 DNS 区域"contoso.com"中设置相对名称"www"。 记录集的完全限定名称是"www.contoso.com"。 记录类型为"A"，并将 TTL 为 60 秒。 在完成这一步之后, 您必须分配给变量*$rs*空"www"记录集。

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>如果记录集已存在

如果记录集已存在，该命令将失败，除非*-覆盖*开关使用。 *-覆盖*选项将触发确认提示，它可以通过抑制*-强制*开关。


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


在此示例中，您通过使用区域名称和资源组名称中指定的区域。 或者，可以指定一个区域的对象，返回的`Get-AzureRmDnsZone`或`New-AzureRmDnsZone`。

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`返回本地对象，它表示在 Azure DNS 中创建的记录集。

### <a name="3-add-a-record"></a>3.添加记录

若要使用新创建的"www"记录集，您需要向其中添加记录。 您可以添加 IPv4 使用下面的示例设置到"www"记录*的*记录。 此示例依赖于您在上一步中设置变量*$rs* 。

将记录添加到记录集使用`Add-AzureRmDnsRecordConfig`是一个离线操作。 仅本地变量*$rs*被更新。


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4.提交的更改

将更改提交到记录集。 使用`Set-AzureRmDnsRecordSet`的更改上载到 Azure DNS 设置的记录。

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5.检索记录集

您可以通过设置从 Azure DNS 记录`Get-AzureRmDnsRecordSet`在下面的示例所示。


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


您可以使用 nslookup 工具或其他 DNS 的工具来查询新的记录集。

如果您不委派到 Azure DNS 名称服务器的域，您需要显式指定名称、 服务器和您的区域的地址。


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>与单个记录创建每种类型的记录集


下面的示例演示如何创建的每个记录类型的记录集。 每个记录集只包含一个记录。

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>下一步行动

[如何管理使用 PowerShell 的 DNS 区域](dns-operations-dnszones.md)

[使用 PowerShell 管理 DNS 记录和记录集](dns-operations-recordsets.md)

[自动化与.NET SDK 的 Azure 操作](dns-sdk.md)
