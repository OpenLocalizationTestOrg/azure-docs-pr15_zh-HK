<properties
   pageTitle="导入和导出到使用 CLI 的 Azure DNS 域区域文件 |Microsoft Azure"
   description="了解如何导入和导出到 Azure DNS 的 DNS 区域文件，通过使用 Azure CLI"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>导入和导出使用 Azure CLI 的 DNS 区域文件


本文将指导您完成如何导入和导出使用 Azure CLI 的 Azure dns 的 DNS 区域文件。

## <a name="introduction-to-dns-zone-migration"></a>DNS 区域迁移简介

DNS 区域文件是一个文本文件，其中包含区域中每个域名系统 (DNS) 记录的详细信息。 它遵循标准的格式，使它适合用于 DNS 系统之间传输 DNS 记录。 使用区域文件是快速、 可靠、 方便地进出 Azure DNS 传输 DNS 区域。

Azure DNS 支持导入和导出使用 Azure 的命令行界面 (CLI) 的区域文件。 Azure CLI 可以用于管理 Azure 服务的跨平台命令行工具。 它是为 Windows、 Mac 和 Linux 平台[Azure 的下载页面](https://azure.microsoft.com/downloads/)中可用。 跨平台支持是导入和导出区域文件尤其重要，因为最常见的名称服务器软件，[绑定](https://www.isc.org/downloads/bind/)，通常运行于 Linux 上。

## <a name="obtain-your-existing-dns-zone-file"></a>获取现有 DNS 区域文件

将 DNS 区域文件导入到 Azure DNS 之前，您需要获取区域文件的副本。 此文件的来源将取决于当前承载 DNS 区域。

- 如果您的 DNS 区域由伙伴服务 （如域注册、 专用的 DNS 承载提供程序或其他云提供商），该服务应提供下载 DNS 区域文件的能力。

- 如果在 Windows DNS 承载 DNS 区域，区域文件的默认文件夹是**%systemroot%\system32\dns**。 DNS 服务管理控制台的**常规**选项卡还显示每个区域文件的完整路径。

- 如果使用绑定由承载 DNS 区域，每个区域的区域文件的位置指定绑定配置文件**named.conf**中。

**使用从 GoDaddy 的区域文件**<BR>
从 GoDaddy 的区域文件具有稍有非标准的格式。 您需要更正此问题之前将这些区域文件导入到 Azure DNS。 在 RData 的每个 DNS 记录的 DNS 名称被指定为完全限定的名称，但他们没有终止"。"这意味着其他 DNS 系统作为相对名称解释它们。 您需要编辑区域文件追加终止"。"为它们之前将它们导入 Azure DNS 的名称。

## <a name="import-a-dns-zone-file-into-azure-dns"></a>将 DNS 区域文件导入到 Azure DNS


导入区域文件如果将创建一个新的区域在 Azure DNS 中尚不存在。 如果区域已经存在，则必须与现有记录集合并区域文件中的记录集。

### <a name="merge-behavior"></a>合并行为

- 默认情况下，合并现有的和新的记录集。 相同的记录，合并记录集内的重复数据消除。

- 或者，通过指定`--force`选项，导入过程将替换现有与新的记录集的记录集。 没有相应的记录设置的导入的区域文件中的现有记录集将不会被删除。

- 当合并记录集时，将使用生存时间 ttl () 的预先存在的记录集。 当`--force`被使用，使用 TTL 设置为新的记录集。

- 起始授权机构 (SOA) 的参数 (除`host`) 总是来自导入的区域文件，而不管是否`--force`使用。 同样，在区域 apex 设置名称服务器记录，TTL 总是来自导入的区域文件。

- 导入的 CNAME 记录不会替换现有的 CNAME 记录使用相同的名称除非`--force`指定参数。

- 当 CNAME 记录与另一个记录的相同名称但不同类型 （不论现有的或新建） 之间发生冲突时，现有记录被保留下来。 这是独立于使用`--force`。

### <a name="additional-information-about-importing"></a>有关导入的其他信息

下面的说明提供了有关该区域的更多技术详细信息导入过程。

- `$TTL`指令是可选的而且支持。 如果没有`$TTL`给出指令，没有明确的 TTL 记录将导入的设置为默认的 TTL 设置为 3600 秒。 当在同一记录集中的两个记录指定不同的 Ttl 时，请使用较小的值。

- `$ORIGIN`指令是可选的而且支持。 如果没有`$ORIGIN`设置，使用的默认值是在命令行上指定的区域名称 (加上终止"。")。

- `$INCLUDE` ，`$GENERATE`指令不受支持。

- 支持这些记录类型︰ A、 AAAA、 CNAME、 MX，NS、 SOA、 SRV 和 TXT。

- SOA 记录 Azure dns 区域创建时自动创建。 导入区域文件时，所有的 SOA 参数取自区域文件*除*`host`参数。 此参数将使用 Azure DNS 提供的值。 这是因为此参数必须引用提供的 Azure DNS 的主名称服务器。

- 在区域 apex 设置名称服务器记录还时自动创建通过 Azure DNS 区域创建。 仅此记录集的 TTL 是导入。 这些记录包含提供的 Azure DNS 名称服务器名。 导入的区域文件中包含的值不会覆盖记录数据。

- 在公共预览时，Azure DNS 支持只有一个字符串 TXT 记录。 将连接名 TXT 记录，并且将其截断为 255 个字符。

### <a name="cli-format-and-values"></a>CLI 格式和值


要导入一个 DNS 区域的 Azure CLI 命令格式是︰<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

值︰

- `<resource group>`是在 Azure DNS 区域的资源组的名称。
- `<zone name>`是该区域的名称。
- `<zone file name>`为要导入的区域文件的路径名。

如果资源组中不存在具有此名称的区域，将为您创建它。 如果该区域已经存在，导入的记录集将会与现有记录集进行合并。 若要覆盖现有的记录集，请使用`--force`选项。

若要验证而不实际导入的区域文件格式，请使用`--parse-only`选项。

### <a name="step-1-import-a-zone-file"></a>第 1 步。 导入区域文件

若要导入的区域**contoso.com**区域文件。

1. 通过使用 Azure CLI 中登录到 Azure 订购。

        azure login

2. 选择要用来创建新的 DNS 区域的订阅。

        azure account set <subscription name>

3. Azure DNS 是 Azure 的仅限资源管理器服务，因此 Azure CLI 必须切换到资源管理器模式。

        azure config mode arm

4. 使用 Azure DNS 服务之前，您必须注册您的订购以使用 Microsoft.Network 资源提供程序。 （这是为每个订阅的一次性操作）。

        azure provider register Microsoft.Network

5. 如果您还没有，您还需要创建资源管理器的资源组。

        azure group create myresourcegroup westeurope

6. 为文件**contoso.com.txt**区域**contoso.com**导入资源组**myresourcegroup**中新的 DNS 区域中，运行命令`azure network dns zone import`。<BR>此命令将加载的区域文件，并分析它。 该命令将在 Azure DNS 服务创建的区域上执行一系列命令和在该区域中所有记录的设置。 该命令还将报告在控制台窗口中，以及任何错误或警告的进度。 由于系列中创建记录集时，它可能需要几分钟的较大区域文件导入。

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>第 2 步。 请验证该区域

若要导入的文件后，请验证 DNS 区域，可以使用以下方法之一︰

- 您可以通过使用以下 Azure CLI 命令列出记录。

        azure network dns record-set list myresourcegroup contoso.com

- 您可以通过使用 PowerShell cmdlet 列出记录`Get-AzureRmDnsRecordSet`。

- 您可以使用`nslookup`若要验证名称解析的记录。 因为该区域不尚未委派，您需要显式指定正确的 Azure DNS 名称服务器。 下面的示例演示如何检索分配给该区域的名称服务器名称。 IT 还演示如何使用查询"www"记录`nslookup`。

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>第 3 步。 更新 DNS 委派

验证已正确导区域后，您需要更新指向 Azure DNS 名称服务器的 DNS 委派。 有关详细信息，请参阅文章[更新 DNS 委派](dns-domain-delegation.md)。

## <a name="export-a-dns-zone-file-from-azure-dns"></a>从 Azure DNS 的 DNS 区域文件

要导入一个 DNS 区域的 Azure CLI 命令格式是︰<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

值︰

- `<resource group>`是在 Azure DNS 区域的资源组的名称。
- `<zone name>`是该区域的名称。
- `<zone file name>`为要导出的区域文件的路径名。

作为与区域导入，您首先需要进行登录，选择您的订购和配置 Azure CLI 使用资源管理器模式。

### <a name="to-export-a-zone-file"></a>若要导出区域文件


1. 通过使用 Azure CLI 中登录到 Azure 订购。

        azure login

2. 选择要用来创建新的 DNS 区域的订阅。

        azure account set <subscription name>

3. Azure DNS 是 Azure 只资源管理器服务。 Azure CLI 必须切换到资源管理器模式中。

        azure config mode arm

4. 要将资源组**myresourcegroup**在现有 Azure DNS 区域**contoso.com**导出到文件**contoso.com.txt** （位于当前文件夹中），请运行`azure network dns zone export`。 此命令将调用 Azure DNS 服务枚举区域中的记录集，并将结果导出到一个绑定兼容的区域文件。

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

