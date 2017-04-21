<properties
   pageTitle="管理 DNS 记录集和记录在 Azure DNS 使用 Azure CLI |Microsoft Azure"
   description="当承载在 Azure DNS 域管理 DNS 记录集和 Azure DNS 记录。 在记录集和记录的操作的所有 CLI 命令。"
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>使用 CLI 管理 DNS 记录和记录集


> [AZURE.SELECTOR]
- [Azure 门户](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


本文演示如何通过使用跨平台 Azure 命令行界面 (CLI) 来管理记录集和记录您的 DNS 区域。

请务必了解 DNS 记录集和单个 DNS 记录之间的差异。 记录集是在一个区域中具有相同的名称和类型相同的记录集合。 有关详细信息，请参阅[了解记录集和记录](dns-getstarted-create-recordset-cli.md)。


## <a name="configure-the-cross-platform-azure-cli"></a>配置跨平台 Azure CLI

Azure DNS 是 Azure 只资源管理器服务。 它没有 Azure 服务管理 API。 请确保将 Azure CLI 配置为通过使用资源管理器模式`azure config mode arm`命令。

如果您看到**错误: dns 不是 azure 命令**，它很可能是因为您正在使用 Azure CLI 在 Azure 服务管理模式，在资源管理器模式。

## <a name="create-a-new-record-set-and-record"></a>创建一个新的记录集和记录

若要创建新的记录在 Azure 门户网站设置，请参阅[创建记录集和记录](dns-getstarted-create-recordset-cli.md)。


## <a name="retrieve-a-record-set"></a>检索记录集

若要检索现有的记录集，请使用`azure network dns record-set show`。 指定资源组，区域名称记录设置相对名称和记录类型。 使用以下示例中，替换为您自己的值。

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>列表中的记录集

您可以通过使用列出的 DNS 区域中的所有记录`azure network dns record-set list`命令。 您需要指定的资源组名称和区域名称。

### <a name="to-list-all-record-sets"></a>若要列出所有的记录集

此示例返回所有的记录集，而不考虑名称或记录类型︰

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>为给定类型的列表记录集

此示例返回所有匹配给定的记录类型 （在本例中为"A"记录） 的记录集︰

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>将记录添加到记录集

您添加到记录集使用记录`azure network dns record-set add-record`命令。 将记录添加到记录集的参数取决于所设置的记录的类型。 例如，当使用类型"A"记录集时，只能指定记录参数与`-a <IPv4 address>`。

若要创建的记录集，请使用`azure network dns record-set create`命令。 指定资源组，区域名称记录设置相对名称、 记录类型和时间来生存 (TTL)。 如果`--ttl`参数未定义、 四个默认的值 （以秒为单位）。

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


创建"A"记录集之后，通过添加 IPv4 地址`azure network dns record-set add-record`命令。

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


下面的示例演示如何创建的每个记录类型的记录集。 每个记录集只包含一个记录。

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>更新的记录的记录集

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>要将另一个 IP 地址 (1.2.3.4) 添加到一个现有的"A"记录集 ("www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>若要从记录集中删除的现有值
使用`azure network dns record-set delete-record`。

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>从记录集中删除记录

记录可从记录集使用`azure network dns record-set delete-record`。 要删除的记录的所有参数必须完全与现有记录匹配。

从记录集中删除的最后一个记录不会删除该记录集。 有关详细信息，请参阅文[删除某一记录集](#delete)一节。

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>从记录集中删除 AAAA 记录

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>从记录集中删除的 CNAME 记录

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>从记录集中删除的 MX 记录

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>从记录集中删除的 NS 记录

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>从记录集中删除 PTR 记录
在这种情况下我的 arpa-zone.com' 表示表示 IP 范围的 ARPA 区域。  在此区域中设置每个 PTR 记录与此 IP 地址范围内的 IP 地址相对应。

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>从记录集中删除的 SRV 记录

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>从记录集中删除 TXT 记录

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>删除某一记录集

可以通过删除记录集`Remove-AzureRmDnsRecordSet`cmdlet。 您不能删除 SOA 和 NS 记录设置区域 apex (名称 ="@")创建区域时自动创建。 他们将被自动删除如果区域将被删除。

在以下示例中，"A"记录集"测试 a"将从"contoso.com"DNS 区域︰

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

可选*-q*开关可用于取消确认提示。


## <a name="next-steps"></a>下一步行动

有关 Azure DNS 的详细信息，请参阅[Azure DNS 概述](dns-overview.md)。 有关自动 DNS 信息，请参阅[使用.NET SDK 创建 DNS 区域和记录集](dns-sdk.md)。

如果您想要使用反向 DNS 记录，请参阅[如何管理您的服务使用 Azure CLI 的反向 DNS 记录](dns-reverse-dns-record-operations-cli.md)。
