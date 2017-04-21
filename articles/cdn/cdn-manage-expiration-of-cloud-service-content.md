<properties
 pageTitle="如何管理 Azure 的 Web 应用程序/云服务，ASP.NET，IIS 在 Azure CDN 的内容过期 |Microsoft Azure"
 description="介绍如何管理 Azure CDN 中的云服务内容过期"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>如何管理 Azure 的 Web 应用程序/云服务，ASP.NET 或 IIS 在 Azure CDN 的内容过期

> [AZURE.SELECTOR]
- [Azure 的 Web 应用程序/云服务，ASP.NET 或 IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure 存储 blob 服务](cdn-manage-expiration-of-blob-content.md)

可以在 Azure CDN 缓存任何公共可访问的原始 web 服务器中的文件，直到其生存时间 (TTL) 经过。  TTL 是取决于来自源服务器的 HTTP 响应中的[*缓存控制*标头](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)。  这篇文章描述如何设置`Cache-Control`Azure Web 应用程序、 Azure 云服务，ASP.NET 应用程序，和 Internet Information Services 网站，同样配置的所有标头。

>[AZURE.TIP] 您可以选择任何 TTL 设置文件上。  在这种情况下，Azure CDN 自动应用默认的 TTL 数为七天。
>
>加快访问的文件和其他资源的 Azure CDN 工作原理的更多信息，请参阅[Azure CDN 概述](./cdn-overview.md)。

## <a name="setting-cache-control-headers-in-configuration"></a>在配置中设置缓存控制标头

对于静态内容，如图像和样式表，您可以通过修改的**applicationHost.config**或**web.config**文件的 web 应用程序控制更新频率。  设置配置文件中的**system.webServer\staticContent\clientCache**元素，将`Cache-Control`为您的内容的标题。 对于**web.config**文件中，配置设置将影响一切在该文件夹及其所有子文件夹，除非覆盖在子文件夹级别。  例如，可以设置默认生存时间有 3 天，缓存的所有静态内容的根部，但包含子文件夹具有多变量内容缓存设置为 6 小时。  对于**applicationHost.config**，网站上的所有应用程序会受到影响，但可以重写在应用程序的**web.config**文件中。

以下 XML 显示和设置**clientCache**以指定 3 天的最长期限的示例︰  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

指定**UseMaxAge**会增加`Cache-Control: max-age=<nnn>`在**CacheControlMaxAge**属性中指定的值基于响应标头。 时间跨度的格式是**cacheControlMaxAge**属性为<days>。<hours>:<min>:<sec>. **ClientCache**节点的详细信息，请参阅[客户端缓存<clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)。  

## <a name="setting-cache-control-headers-in-code"></a>在代码中设置缓存控制标头

对于 ASP.NET 应用程序，您可以设置通过设置**HttpResponse.Cache**属性以编程方式缓存行为的 CDN。 **HttpResponse.Cache**属性的详细信息，请参阅[HttpResponse.Cache 属性](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx)和[HttpCachePolicy 类](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)。  

如果您希望以编程方式缓存的 ASP.NET 应用程序内容，请确保内容被标记为可缓存通过将 HttpCacheability 设置为*公共*。 另外，请确保缓存验证程序的设置。 高速缓存验证程序可以是最后一次修改时间戳设置通过调用 SetLastModified 或一个 etag 值设置通过调用 SetETag。 或者，您还可以通过调用 SetExpires，指定缓存过期时间或您可以依靠本文档前面所述的默认缓存试探法。  

例如，到缓存一小时的内容，添加以下代码︰  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>下一步行动

- [阅读有关**clientCache**元素的详细信息](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [阅读的**HttpResponse.Cache**属性的文档](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [读**HttpCachePolicy 类**的文档](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)。  
