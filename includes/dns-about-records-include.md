## <a name="about-records"></a>有关记录

每个 DNS 记录都有一个名称和一个类型。 记录被组织成各种根据它们所包含的数据类型。 最常见的类型是"A"记录，它将名称映射到 IPv4 地址。 另一种类型是"MX"的记录，它将名称映射到的邮件服务器。

Azure DNS 支持所有公共 DNS 记录类型，包括 A、 AAAA、 CNAME、 MX，NS、 PTR、 SOA、 SRV 和 TXT。 请注意︰
- 与每个区域自动创建 SOA 记录集，不能单独创建它们。
- 应通过使用 TXT 记录类型创建 SPF 记录。 有关详细信息，请参阅[此页](http://tools.ietf.org/html/rfc7208#section-3.1)。

在 Azure DNS 记录指定使用的相对名称。 "完全合格"的域名称 (FQDN) 包括区域名称，而不为"相对于"名称。 例如，相对的记录名称"www"区域"contoso.com"中提供了记录的完全限定的名 www.contoso.com。

## <a name="about-record-sets"></a>关于记录集

有时，您需要创建具有给定名称和类型的多个 DNS 记录。 例如，假设"www.contoso.com"网站位于两个不同的 IP 地址。 该网站要求两个不同的记录，另一个用于每个 IP 地址。 这是一种记录集︰

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS 可通过使用记录集来管理 DNS 记录。 记录集是在区域中，具有相同的名称和类型相同的 DNS 记录的集合。 大多数的记录集包含同一个记录，但示例此类似，在其中的记录集包含多个记录，并非少见。

SOA 和 CNAME 记录集是例外。 DNS 标准不允许对这些类型同名的多个记录。

生存，时间或 TTL 指定多长时间的每条记录被缓存客户端之前对其进行重新查询。 在此示例中，TTL 是 3600 秒或 1 小时。 TTL 指定为记录集，不是为每条记录，因此为该记录集内的所有记录中使用相同的值。

#### <a name="wildcard-record-sets"></a>通配符的记录集

Azure DNS 支持[通配符的记录](https://en.wikipedia.org/wiki/Wildcard_DNS_record)。 （除非有更匹配从非通配符记录集） 的任何具有匹配名称的查询返回。 除 NS 和 SOA 的所有记录类型支持通配符的记录集。  

若要创建通配符的记录集，请使用记录集名称"\*"。 或者，使用带有标签的名称"\*"，例如，"\*.foo"。

#### <a name="cname-record-sets"></a>CNAME 记录集

CNAME 记录集不能共存与其他具有相同名称的记录集。 例如，不能创建 CNAME 记录使用"www"的相对名称设置和 A 记录的相对名称"www"在同一时间。 因为区域 apex (名称 =‘@’)始终包含 NS 和 SOA 记录集创建创建该区域时，您不能创建在区域 apex 设置 CNAME 记录。 这些约束源自 DNS 标准并不是 Azure DNS 的限制。
