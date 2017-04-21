<properties
     pageTitle="如何将 Azure 内容传递网络 (CDN) 内容映射到自定义的域 |Microsoft Azure"
     description="本主题演示如何 CDN 的内容映射到自定义的域。"
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>如何将自定义的域映射到内容传递网络 (CDN) 终结点
要缓存的内容，而不是使用 azureedge.net 的子域的 Url 中使用您自己的域名，可以将自定义的域映射到的 CDN 终结点。

有两种方法可以将您自定义的域映射到一个 CDN 终结点︰

1. [使用您的域注册创建 CNAME 记录，并对 CDN 终结点映射您的自定义的域和子域](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    CNAME 记录是一种类似映射源域，DNS 功能`www.contosocdn.com`或`cdn.contoso.com`，到目标域。 在这种情况下，源域是您自定义的域和子域 （子域， **www**或**cdn**总是需要一样）。 目标域是您 CDN 终结点。  

    CDN 端点映射您自定义的域的过程可以但是，导致的停机时间域在短时间内虽然要在 Azure 门户注册域。

2. [添加**cdnverify**中间注册步骤︰](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    如果您自定义的域当前支持的应用程序服务级别协议 (SLA) 的要求，能不停机的情况，然后可以使用 Azure **cdnverify**子域提供中间注册步骤，以便用户将能够访问您的域 DNS 映射发生时。  

注册自定义使用上述过程中的一个域后，您将需要[验证自定义子域引用 CDN 端点](#verify-that-the-custom-subdomain-references-your-cdn-endpoint)。

> [AZURE.NOTE] 您必须与您的域注册，将您的域映射到 CDN 的终结点创建 CNAME 记录。 CNAME 记录映射特定子域如`www.contoso.com`或`cdn.contoso.com`。 不能将 CNAME 记录映射到一个根域，如`contoso.com`。
>    
> 子域只能与一个 CDN 终结点关联。 创建 CNAME 记录将路由所有通信发送到指定终结点到子域。  例如，如果您将`www.contoso.com`使用 CDN 的终结点，然后您不能将其与其他 Azure 的终结点，例如存储帐户终结点或云服务终结点。 但是，可以为不同的服务终结点相同的域中使用不同的子域。 您还可以映射到同一个 CDN 终结点的不同的子域。
>
> **从 Verizon 的 Azure CDN** （标准和特优） 终结点，请注意它会占用为自定义的域更改传播给 CDN 边缘节点**90 分钟**。

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>注册自定义 Azure CDN 终结点的域

1.  登录到[Azure 的门户](https://portal.azure.com/)。
2.  单击**浏览**，然后为**CDN 的配置文件**，然后 CDN 配置文件与您要映射到自定义的域的终结点。  
3.  在**CDN 配置文件**刀片式服务器，您希望将该子域的 CDN 终结点。
4.  在刀片式服务器终结点的顶部，单击**添加自定义域**按钮。  在**将自定义的域添加**刀片式服务器，您将看到 CDN 终结点，可以在中创建的新 CNAME 记录使用从派生的端点主机名称。 主机名地址的格式将显示为**&lt;EndpointName >。 azureedge.net**。  您可以将复制用于创建 CNAME 记录此主机名。  
5.  导航到您的域注册的 web 站点，并找到用于创建 DNS 记录的部分。 您可能会发现这在**域名**、 **DNS**或**名称服务器管理**等部分。
6.  用于管理 Cname 找到的部分。 您可能需要转到高级的设置页，请查找词组 CNAME、 别名或子域。
7.  提供**自定义的域中添加**刀片式服务器中的主机名创建映射您选择 （例如， **www**或**cdn**） 的子域的新 CNAME 记录。
8.  返回到**添加自定义域**刀片式服务器，并输入您自定义的域，包括该子域中，在对话框中。 例如，格式输入的域名`www.contoso.com`或`cdn.contoso.com`。   

    Azure 将验证您输入的域名存在 CNAME 记录。 CNAME 是正确的如果您自定义的域进行验证。  对于**从 Verizon 的 Azure CDN** （标准和特优） 终结点，可能需要自定义的域设置传播到所有的 CDN 边缘节点，但是达 90 分钟。  

    请注意，在某些情况下它可以传播到 Internet 上的名称服务器的 CNAME 记录的时间。 如果不立即验证您的域，并且您认为 CNAME 记录正确无误，然后等待几分钟后再试。


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>注册自定义 Azure CDN 终结点使用中间的 cdnverify 子域的域  

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 单击**浏览**，然后为**CDN 的配置文件**，然后 CDN 配置文件与您要映射到自定义的域的终结点。  
3. 在**CDN 配置文件**刀片式服务器，您希望将该子域的 CDN 终结点。
4. 在刀片式服务器终结点的顶部，单击**添加自定义域**按钮。  在**将自定义的域添加**刀片式服务器，您将看到 CDN 终结点，可以在中创建的新 CNAME 记录使用从派生的端点主机名称。 主机名地址的格式将显示为**&lt;EndpointName >。 azureedge.net**。  您可以将复制用于创建 CNAME 记录此主机名。
5. 导航到您的域注册的 web 站点，并找到用于创建 DNS 记录的部分。 您可能会发现这在**域名**、 **DNS**或**名称服务器管理**等部分。
6. 用于管理 Cname 找到的部分。 您可能需要转到高级的设置页查找词**CNAME**，**别名**或**子域**。
7. 创建新的 CNAME 记录，并提供包括**cdnverify**子域的子域别名。 例如，您指定的子域将格式**cdnverify.www**或**cdnverify.cdn**。 然后提供主机名，它是您 CDN 的终结点，以**格式 cdnverify。&lt;EndpointName >。 azureedge.net**。
8. 返回到**添加自定义域**刀片式服务器，并输入您自定义的域，包括该子域中，在对话框中。 例如，格式输入的域名`www.contoso.com`或`cdn.contoso.com`。 请注意，在此步骤中，您不需要与**cdnverify**子域作为开头。  

    Azure 将验证您输入的 cdnverify 域名存在 CNAME 记录。
9. 此时，您自定义的域经过 Azure，但向您的域的通信将不被路由到 CDN 端点。 等待足够长的时间以允许自定义的域设置传播到 CDN 边缘节点 （90 分钟**从 Verizon 的 Azure CDN**， **Akamai 从 Azure CDN**的 1-2 分钟），返回到您的 DNS 注册商的 web 站点并创建另一个 CNAME 记录后，它将映射到 CDN 端点的子域。 例如，为**www**或**cdn**和作为主机名指定子域**&lt;EndpointName >。 azureedge.net**。 使用此步骤中，您的自定义域名注册已完成。
10. 最后，您可以删除使用**cdnverify**，您创建 CNAME 记录一样，有必要仅作为一个中间步骤。  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>验证自定义子域引用 CDN 端点

- 完成您的自定义域名注册后，可以在您使用自定义的域的 CDN 终结点来访问缓存的内容。
首先，确保您已缓存端点处的公共内容。 例如，如果 CDN 端点关联使用存储帐户，CDN 缓存中公钥 blob 容器的内容。 若要测试自定义的域，请确保容器被设置为允许公共访问权限，并且包含至少一个 blob。
- 在浏览器中，导航到 blob 使用自定义的域的地址。 例如，如果您自定义的域为`cdn.contoso.com`，blob 缓存的 URL 将类似于下面的 URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>请参见

[如何启用的 Azure 的内容传递网络 (CDN)](./cdn-create-new-endpoint.md)  
