域名系统 (DNS) 用于在 internet 上查找资源。 例如，当您在浏览器中，输入 web 应用程序地址或单击 web 页上的链接，它使用 DNS 来将域转换为 IP 地址。 IP 地址就像一个街道地址，但它不是人类非常友好。 例如，就更容易记住 DNS 名称类似**contoso.com**比记住一个 IP 地址，例如 192.168.1.88 或 2001:0:4137:1f67:24a2:3888:9cce:fea3。

DNS 系统基于*记录*。 记录将一个特定*名称*，如**contoso.com**，与另一个 DNS 名称或 IP 地址相关联。 当一个应用程序，如 web 浏览器查找 DNS 中的名称时，它查找记录，并使用任何它指向的地址。 如果它指向的值是一个 IP 地址，浏览器将使用该值。 如果它指向另一 DNS 名称时，应用程序将不得不再次进行解析。 从根本上讲，所有的名称解析将结束 IP 地址中。

应用程序服务中创建 web 应用程序时，DNS 名称自动分配给该 web 应用程序。 此名称将采用的形式**&lt;yourwebappname&gt;。 azurewebsites.net**。 还有一个虚拟 IP 地址可供使用时创建 DNS 记录，因此您可以创建指向记录**。 azurewebsites.net**，或您可以指向的 IP 地址。

> [AZURE.NOTE] 如果删除并重新创建 web 应用程序，或者应用程序服务计划模式改为**免费**后设**基本**、**共享**或**标准**,，则将更改您的 web 应用程序的 IP 地址。

也有多种类型的记录，每一个都有自己的功能和限制，但是对于 web 应用程序我们只关心两种，*一个*和*CNAME*记录。

###<a name="address-record-a-record"></a>地址记录 （记录）

A 记录映射到域中的，如**contoso.com**或**www.contoso.com**，*或一个通配符域*如**\*。 contoso.com**，为 IP 地址。 Web 应用程序在应用程序服务，在虚拟 IP 的服务或一个特定的 IP 地址您为您的 web 应用程序购买。

A 记录的 CNAME 记录上的主要优点是︰

* 可以将根如**contoso.com**域映射到 IP 地址;许多注册只允许使用 A 记录

* 您可以使用通配符，如一项**\*。 contoso.com**，它可以处理对**mail.contoso.com**、 **blogs.contoso.com**或**www.contso.com**等多个子域的请求。

> [AZURE.NOTE] 因为 A 记录被映射到一个静态 IP 地址，它不能自动解析到您的 web 应用程序的 IP 地址的更改。 当您配置自定义的域的名称设置为您的 web 应用程序; 提供与记录一起使用的 IP 地址但是，此值可能会更改如果您删除并重新创建 web 应用程序，或更改要回**自由**的应用程序服务计划模式。

###<a name="alias-record-cname-record"></a>别名 （CNAME 记录） 记录

CNAME 记录将一个*特定*的 DNS 名称，如**mail.contoso.com**或**www.contoso.com**，映射到另一个 （规范） 的域名。 如果应用程序服务 Web 应用程序，规范域名是**&lt;yourwebappname >。 azurewebsites.net**您的 web 应用程序的域名。 一旦创建，CNAME 别名**&lt;yourwebappname >。 azurewebsites.net**域名。 CNAME 条目将解析的 IP 地址为您**&lt;yourwebappname >。 azurewebsites.net**域名自动，因此如果 web 应用程序的 IP 地址发生变化，不需要执行任何操作。

> [AZURE.NOTE] 某些域注册只允许您使用 CNAME 记录，例如， **www.contoso.com**和非根名称，如**contoso.com**时映射的子域。 CNAME 记录的详细信息，请参阅提供注册、 <a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME 记录的维基百科条目</a>或<a href="http://tools.ietf.org/html/rfc1035">IETF 域名︰ 实现和规范</a>文档的文档。

###<a name="web-app-dns-specifics"></a>Web 应用程序 DNS 的详细信息

Web 应用程序中使用 A 记录需要您首先创建一个下面的 TXT 记录︰

* **根域**的 DNS TXT 记录**@**到**&lt;yourwebappname&gt;。 azurewebsites.net**。

* **对于特定的子域**的 DNS 名称**&lt;子域名 >**到**&lt;yourwebappname&gt;。 azurewebsites.net**。 例如，**博客**A 记录是否为**blogs.contoso.com**。

* **为通配符子-dodmains** -DNS TXT 记录的 *** 到**&lt;yourwebappname&gt;。 azurewebsites.net**。

此 TXT 记录用于验证您拥有您正在尝试使用的域。 这是在创建指向您的 web 应用程序的虚拟 IP 地址的 A 记录。

您可以查找的 IP 地址和**。 azurewebsites.net**名称为您的 web 应用程序，请执行以下步骤︰

1. 在浏览器中，打开[Azure 门户](https://portal.azure.com)。

2. 在**Web 应用程序**刀片式服务器，请单击您的 web 应用程序的名称，然后选择从页面底端的**自定义的域**。

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. 在**自定义的域**刀片式服务器，您将看到虚拟 IP 地址。 保存此信息，因为它将用于创建 DNS 记录时

    ![](./media/custom-dns-web-site/virtual-ip-address.png)

    > [AZURE.NOTE] 您不能自定义域名使用**免费**的 web 应用程序，并必须升级到**共享**、**基本**、**标准**或**特优**层的应用程序服务计划。 有关详细信息的应用程序服务计划定价层，包括如何更改定价层 web 应用程序，请参阅[如何扩展 web 应用程序](../articles/web-sites-scale.md)。
