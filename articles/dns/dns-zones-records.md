<properties 
   pageTitle="DNS 区域和记录 |Microsoft Azure" 
   description="支持用于承载 DNS 区域和 Microsoft Azure DNS 中的记录的概览。" 
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
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS 区域和记录

此页解释域，DNS 区域和 DNS 记录和记录集，以及如何在 Azure DNS 支持的主要概念。

## <a name="domain-names"></a>域名称
 
域名系统是域的层次结构。 在层次结构开始从根域中，其名称就是**.**。  在此之下是顶级域，例如 com，净、 组织、 英国或 jp。  下面这些是第二层域，例如 org.uk 或 co.jp'。 全球分布域 DNS 层次结构中的，由世界各地的 DNS 名称服务器。

域的域名注册是一个组织，您可以购买一个域名，如 'contoso.com'。  购买一个域名可以让您控制 DNS 层次结构下该名称，例如允许您将您公司的网站名称 www.contoso.com 的权利。 注册器可以承载在自己替您的名称服务器的域或者也可以指定其他名称服务器。

Azure DNS 提供遍布全球的、 高可用性的名称服务器基础架构，您可以使用它来承载您的域。 通过设立在 Azure DNS 域，可以作为其他 Azure 服务来管理您的 DNS 记录与相同的凭据、 Api、 工具、 计费和支持。

Azure DNS 当前不支持购买的域名。 如果您想要购买一个域名，您需要使用第三方域的域名注册。 注册器通常会收取少量的年度费用。 域然后可以在 Azure DNS 中用于管理 DNS 记录的承载。 有关详细信息，请参阅[委托到 Azure DNS 域](dns-domain-delegation.md)。

## <a name="dns-zones"></a>DNS 区域 

DNS 区域用于承载特定域的 DNS 记录。 若要启动承载在 Azure DNS 域，您需要创建该域名的 DNS 区域。 在该 DNS 区域内再创建为您的域的每个 DNS 记录。 

例如，域 'contoso.com' 可能包含多个 DNS 记录，如 mail.contoso.com （用于邮件服务器） 和 www.contoso.com （对于 web 站点）。

当在 Azure DNS 创建 DNS 区域，该区域的名称必须是唯一资源组内。 在不同的资源组或其他 Azure 订阅可以重用相同的区域名称。 在多个区域共享相同的名称，每个实例分配不同的名称服务器的地址。 只有一组的地址可以配置的域的名称注册。

>[AZURE.NOTE] 不需要有一个域名在 Azure DNS 域同名创建 DNS 区域。 但是，您需要拥有域将 Azure DNS 名称服务器配置为正确的名称服务器上的域的名称注册的域名。

有关详细信息，请参阅[委派到 Azure DNS 域](dns-domain-delegation.md)。

## <a name="dns-records"></a>DNS 记录

### <a name="record-types"></a>记录类型

每个 DNS 记录都有一个名称和一个类型。 记录被组织成各种根据它们所包含的数据类型。 最常见的类型是 A 记录，它将名称映射到 IPv4 地址。 另一个常见的类型是 MX 的记录，它将名称映射到的邮件服务器。

Azure DNS 支持所有常见的 DNS 记录类型︰ A、 AAAA、 CNAME、 MX，NS、 PTR、 SOA、 SRV 和 TXT。

### <a name="record-names"></a>记录名称

在 Azure DNS 记录指定使用的相对名称。 *完全限定的*域名 (FQDN) 包括区域名称，而*相对*的名称则不。 例如，相对的记录名称区域 'contoso.com' www 提供完全限定的记录名称 www.contoso.com。

*Apex*记录是在根 （或*apex*） 的 DNS 记录的 DNS 区域。 例如，在 DNS 区域 'contoso.com' 中，apex 记录还有 'contoso.com' （这有时称为*裸*域） 的完全限定的名。  按照约定，相对名称'@'用于创建 apex 记录。

### <a name="record-sets"></a>记录集
有时，您需要创建具有给定名称和类型的多个 DNS 记录。 例如，假设 www.contoso.com 网站位于两个不同的 IP 地址。 该网站要求两个不同的记录，另一个用于每个 IP 地址。 这是一种记录集︰

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS 管理 DNS 记录使用*记录集*。 记录集 （也称为*资源*记录集） 是区域中 DNS 记录具有相同的名称，它们是同一类型的集合。 大多数的记录集包含同一个记录，但示例此类似，在其中的记录集包含多个记录，并非少见。

例如，假设您已经 'contoso.com' 的区域创建 A 记录 www，指向该 IP 地址"134.170.185.46"（上面的第一个记录）。  若要创建第二个记录您会将该记录添加到现有的记录集，而不是创建新的记录集。

SOA 和 CNAME 记录类型是例外。 DNS 标准不允许对这些类型同名的多个记录，因此这些记录集只能包含一个记录。

### <a name="time-to-live"></a>生存时间

生存，时间或 TTL 指定多长时间的每条记录被缓存客户端之前对其进行重新查询。 在上面的示例中，TTL 是 3600 秒或 1 小时。

在 Azure DNS TTL 指定记录集，不是为每个记录，以便为该记录集内的所有记录中使用相同的值。  您可以指定任何 TTL 值介于 1 和 2147483647 秒之间。

### <a name="wildcard-records"></a>通配符的记录

Azure DNS 支持[通配符的记录](https://en.wikipedia.org/wiki/Wildcard_DNS_record)。 通配符的记录返回到具有匹配名称的所有查询的响应中 （除非从非通配符记录集更匹配）。 除 NS 和 SOA 的所有记录类型支持通配符的记录集。  

若要创建通配符的记录集，请使用记录集名称\*。 另外，也可以使用的名称与\*作为其最左边的标签，例如，\*.foo'。

### <a name="cname-records"></a>CNAME 记录

CNAME 记录集不能共存与其他具有相同名称的记录集。 例如，不能创建 CNAME 记录使用 www 的相对名称设置和 A 记录的相对名称 www 在同一时间。

因为区域 apex (名称 =‘@’)始终包含 NS 和 SOA 记录集创建创建该区域时，您不能创建在区域 apex 设置 CNAME 记录。

这些约束源自 DNS 标准并不是 Azure DNS 限制。

### <a name="ns-records"></a>NS 记录

在每个区域的 apex 自动创建 NS 记录集 (名称 =‘@’),和删除该区域时将自动删除 （但不能删除分开）。  您可以修改 TTL 设置为此记录集，但不是能修改记录，进行了预配置的引用分配给该区域的 Azure DNS 名称服务器。

您可以创建和删除该区域，不能在区域 apex 内的其他 NS 记录。  这使您可以配置的子区域 （请参阅[在 Azure DNS 委派子域](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)）。

### <a name="soa-records"></a>SOA 记录

在每个区域的 apex 自动创建 SOA 记录集 (名称 =‘@’),和删除该区域时将自动删除。  不能创建或删除单独 SOA 记录。 

您可以修改 host 属性，即预配置为引用 Azure DNS 提供的主名称服务器名称除外的 SOA 记录的所有属性。

### <a name="spf-records"></a>SPF 记录

发件人策略框架 (SPF) 记录用于指定允许哪些电子邮件服务器发送电子邮件，代表一个给定的域名。  正确配置的 SPF 记录非常重要，若要禁止收件人标记为垃圾邮件电子邮件。

DNS Rfc 最初引入了新的 SPF 记录类型来支持此方案。 为了支持较旧的名称服务器，它们也允许指定 SPF 记录的 TXT 记录类型的使用。  此二义性导致混淆，解决了[RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)。  这规定 SPF 记录应只能创建使用 TXT 记录类型，并不建议使用 SPF 记录类型。 

**SPF 记录 Azure DNS 支持，应使用 TXT 记录类型创建。** 过时的 SPF 记录类型不受支持。 当[导入一个 DNS 区域文件](dns-import-export.md)，使用 SPF 记录类型的任何 SPF 记录转换为 TXT 记录类型。 

### <a name="srv-records"></a>SRV 记录

由各种服务使用的[SRV 记录](https://en.wikipedia.org/wiki/SRV_record)用于指定服务器位置。 当指定在 Azure DNS SRV 记录︰

- 必须为带有下划线前缀的记录集名称的一部分指定的*服务*和*协议*。  例如，\_的 sip。\_tcp.name。  区域顶点处的记录，则不需要指定'@'在该记录的名称，只需使用的服务和协议，如\_的 sip。\_tcp。
- *优先级*、*重量*、*端口*和*目标*被指定为记录集中的每条记录的参数。 

## <a name="tags-and-metadata"></a>标记和元数据

### <a name="tags"></a>标记
标记的名称-值对列表和标签的资源使用 Azure 资源管理器。  Azure 的资源管理器使用标记，使您 Azure 清单的筛选的视图，还使您能够设置标记是必需的策略。 有关标记的详细信息，请参阅[使用标签来组织您的 Azure 资源](../resource-group-using-tags.md)。

Azure DNS 支持 DNS 区域资源使用 Azure 资源管理器标记。  不支持 DNS 记录集上的标记。

### <a name="metadata"></a>元数据

为记录集标签或者，Azure DNS 支持批注记录集使用元数据。  与标签类似，元数据使您可以将名称-值对与每个记录集相关联。  这很有用，例如对记录的每个记录集的用途。  与标记，不同元数据不能用来提供筛选的视图的 Azure 账单并不能在 Azure 资源管理器策略中指定。

## <a name="limits"></a>限制

当使用 Azure DNS 时，下面的默认限制适用︰

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>下一步行动

- 要开始使用 Azure DNS，了解如何[创建 DNS 区域](dns-getstarted-create-dnszone-portal.md)以及[创建 DNS 记录](dns-getstarted-create-recordset-portal.md)。
- 若要迁移现有的 DNS 区域，了解如何[导入和 DNS 区域文件](dns-import-export.md)。
