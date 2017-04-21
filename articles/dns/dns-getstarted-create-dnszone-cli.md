<properties
   pageTitle="创建使用 CLI 的 DNS 区域 |Microsoft Azure"
   description="了解如何创建 Azure DNS 分步启动承载您使用 CLI 的 DNS 域的 DNS 区域"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>创建使用 CLI Azure DNS 区域


> [AZURE.SELECTOR]
- [Azure 门户](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


这篇文章将引导您完成创建 DNS 区域通过使用 CLI 的步骤。 您还可以创建 DNS 区域使用 PowerShell 或 Azure 的门户。

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>在开始之前

这些说明使用 Microsoft Azure CLI。 请务必更新到最新的 Azure CLI (0.9.8 或更高版本) 使用 Azure DNS 命令。 类型`azure -v`来检查计算机中当前安装的 Azure CLI 版本。

## <a name="step-1---set-up-azure-cli"></a>步骤 1-设置 Azure CLI

### <a name="1-install-azure-cli"></a>1.安装 Azure CLI

您可以安装的 Windows Azure CLI、 Linux、 或 mac。 以下步骤需要您可以管理 Azure DNS 使用 Azure CLI 之前完成。 在[安装 Azure CLI](../xplat-cli-install.md)提供了详细信息。 这些 DNS 命令要求 Azure CLI 版本 0.9.8 或更高版本。

Cli 的所有网络提供程序命令都可以找到使用下面的命令︰

    azure network

### <a name="2-switch-cli-mode"></a>2.切换 CLI 模式

Azure 的 DNS 使用 Azure 资源管理器。 请确保您切换 CLI 使用 ARM 命令模式。

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3.注册到 Azure 帐户

系统将提示您要使用您的凭据进行身份验证。 请记住，您只能使用 ORGID 帐户。

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4.选择订阅

选择您要使用的 Azure 订阅。

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5.创建的资源组

Azure 的资源管理器需要的所有资源组都指定一个位置。 这是该资源组中的资源用作默认位置。 但是，由于所有 DNS 资源全局、 没有区域，资源组位置的选择对 Azure DNS 没有任何影响。

如果您正在使用现有资源组，您可以跳过此步骤。

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6.寄存器

由 Microsoft.Network 资源提供程序管理 Azure DNS 服务。 Azure 订阅需要注册才能使用 Azure DNS 使用此资源提供程序。 这是为每个订阅的一次性操作。

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>步骤 2-创建 DNS 区域

使用创建 DNS 区域`azure network dns zone create`命令。 您可以选择创建 DNS 区域以及标记。 标记的名称-值对列表，并且使用 Azure 资源管理器标签的资源进行计费或分组。 有关标记的详细信息，请参阅[使用标签来组织您的 Azure 资源](../resource-group-using-tags.md)。

在 Azure DNS 区域名称应指定不终止情况下**。**。 例如，作为 '**contoso.com**' 而不是**contoso.com。**。


### <a name="to-create-a-dns-zone"></a>若要创建 DNS 区域

下面的示例创建名为*MyResourceGroup*的资源组中称为*contoso.com* DNS 区域。

使用该示例来创建您的 DNS 区域，替换为您自己的值。

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>若要创建 DNS 区域和标记。

Azure DNS CLI 支持使用可选的指定的 DNS 区域的标记*-标记*参数。 下面的示例演示如何使用两个标记创建 DNS 区域、 项目演示和 env = = 测试。

使用下面的示例创建一个 DNS 区域和标记，替换为您自己的值。

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>查看记录

创建 DNS 区域还将创建以下 DNS 记录︰

- 起始授权机构 (SOA) 记录。 这是存在于每个 DNS 区域根目录。

- 授权的名称服务器 (NS) 记录。 这些显示哪些名称服务器承载的区域。 Azure 的 DNS 使用的名称服务器池，因此不同名称服务器可以分配到 Azure DNS 中的不同区域。 有关更多信息，请参见[委托到 Azure DNS 域](dns-domain-delegation.md)。

若要查看这些记录，请使用`azure network dns-record-set show`。<BR>
*用法︰ 网络 dns 记录集显示 < 资源组 >< dns 区域名称 > <name><type>*


在下面的示例中，如果使用资源组*myresourcegroup*，运行命令记录集名称*"@"*（对于根记录），键入*SOA*，它将产生以下输出︰


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
要查看创建的区域的 NS 记录，请使用下面的命令︰

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] 记录集的根 （或*apex*） 的 DNS 区域使用**@**为该记录集的名称。

## <a name="test"></a>测试

您可以使用 nslookup，挖，如 DNS 工具测试您的 DNS 区域或`Resolve-DnsName`PowerShell cmdlet。

如果还没有委托您使用新在 Azure DNS 区域的域，您需要将直接与您的区域的名称服务器的 DNS 查询。 您的区域的名称服务器给出与"azure 网络 dns 记录集展览"由上述的 NS 记录。 请确保替换您在以下命令中的区域的正确值。

下面的示例使用挖来查询使用的 DNS 区域分配的名称服务器域 contoso.com。 查询包含指向我们使用的名称服务器*@*并使用挖的区域名称。

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>下一步行动

创建 DNS 区域之后, 创建数据[记录集和记录](dns-getstarted-create-recordset-cli.md)启动 Internet 域的名称解析。
