<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>常见问题-Azure 分配 IP 地址的反向 DNS

### <a name="how-much-do-reverse-dns-records-cost"></a>多少做反向 DNS 记录成本？
它们是免费的 ！  没有免费的反向 DNS 记录或查询。

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>将从互联网解决我 Azure 分配公用 IP 地址的反向 DNS 记录？
是的。 一旦您设置公用 IP 地址的反向 DNS 属性，Azure 管理所有 DNS 委派和确保反向 DNS 记录解析为所有互联网用户所需的 DNS 区域。

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>将会为我公用 IP 地址创建的默认反向 DNS 记录？
不。 反向 DNS 是自愿加入的功能。 如果您选择不配置这些，创建没有默认的反向 DNS 记录。

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>完全限定的域名 (FQDN) 的格式是什么？
Fqdn 指定向顺序，必须通过一个点 (例如，"app1.contoso.com。") 结束。

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>如果反向 DNS 已指定验证检查失败，怎么样？
其中反向 DNS 的验证检查失败，服务管理操作将会失败。 请纠正反向 DNS 值为必需的然后重试。

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>我可以管理我的 Azure 网站反向 DNS？
Azure 网站不支持反向 DNS。 反向 DNS 支持的 Azure 的虚拟机。

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>我为我的公用 IP 地址配置多个反向 DNS 记录
不。 Azure 支持的每个公用 IP 地址的反向一条 DNS 记录。 但是每个公用 IP 地址可以有其自己的反向 DNS 记录。

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>可以配置 IPv6 公用 IP 地址的反向 DNS 记录？
不。  到目前为止，反向 DNS 记录支持 IPv4 公用 IP 地址只。

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>可以配置反向 DNS 记录我的公共 IP 地址而不指定 DomainNameLabel？
不。 利用公用 IP 地址的反向 DNS 记录，必须指定 DomainNameLabel 属性。

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>我可以发送电子邮件到外部域从我 Azure 计算的服务？
不。 [Azure 计算服务不支持向外部域发送电子邮件](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)。
