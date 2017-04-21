<properties
   pageTitle="开始使用 Azure DNS |Microsoft Azure"
   description="了解如何为 Azure DNS 创建 DNS 区域。这是循序渐进以获取第一个 DNS 区域创建启动承载您使用 PowerShell 的 DNS 域。"
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

# <a name="create-a-dns-zone-using-powershell"></a>创建使用 Powershell 的 DNS 区域

> [AZURE.SELECTOR]
- [Azure 门户](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

这篇文章将引导您完成这些步骤，使用 PowerShell 创建 DNS 区域。 您还可以创建 DNS 区域使用 CLI 或 Azure 的门户。

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>关于 Etag 和标记

### <a name="etags"></a>Etag

假设两个人或两个进程试图同时修改 DNS 记录。 哪一次为准？ 但获胜者知道它们已经不仅仅是覆盖由其他人创建的更改？

Azure 的 DNS 使用 Etag 来安全地处理对同一资源的并发更改。 （区域或记录集） 的每个 DNS 资源具有与之关联的 Etag。 每当检索资源时，还会检索其 Etag。 当更新资源，您可以选择要传递回 Etag 因此 Azure DNS 可验证的 Etag 的服务器匹配。 由于对资源的每次更新会导致重新生成 Etag，Etag 匹配指示发生了并发更改。 Etag 还使用时创建新的资源，确保资源不已经存在。

默认情况下，Azure DNS PowerShell 使用 Etag 可以阻止并发更改区域和记录集。 可选的*-覆盖*开关可用于取消 Etag 检查，这种情况下所发生的任何并发更改将被覆盖。

在 Azure DNS REST API 级别，使用 HTTP 标头指定 Etag。  下表给出了它们的行为︰

|标头|行为|
|------|--------|
|无|将始终成功 （没有 Etag 检查）|
|如果匹配<etag>|PUT 只会成功如果资源存在并 Etag 匹配|
|如果匹配 *     | 如果资源存在 PUT 只会成功|
|如果没有-匹配 * |  如果资源不存在 PUT 只会成功|

### <a name="tags"></a>标记

标记是不同的 Etag。 标记的名称-值对列表，并且使用 Azure 资源管理器标签的资源进行计费或分组。 有关标记的详细信息，请参阅[使用标签来组织您的 Azure 资源](../resource-group-using-tags.md)。

Azure DNS PowerShell 支持区域和使用选项指定的记录集上的标记`-Tag`参数。


## <a name="before-you-begin"></a>在开始之前

请确保在开始配置之前有下列各项。

- Azure 的订阅。 如果没有 Azure 的订阅，您可以激活您的[MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或符号向上[免费的帐户](https://azure.microsoft.com/pricing/free-trial/)。

- 您将需要安装最新版本的 Azure 资源管理器 PowerShell cmdlet （1.0 或更高版本）。 有关安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。

## <a name="step-1---sign-in"></a>步骤 1-登录

打开 PowerShell 控制台并连接到您的帐户。 有关详细信息，请参阅[使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)。

使用下面的示例可帮助您连接︰

    Login-AzureRmAccount

请检查帐户订阅。

    Get-AzureRmSubscription

指定要使用的订阅。

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>步骤 2-创建的资源组

Azure 的资源管理器需要的所有资源组都指定一个位置。 这是该资源组中的资源用作默认位置。 但是，由于所有 DNS 资源全局、 没有区域，资源组位置的选择对 Azure DNS 没有任何影响。

如果您正在使用现有资源组，您可以跳过此步骤。

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>步骤 3-寄存器

由 Microsoft.Network 资源提供程序管理 Azure DNS 服务。 Azure 订阅需要注册才能使用 Azure DNS 使用此资源提供程序。 这是为每个订阅的一次性操作。

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>第 4 步-创建 DNS 区域

通过创建 DNS 区域`New-AzureRmDnsZone`cmdlet。 有以下示例以带有或不带标记创建 DNS 区域。 有关标记的详细信息，请参阅本文中的[标记](#tags)上的部分。

>[AZURE.NOTE] 在 Azure DNS 区域名称应指定不终止情况下**。**。 例如，作为 '**contoso.com**' 而不是**contoso.com。**。

### <a name="to-create-a-dns-zone"></a>若要创建 DNS 区域

下面的示例创建名为*MyResourceGroup*的资源组中称为*contoso.com* DNS 区域。 使用示例创建 DNS 区域，替换为您自己的值。

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>使用标记创建 DNS 区域

下面的示例演示如何使用两个标记，创建 DNS 区域*项目 = 演示*和*env = 测试*。 使用示例创建 DNS 区域，替换为您自己的值。

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>查看记录

创建 DNS 区域还将创建以下 DNS 记录︰

- *起始授权机构*(SOA) 记录。 这是存在于每个 DNS 区域根目录。

- 授权的名称服务器 (NS) 记录。 这些显示哪些名称服务器承载的区域。 Azure 的 DNS 使用的名称服务器池，因此不同名称服务器可能会分配到 Azure DNS 中的不同区域。 [委派到 Azure DNS 域](dns-domain-delegation.md)的详细信息，请参阅。

若要查看这些记录，请使用`Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


记录集的根 （或*apex*） 的 DNS 区域使用**@**为该记录集的名称。


## <a name="test"></a>测试

可以通过使用 DNS 工具例如 nslookup，挖或为[解决 DnsName PowerShell cmdlet](https://technet.microsoft.com/library/jj590781.aspx)来测试您的 DNS 区域。

如果尚没有委托您使用新在 Azure DNS 区域的域，您需要直接直接到您的区域的名称服务器的 DNS 查询。 您的区域的名称服务器中给出了 NS 记录中，通过列出`Get-AzureRmDnsRecordSet`以上。 请确保替换您的区域分成以下命令的正确值。

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>下一步行动

创建 DNS 区域之后, 创建数据[记录集和记录](dns-getstarted-create-recordset.md)启动 Internet 域的名称解析。

