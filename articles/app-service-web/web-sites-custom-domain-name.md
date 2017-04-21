<properties
    pageTitle="自定义域名映射到 Azure 应用程序"
    description="了解如何自定义域名 （虚荣域） 映射到您的应用程序在 Azure 应用程序服务。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"
    tags="top-support-issue"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="cephalin"/>

# <a name="map-a-custom-domain-name-to-an-azure-app"></a>自定义域名映射到 Azure 应用程序

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

本文介绍如何手动将一个自定义域名映射到您的 web 应用程序、 移动应用程序的后端或 API 在[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)的应用程序。 

您的应用程序已配有一个独特的 azurewebsites.net 子域。 例如，如果您的应用程序名为**contoso**，其域名为**contoso.azurewebsites.net**。 但是，您可以将映射自定义域名称添加到应用程序如此，其 URL，如`www.contoso.com`，反映了自己的品牌。

>[AZURE.NOTE] 从 Azure [Azure 论坛](https://azure.microsoft.com/support/forums/)上的专家获得帮助。 对于更高程度的支持，转到[Azure 支持站点](https://azure.microsoft.com/support/options/)，然后单击**获取支持**。

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## <a name="buy-a-new-custom-domain-in-azure-portal"></a>在 Azure 门户购买一个新的自定义的域

如果您还没有购买自定义域名，可以购买并直接在您的应用程序设置[Azure 的门户网站](https://portal.azure.com)中管理它。 此选项很容易将一个自定义的域映射到您的应用程序，或不是您的应用程序使用[Azure 流量管理器](web-sites-traffic-manager-custom-domain-name.md)。 

有关说明，请参阅[购买应用程序服务的自定义域名称](custom-dns-web-site-buydomains-web-app.md)。

## <a name="map-a-custom-domain-you-purchased-externally"></a>映射自定义域从外部购买

如果您已购买的自定义的域从[Azure DNS](https://azure.microsoft.com/services/dns/)或第三方供应商提供，有三个主要步骤，以将自定义的域映射到您的应用程序︰

1. [ *（仅记录）*获取应用程序的 IP 地址](#vip)。
2. [创建映射到您的应用程序域的 DNS 记录](#createdns)。 
    - **其中**︰ 您域注册商管理工具 （例如 Azure DNS，GoDaddy 等）。
    - **为什么**︰ 以便于 Azure 应用程序自定义所需的域解析到知道您的域注册。
1. [启用您 Azure 应用程序的自定义域名](#enable)。
    - **其中**︰ [Azure 的门户](https://portal.azure.com)。
    - **为什么**︰ 所以您的应用程序知道响应对自定义域名的请求。
3. [验证 DNS 传播](#verify)。

### <a name="types-of-domains-you-can-map"></a>您可以将映射的域的类型

Azure 应用程序服务，您可以将以下类别的自定义的域映射到您的应用程序。

- **根域**-您的域注册保留的域名称 (由`@`通常的主机记录)。 例如， **contoso.com**。
- **子域**-下根域的任何域。 例如， **www.contoso.com** (由`www`的主机记录)。  您可以在 Azure 中的不同应用程序映射不同的同一根域的子域。
- **通配符域** - [其最左边的 DNS 标签是任何子域`*`](https://en.wikipedia.org/wiki/Wildcard_DNS_record)(如主机记录`*`， `*.blogs`)。 例如， ** \*。 contoso.com**。

### <a name="types-of-dns-records-you-can-use"></a>您可以使用 DNS 记录类型

根据您的需要，可以使用两种不同标准的 DNS 记录的映射您的自定义域︰ 

- [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) -自定义域名到 Azure 应用程序的虚拟 IP 地址直接映射。 
- [CNAME](https://en.wikipedia.org/wiki/CNAME_record) -将您的自定义域名映射到您的应用程序域名 Azure，**&lt;*应用程序名*>。 azurewebsites.net**。 

CNAME 的优点是，仍然存在跨 IP 地址更改。 如果删除并重新创建您的应用程序，或者从更高的定价层更改回**共享**层，可能会更改您的应用程序的虚拟 IP 地址。 通过此类更改，CNAME 记录是仍然有效，而 A 记录需要更新。 

本教程演示了使用 A 记录，还使用 CNAME 记录的步骤。

>[AZURE.IMPORTANT] 不要创建 CNAME 记录您的根域 （如"根记录"）。 有关详细信息，请参阅[为什么无法 CNAME 记录使用根域](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain)。
要将根域映射到 Azure 应用程序，使用 A 记录。

<a name="vip"></a>
## <a name="step-1-a-record-only-get-apps-ip-address"></a>第 1 步。 *（仅记录）*获取应用程序的 IP 地址
若要映射自定义域名使用 A 记录，您需要 Azure 应用程序的 IP 地址。 如果将映射将改为使用 CNAME 记录，请跳过此步骤并移动到下一节。

1.  登录到[Azure 的门户](https://portal.azure.com)。

2.  单击左侧菜单上的**应用程序服务**。

4.  单击您的应用程序，然后单击**自定义的域**。

6.  记下 IP 地址的主机名部分上方.

    ![映射自定义域名称记录︰ 您 Azure 应用程序服务的应用程序获取 IP 地址](./media/web-sites-custom-domain-name/virtual-ip-address.png)

7.  保持此门户刀片处于打开状态。 您将回来给它一次创建的 DNS 记录。

<a name="createdns"></a>
## <a name="step-2-create-the-dns-records"></a>第 2 步。 创建 DNS 记录

登录到您的域注册和使用他们的工具中添加一个记录或 CNAME 记录。 因此，您应该咨询您的提供程序的文档，是稍有不同，每个注册的用户界面。 但是，还有一些一般的指导原则。

1.  用于管理 DNS 记录到的页面。 查找链接或站点标记为**域名**、 **DNS**或**名称服务器管理**的区域。 通常情况下，您可以通过查看您的帐户信息，然后查看诸如**我的域**的链接中找到链接。
2.  寻找您可以添加或编辑 DNS 记录的链接。 这可能是一个**区域文件**或**DNS 记录**链接或**高级**配置链接。
3.  创建记录并保存您的更改。
    - [此处的说明 A 记录](#a)。
    - [此处的说明的 CNAME 记录](#cname)。

<a name="a"></a>
### <a name="create-an-a-record"></a>创建 A 记录

若要使用 A 记录映射到 Azure 应用程序的 IP 地址，您实际上需要创建 A 记录和 TXT 记录。 A 记录是用于 DNS 解析本身，和 TXT 记录的 Azure 来验证您自己的自定义域名。 

按照以下方式配置 A 记录(@通常表示根域):
 
<table cellspacing="0" border="1">
  <tr>
    <th>FQDN 示例</th>
    <th>一台主机</th>
    <th>一个值</th>
  </tr>
  <tr>
    <td>contoso.com （根）</td>
    <td>@</td>
    <td>来自 IP 地址<a href="#vip">第 1 步</a></td>
  </tr>
  <tr>
    <td>www.contoso.com （子）</td>
    <td>www</td>
    <td>来自 IP 地址<a href="#vip">第 1 步</a></td>
  </tr>
  <tr>
    <td>*。 contoso.com （通配符）</td>
    <td>*</td>
    <td>来自 IP 地址<a href="#vip">第 1 步</a></td>
  </tr>
</table>

您附加的 TXT 记录将映射从约定采用&lt;*子域*>。&lt; *rootdomain*> 到&lt;*应用程序名*>。 azurewebsites.net。 对 TXT 记录如下配置︰

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN 示例</th>
    <th>TXT 主机</th>
    <th>文本值</th>
  </tr>
  <tr>
    <td>contoso.com （根）</td>
    <td>@</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com （子）</td>
    <td>www</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
  <tr>
    <td>*。 contoso.com （通配符）</td>
    <td>*</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>
###创建 CNAME 记录

如果使用 CNAME 记录映射到 Azure 应用程序的默认域的名称，则不需要额外的 TXT 记录一样的 A 记录。 

>[AZURE.IMPORTANT] 不要创建 CNAME 记录您的根域 （如"根记录"）。 有关详细信息，请参阅[为什么无法 CNAME 记录使用根域](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain)。
要将根域映射到 Azure 应用程序，使用[A 记录](#a)。

按照以下方式配置您的 CNAME 记录(@通常表示根域):

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN 示例</th>
    <th>CNAME 主机</th>
    <th>CNAME 值</th>
  </tr>
  <tr>
    <td>www.contoso.com （子）</td>
    <td>www</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
  <tr>
    <td>*。 contoso.com （通配符）</td>
    <td>*</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>
##第 3 步。 使您的应用程序的自定义域名

在 Azure 门户中**自定义的域**刀片式服务器中 （见[第 1 步](#vip)），您需要向列表中添加您自定义的域的完全合格的域名称 (FQDN)。

1.  如果没有，请登录到[Azure 的门户](https://portal.azure.com)。

2.  在 Azure 的门户中，单击左侧菜单上的**应用程序服务**。

3.  单击您的应用程序，然后单击**自定义的域** > **添加主机名**。

4.  添加到列表中 (例如**www.contoso.com**) 您自定义的域的 FQDN。

    ![自定义域名映射到 Azure 应用程序︰ 将添加到列表中的域名称](./media/web-sites-custom-domain-name/add-custom-domain.png)

    >[AZURE.NOTE] Azure 将尝试验证您在此处使用的域名。 请确保它是您为其创建 DNS 记录在[步骤 2](#createdns)中相同的域名。 

5.  单击**验证**。

6.  在单击**验证**Azure 的启动时间域验证工作流。 这将检查域所有权以及主机可用性并报告成功或与规范性 guidence 如何修复该错误的详细的错误。    

7.  成功地验证**主机名添加**按钮将变为活动状态，您可以为分配主机名。 

8.  一旦 Azure 完成配置您新建的自定义域名后，导航到您在浏览器中的自定义域名。 浏览器将打开您 Azure 应用程序，这意味着您的自定义域名正确配置。

> [AZURE.NOTE] 如果 DNS 记录正在使用 （活动域服务通信方案），则需要预先您的 web 应用程序绑定到其域验证，然后只需创建 TXT 记录作为下表中所示的示例。 您附加的 TXT 记录将映射从约定采用&lt;*子域*>。&lt; *rootdomain*> 到&lt;*应用程序名*>。 azurewebsites.net。 
> <table cellspacing="0" border="1">
  <tr>
    <th>FQDN 示例</th>
    <th>TXT 主机</th>
    <th>文本值</th>
  </tr>
  <tr>
    <td>contoso.com （根）</td>
    <td>awverify.contoso.com</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com （子）</td>
    <td>awverify.www.contoso.com</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
    <tr>
    <td>*。 contoso.com （子）</td>
    <td>awverify.*.contoso.com</td>
    <td>&lt;<i>应用程序名</i>>。 azurewebsites.net</td>
  </tr>
</table>
一旦创建此 DNS 记录，返回到 Azure 门户并将您的自定义域名添加到您的 web 应用程序。
 

<a name="verify"></a>
##验证 DNS 传播

完成这些配置步骤后，可能需要一些时间，以便更改传播，具体取决于您的 DNS 提供商。 您可以验证 DNS 传播正在按预期的方式使用[http://digwebinterface.com/](http://digwebinterface.com/)。 浏览至网站后，在文本框中指定的主机名，然后单击**挖**。 验证结果，以确认最近的更改已生效。  

![自定义域名映射到 Azure 应用程序︰ 验证 DNS 传播](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] DNS 条目的传播可以最多 48 小时 （有时会再）。 如果您已正确配置的所有内容，您仍需要等待成功的传播。

## <a name="next-steps"></a>下一步行动
了解如何保护您的自定义域名使用 HTTPS 通过[购买在 Azure 中的 SSL 证书](web-sites-purchase-ssl-web-site.md)或[使用 SSL 证书的其他地方](web-sites-configure-ssl-certificate.md)。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

[开始使用 Azure DNS](../dns/dns-getstarted-create-dnszone.md)  
[为自定义的域中创建一个 web 应用程序的 DNS 记录](../dns/dns-web-sites-custom-domain.md)  
[对 Azure DNS 委派域](../dns/dns-domain-delegation.md)


<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
