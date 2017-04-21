<properties
    pageTitle="在云服务中配置自定义的域的名称 |Microsoft Azure"
    description="了解如何通过配置 DNS 设置公开 Azure 应用程序或自定义的域上的数据。"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>配置自定义域名 Azure 的云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-custom-domain-name-portal.md)
- [Azure 的传统门户网站](cloud-services-custom-domain-name.md)


创建云服务时，Azure 将其赋给一个 cloudapp.net 的子域中。 例如，如果您的云服务名为"contoso"，您的用户将能够访问应用程序的 URL，如 http://contoso.cloudapp.net。 Azure 还会指定一个虚拟 IP 地址。

但是，您也可以在您自己的域名，如 contoso.com 上公开您的应用程序。 本文解释如何保留或配置自定义域名为云服务 web 角色。

您是否已经 undestand 是什么 CNAME 和 A 记录？ [过去的解释的跳转](#add-a-cname-record-for-your-custom-domain)。

> [AZURE.NOTE]
> 获得转得更快 ！ 使用 Azure[指导演练](http://support.microsoft.com/kb/2990804)。 它使得关联自定义域名和安全管理单元的通信 (SSL) 使用 Azure 云服务或 Azure 网站。

<p/>

> [AZURE.NOTE]
> 在此任务中的步骤应用到 Azure 云服务。 应用程序服务，请参阅[此](../app-service-web/web-sites-custom-domain-name.md)。 对于存储帐户，请参阅[此](../storage/storage-custom-domain-name.md)。


## <a name="understand-cname-and-a-records"></a>理解和 CNAME 记录

CNAME （或别名记录），这两个记录允许您特定服务器相关联的域名称 （或在这种情况下，服务） 但是他们的工作方式不同。 也有一些特定的因素在决定使用哪一个之前应考虑的 Azure 云服务中使用 A 记录时。

### <a name="cname-or-alias-record"></a>CNAME 或别名记录

CNAME 记录将*特定*的域，如**contoso.com**或**www.contoso.com**，映射到规范域名。 在这种情况下，规范域名是**[myapp].cloudapp.net**您 Azure 的域名托管应用程序。 一旦创建，CNAME 别名**[myapp].cloudapp.net**。 CNAME 条目将解析的 IP 地址为您**[myapp].cloudapp.net**自动服务，因此如果云服务的 IP 地址发生变化，不需要执行任何操作。

> [AZURE.NOTE]
> 某些域注册只允许您使用 CNAME 记录，例如，www.contoso.com 和非根名称，如 contoso.com 时映射的子域。 CNAME 记录的详细信息，请参阅提供注册、 [CNAME 记录的维基百科条目](http://en.wikipedia.org/wiki/CNAME_record)或[IETF 域名︰ 实现和规范](http://tools.ietf.org/html/rfc1035)文档的文档。

### <a name="a-record"></a>一条记录

A 记录映射到域中的，如**contoso.com**或**www.contoso.com**，*或一个通配符域*如**\*。 contoso.com**，为 IP 地址。 在 Azure 云服务，该服务的虚拟 IP。 这样的 CNAME 记录通过 A 记录的主要优点是您可以使用通配符，如一项\* **。 contoso.com**，它可以处理对**mail.contoso.com**、 **login.contoso.com**或**www.contso.com**等多个子域的请求。

> [AZURE.NOTE]
> 因为 A 记录被映射到一个静态 IP 地址，它不能自动解析到云服务的 IP 地址的更改。 云服务使用的 IP 地址被分配的第一次将部署到空插槽 （生产环境或临时。）如果您删除了插槽的部署，通过 Azure 释放 IP 地址和任何未来部署到插槽中可能会获得新的 IP 地址。
>
> 为方便起见后临时和生产部署或执行就地升级现有部署之间交换,，并会保持一个给定的部署插槽 （生产或转移） 的 IP 地址。 执行这些操作的详细信息，请参阅[如何管理云服务](cloud-services-how-to-manage.md)。


## <a name="add-a-cname-record-for-your-custom-domain"></a>添加 CNAME 记录为您自定义的域

要创建 CNAME 记录，您必须添加一个新条目在 DNS 表中为您自定义的域使用注册所提供的工具。 每个注册指定的 CNAME 记录，类似但略有不同的方法，但概念是相同的。

1. 使用以下方法之一来查找**。 cloudapp.net**域名分配给您的云服务。

    * 登录到[Azure 的传统门户网站]，选择云服务，选择**仪表板**，然后**快速浏览**部分中可以找到该**网站 URL**条目。
    
        ![快速浏览部分显示该网站的 URL][csurl]
    
        **OR**  
    
    * 安装和配置[Azure Powershell](../powershell-install-configure.md)，然后使用下面的命令︰
        
        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    保存因为创建 CNAME 记录时将需要它在任意一种方式返回的 URL 中使用的域名。

1.  登录到您的 DNS 注册网站上，并转到页面用于管理 DNS。 查找链接或站点标记为**域名**、 **DNS**或**名称服务器管理**的区域。

2.  现在找到可以选择或输入 CNAME 的位置。 您可能需要向下，选择从中删除该记录类型或转到高级的设置页。 您应查找单词**CNAME**，**别名**或**子域**。

3.  如果您想要创建的别名**www.customdomain.com**，还必须为 CNAME，例如**www**提供的域或子域别名。 如果您想要为根域创建别名，它可能被列为**@**中注册 DNS 工具的符号。

4. 然后，必须提供一个规范的主机名，它在这种情况是您应用程序**cloudapp.net**域。

例如，下面的 CNAME 记录所有将通信转发从**www.contoso.com**到**contoso.cloudapp.net**，您部署的应用程序的自定义域名称︰

| 别名/主机名/子域 | 规范的域     |
| ------------------------- | -------------------- |
| www                       | contoso.cloudapp.net |

**Www.contoso.com**的访问者不会看到真正的主机 (contoso.cloudapp.net)，以便转发过程是对最终用户不可见。

> [AZURE.NOTE]
> 上面的示例仅适用于在**www**子域的通讯。 由于不能使用 CNAME 记录使用通配符，您必须创建一个 CNAME 为每个域/子域。 如果想要直接通信从子域，如\*。 contoso.com，到您的 cloudapp.net 地址，可以在您的 DNS 设置，配置一个**URL 重定向**或**URL 转发**项或创建 A 记录。


## <a name="add-an-a-record-for-your-custom-domain"></a>添加一个记录您自定义的域

若要创建一个记录，必须先找到你的云服务的虚拟 IP 地址。 然后通过您的注册商提供的工具添加您自定义的域的 DNS 表中的新项。 每个注册指定的 A 记录，类似但略有不同的方法，但概念是相同的。

1. 使用下列方法之一来获取您的云服务的 IP 地址。
    
    * 登录到[Azure 的传统门户网站]，选择云服务，选择**仪表板**，然后**快速浏览**部分中可以找到**公用虚拟 IP (VIP) 地址**条目。
    
        ![快速浏览部分显示 VIP][vip]
    
        **OR**  
    
    * 安装和配置[Azure Powershell](../powershell-install-configure.md)，然后使用下面的命令︰
    
        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    如果您有多个终结点与云服务相关联，您将收到包含 IP 地址的多个行，但所有应显示相同的地址。
    
    当您需要它时创建的记录保存 IP 地址。

1.  登录到您的 DNS 注册网站上，并转到页面用于管理 DNS。 查找链接或站点标记为**域名**、 **DNS**或**名称服务器管理**的区域。

2.  现在发现，选择或输入一个记录。 您可能需要向下，选择从中删除该记录类型或转到高级的设置页。

3. 选择或输入域或子域将使用此 A 记录。 如果您想要创建的别名**www.customdomain.com**，例如，选择**www** 。 如果您要为创建一个通配符条目的所有子域，请输入__*__。 这将涵盖如**mail.customdomain.com**、 **login.customdomain.com**和**www.customdomain.com**的所有子域。

    如果您想要创建的根域 A 记录，它可能被列为**@**中注册 DNS 工具的符号。

4. 在提供的字段中输入您的云服务的 IP 地址。 这将使用云服务部署的 IP 地址的记录中的域条目相关联。

例如，下面一条记录将从**contoso.com**所有通信量都转发到**137.135.70.239**，您部署的应用程序的 IP 地址︰

| 主机名称/子域 | IP 地址     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |



此示例演示如何创建根域 A 记录。 如果您想要创建一个通配符条目，以涵盖所有子域，则输入__*__作为子域。

>[AZURE.WARNING]
>在 Azure 中的 IP 地址是动态的默认情况下。 您可能需要使用[保留的 IP 地址](../virtual-network/virtual-networks-reserved-public-ip.md)，以确保您的 IP 地址不会改变。

## <a name="next-steps"></a>下一步行动

* [如何管理云服务](cloud-services-how-to-manage.md)
* [如何 CDN 的内容映射到自定义的域](../cdn/cdn-map-content-to-custom-domain.md)
* [云服务的常规配置](cloud-services-how-to-configure.md)。
* 了解如何[部署云服务](cloud-services-how-to-create-deploy.md)。
* 配置[ssl 证书](cloud-services-configure-ssl-certificate.md)。




[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure 的传统门户网站]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
 