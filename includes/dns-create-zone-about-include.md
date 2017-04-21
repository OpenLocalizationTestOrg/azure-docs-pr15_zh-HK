DNS 区域用于承载特定域的 DNS 记录。 为了启动承载您的域，您需要创建 DNS 区域。 为特定的域创建任何 DNS 记录将会在 DNS 区域的域内。 

例如，域"contoso.com"可能包含大量的 DNS 记录，如"mail.contoso.com"（用于邮件服务器） 和"www.contoso.com"（对于 web 站点）。 


## <a name="names"></a>有关 DNS 区域名称
 
- 区域名称中必须是唯一的资源组中，并且区域必须已存在。 否则，操作将失败。

- 同一区域名称可以重复使用不同的资源组或其他 Azure 订阅中。 

- 多个区域共享相同的名称，每个实例都将分配到不同的名称服务器地址，并可以从父域委派只能有一个实例。 有关详细信息，请参阅[委派到 Azure DNS 域](../articles/dns/dns-domain-delegation.md)。