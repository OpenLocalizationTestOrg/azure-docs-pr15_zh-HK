<properties 
    pageTitle="使用 Azure Redis Azure 应用程序服务中的高速缓存的会话状态" 
    description="了解如何使用 Azure 缓存服务以支持 ASP.NET 会话状态缓存。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>使用 Azure Redis Azure 应用程序服务中的高速缓存的会话状态


本主题说明如何使用会话状态的 Azure Redis 缓存服务。

如果您的 ASP.NET web 应用程序中使用会话状态，您将需要配置外部的会话状态提供程序 （Redis 缓存服务或 SQL Server 会话状态提供程序）。 如果使用会话状态，并且不使用外部提供程序时，您将被限制到您的 web 应用程序的一个实例。 该 Redis 缓存服务是最快和最简单的方法使。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>创建高速缓存
按照[这些说明进行操作](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache)来创建高速缓存。

##<a id="configureproject"></a>将 RedisSessionStateProvider NuGet 程序包添加到您的 web 应用程序
安装 NuGet`RedisSessionStateProvider`软件包。  使用下面的命令来安装程序包管理器控制台 (**工具** > **NuGet 程序包管理器** > **程序包管理器控制台**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
从**工具**安装 > **NuGet 程序包管理器** > **解决方案管理金块包**，搜索`RedisSessionStateProvider`。

详细信息请参阅[NuGet RedisSessionStateProvider 页](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ )上，然后[配置缓存客户端](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet)。

##<a id="configurewebconfig"></a>修改 Web.Config 文件
除了进行缓存的程序集引用，NuGet 程序包在*web.config*文件中添加存根 （stub） 的条目。 

1. 打开*web.config*并查找**用户**元素。

1. 输入的值为`host`， `accessKey`， `port` （SSL 端口应 6380），并设置`SSL`到`true`。 这些值可以从[Azure 门户](http://go.microsoft.com/fwlink/?LinkId=529715)刀片式服务器的缓存实例。 有关详细信息，请参阅[连接到高速缓存](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache)。 请注意，使用非 SSL 端口被禁用默认情况下，新的高速缓存。 有关启用非 SSL 端口的详细信息，请参阅[配置 Redis Azure 的高速缓存中的缓存](https://msdn.microsoft.com/library/azure/dn793612.aspx)主题中的[访问端口](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts)部分。 下面的标记显示修改*web.config*文件，专门更改*端口*、*主机*、 accessKey*，和*ssl *。

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>在代码中使用会话对象
最后一步是要开始在 ASP.NET 代码中使用会话对象。 通过使用**Session.Add**方法将对象添加到会话状态。 此方法使用键 / 值对存储在会话状态高速缓存项。

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

下面的代码从会话状态中检索该值。

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

此外可以使用 Redis 的高速缓存对象缓存 web 应用程序中。 更多的信息，请参阅[影片应用 MVC Azure Redis 缓存在 15 分钟内](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/)。
有关如何使用 ASP.NET 会话状态的详细信息，请参见[ASP.NET 会话状态概述][]。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

  *由[Rick Anderson](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.NET 会话状态概述]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
