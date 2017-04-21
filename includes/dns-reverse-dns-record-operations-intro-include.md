## <a name="what-is-reverse-dns"></a>什么是反向 DNS？

传统的 DNS 记录启用映射从 DNS 名称 （例如 www.contoso.com) 为 IP 地址 （如 64.4.6.100)。  反向 DNS 使 IP 地址 (64.4.6.100) 返回到一个名称 (www.contoso.com) 的翻译。

在很多情况下使用反向 DNS 记录。 例如，反向 DNS 记录被广泛用于抗击通过验证电子邮件的发件人的电子邮件垃圾邮件。  接收的邮件服务器将发送服务器的 IP 地址的反向 DNS 记录中检索并验证该主机有权从起始域发送电子邮件。 (但请注意[Azure 计算服务不支持向外部域发送电子邮件](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)。)

## <a name="how-reverse-dns-works"></a>如何反向 DNS 的工作原理

在特殊的 DNS 区域，称为 ARPA' 区域承载反向 DNS 记录。  这些区域与宿主域如 'contoso.com' 普通层次结构并联构成单独的 DNS 层次结构。

例如，DNS 记录 'www.contoso.com' 实现 'contoso.com' 区域中使用 www 名称的 DNS A 记录。  此 A 记录指向对应的 IP 地址，在这种情况下 64.4.6.100。  反向查找被实现单独设置，使用 PTR 记录名为"100""6.4.64.in-addr.arpa"区域中的 （注意 IP 地址会颠倒 ARPA 区域中）。 此 PTR 记录，如果正确，请配置指向名称 www.contoso.com。

组织分配 IP 地址块后，他们还获得管理相应的 ARPA 区域的权限。 对应于使用 Azure 的 IP 地址块的 ARPA 区域是承载和管理由 Microsoft。 您的 ISP 可能主持 ARPA 区域，以供您自己的 IP 地址，或者也许可以承载您的选择，如 Azure DNS 的 DNS 服务中的 ARPA 区域。

>[AZURE.NOTE] 在单独的、 并行的 DNS 层次结构中实现了正向 DNS 搜索和反向 DNS 查找。 反向搜索的 'www.contoso.com'**不**承载在该区域 'contoso.com'，而是它位于相应的 IP 地址块的 ARPA 区域。

反向 DNS 的详细信息，请参阅[反向 DNS 查找](http://en.wikipedia.org/wiki/Reverse_DNS_lookup)。

## <a name="azure-support-for-reverse-dns"></a>反向 DNS 的 azure 支持

Azure 支持相关进行反向 DNS 的两个单独的方案︰

1. 主持 ARPA 区域对应 IP 地址块。
2. 使您可以配置到 Azure 服务分配的 IP 地址的反向 DNS 记录。

为了支持前者的 Azure DNS 可承载 ARPA 区域和管理每个反向 DNS 搜索的 PTR 记录。  创建 ARPA 区域、 建立委派，并配置 PTR 记录的过程是与常规 DNS 区域相同。  仅有的区别是必须通过您的 isp 联系，而不是 DNS 注册，配置委派，并应仅 PTR 记录类型。

若要支持后者，Azure 使您能够配置反向查找分配给您的服务的 IP 地址。  Azure 作为 PTR 记录相应的 ARPA 区域中配置此反向查找。  由 Microsoft 承载这些 ARPA 区域，对应于 Azure，所使用的 IP 范围。 **本文的其余部分介绍详细的这种情况。**
