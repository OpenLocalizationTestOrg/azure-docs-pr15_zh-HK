<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>常见问题-Azure 分配 IP 地址的反向 DNS

### <a name="how-much-do-reverse-dns-records-cost"></a>多少做反向 DNS 记录成本？
它们是免费的 ！  没有免费的反向 DNS 记录或查询。

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>将从互联网解决我的反向 DNS 记录？
是的。 一旦将反向 DNS 属性设置为您的云服务，Azure 管理所有 DNS 委派和确保反向 DNS 记录解析为所有互联网用户所需的 DNS 区域。

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>将为我的云服务创建的默认反向 DNS 记录？
不。 反向 DNS 是自愿加入的功能。 如果您选择不配置这些，创建没有默认的反向 DNS 记录。

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>完全限定的域名 (FQDN) 的格式是什么？
Fqdn 指定向顺序，必须通过一个点 (例如，"app1.contoso.com。") 结束。

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>如果反向 DNS 已指定验证检查失败，怎么样？
其中反向 DNS 的验证检查失败，服务管理操作将会失败。 请纠正反向 DNS 值为必需的然后重试。

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>我可以管理我的 Azure 网站反向 DNS？
Azure 网站不支持反向 DNS。 反向 DNS 支持 Azure PaaS 角色和 IaaS 的虚拟机。

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>我为我的云服务配置多个反向 DNS 记录
不。 Azure 支持的每个 Azure 云服务的一个反向 DNS 记录。 但是每个 Azure 云服务可以有他们自己的反向 DNS 记录。

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>我可以发送电子邮件到外部域从我 Azure 计算的服务？
不。 [Azure 计算服务不支持向外部域发送电子邮件](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)。
