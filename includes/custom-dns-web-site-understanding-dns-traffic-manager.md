域名系统 (DNS) 用于在 internet 上找到的事情。 例如，当您在您的浏览器中输入地址或单击 web 页上的链接，它使用 DNS 来将域转换为 IP 地址。 IP 地址就像一个街道地址，但它不是人类非常友好。 例如，就更容易记住 DNS 名称类似**contoso.com**比记住一个 IP 地址，例如 192.168.1.88 或 2001:0:4137:1f67:24a2:3888:9cce:fea3。

DNS 系统基于*记录*。 记录将一个特定*名称*，如**contoso.com**，与另一个 DNS 名称或 IP 地址相关联。 当一个应用程序，如 web 浏览器查找 DNS 中的名称时，它查找记录，并使用任何它指向的地址。 如果它指向的值是一个 IP 地址，浏览器将使用该值。 如果它指向另一 DNS 名称时，应用程序将不得不再次进行解析。 从根本上讲，所有的名称解析将结束 IP 地址中。

Azure 网站创建时，DNS 名称自动分配给该站点。 此名称将采用的形式**&lt;yoursitename&gt;。 azurewebsites.net**。 作为 Azure 流量管理器终结点添加您的网站时，您的网站是否可以通过访问**&lt;yourtrafficmanagerprofile&gt;。 trafficmanager.net**域。

> [AZURE.NOTE] 在您的网站被配置为一个流量管理器终结点，您可以使用**。 trafficmanager.net**地址创建 DNS 记录时。

> 您只能使用 CNAME 记录使用通信管理器

也有多种类型的记录，每一个都有自己的功能和限制，但网站作为通信管理器终结点配置为，我们只关心大约一个;*CNAME*记录。

###<a name="cname-or-alias-record"></a>CNAME 或别名记录

CNAME 记录将一个*特定*的 DNS 名称，如**mail.contoso.com**或**www.contoso.com**，映射到另一个 （规范） 的域名。 在 Azure 网站使用流量管理器的情况下是规范域名**&lt;myapp >。 trafficmanager.net**流量管理器配置文件的域的名称。 一旦创建，CNAME 别名**&lt;myapp >。 trafficmanager.net**域名。 CNAME 条目将解析的 IP 地址为您**&lt;myapp >。 trafficmanager.net**域名自动，因此如果该网站的 IP 地址发生变化，不需要采取任何措施。

通信管理器在通信后，它随后将通信路由到您的网站，使用负载平衡的方法为配置。 这是对您的网站的访问者完全透明。 他们只会看到他们的浏览器中的自定义域名。

> [AZURE.NOTE] 某些域注册只允许您使用 CNAME 记录，例如， **www.contoso.com**和非根名称，如**contoso.com**时映射的子域。 CNAME 记录的详细信息，请参阅提供注册、 <a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME 记录的维基百科条目</a>或<a href="http://tools.ietf.org/html/rfc1035">IETF 域名︰ 实现和规范</a>文档的文档。
