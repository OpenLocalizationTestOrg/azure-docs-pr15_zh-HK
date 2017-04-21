<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>常见问题-承载 ARPA 在 Azure DNS 区域

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>可以在 Azure DNS 上我的 ISP 分配的 IP 块承载 ARPA 区域？
是的。 完全支持承载 ARPA 区域为自己在 Azure DNS 的 IP 地址范围。

只需[创建在 Azure DNS 区域](dns-getstarted-create-dnszone.md)，然后与您的 ISP 为[委派区域](dns-domain-delegation.md)的工作。  然后，您可以管理对每个反向搜索 PTR 记录与其他记录类型相同的方式。

您还可以[导入使用 Azure CLI 现有反向查找区域](dns-import-export.md)。

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>托管我 ARPA 区域成本多少呢？
您的 ISP 分配 ip 承载 ARPA 区域在[Azure DNS 的标准费率](https://azure.microsoft.com/pricing/details/dns/)收取 Azure DNS 中的块。

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>我可以承载在 Azure DNS 中的 IPv4 和 IPv6 地址的 ARPA 区域？
是的。