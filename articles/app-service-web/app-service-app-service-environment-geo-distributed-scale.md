<properties 
    pageTitle="地理分布与应用程序服务环境的比例" 
    description="了解如何来进行横向扩展应用程序使用具有流量管理器和应用程序服务环境地理分布。" 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>地理分布与应用程序服务环境的比例

## <a name="overview"></a>概述 ##
应用程序方案需要非常高的比例可以超过单个的应用程序部署到可用的计算资源容量。  投票的应用程序，体育活动和 televised 的娱乐事件是典型的需要极高比例的情况。 水平扩展与正在进行的单个区域内和跨区域，处理极端负载要求的多个应用程序部署的应用程序，能够满足高比例要求。

应用程序服务环境是一个理想的平台，为水平向外扩展。  一次应用程序服务环境在选择配置，以支持已知的请求速率，开发人员可以部署其他应用程序服务环境进行"切割"的方式即可获得所需的峰值负载能力。

例如，假设应用程序服务环境配置上运行的应用程序已经过测试，20 K 每秒请求数 (RPS) 处理。  如果所需的峰值负载容量是 10 万 RPS，然后五 （5） 应用程序服务环境可以创建和配置，以确保应用程序可以处理的最大预测的负荷。

由于客户通常访问应用程序使用的任何自定义 （或虚荣） 域，开发人员需要一种方法来分发应用程序请求跨所有应用程序服务环境实例。  实现此目的的一个好办法是解决使用[Azure 流量管理器配置文件]的自定义的域[AzureTrafficManagerProfile]。  通信管理器配置文件可以配置为指向的所有单个应用程序服务环境。  通信管理器自动将所有基于负载平衡通信管理器配置文件中的设置应用程序服务的环境中处理分发客户。  此方法有效，无论是否部署在单个 Azure 区域，或多个 Azure 区域间在全球部署的所有应用程序服务环境。

此外，客户通过虚荣域访问应用程序，因为客户是数量的知道运行应用程序的应用程序服务环境。  因此开发人员可以快速而轻松地添加和移除，根据观测到的通信负载应用程序服务环境。

概念图描述了应用程序在单个区域中的三种应用程序服务环境水平放大。

![概念性体系结构][ConceptualArchitecture] 

本主题的其余部分遍历设置分布式拓扑的示例应用程序使用多个应用程序服务环境所涉及的步骤。

## <a name="planning-the-topology"></a>规划的拓扑结构 ##
之前构建分布式应用程序占用的空间，将有助于提前几条信息。

- **自定义应用程序域︰** 客户将用于访问应用程序的自定义域名称是什么？  对于示例应用程序的自定义域名是*www.scalableasedemo.com*
- **管理器通信域︰** 需要创建一个[Azure 流量管理器配置文件]时选择一个域名[AzureTrafficManagerProfile]。  此名称将与*trafficmanager.net*后缀注册域的项由通信管理器管理的结合。  示例应用程序，选择的名称提供了*可扩展 ase 演示*。  因此由通信管理器管理的完整域名是*可伸缩 ase demo.trafficmanager.net*。
- **进行缩放的应用程序占用的策略︰** 将应用程序占用的空间分布于单个区域中的多个应用程序服务环境吗？  多个区域？  混合和匹配这两种方法吗？  决定应基于期望的客户通信将源自何处，以及如何更好地可缩放应用程序的支持后端基础结构的其余部分。  例如，用 100%的无状态应用程序，应用程序可以大规模应用缩放每个 Azure 地区，乘以在 Azure 的多个区域部署的应用程序服务环境中使用多个应用程序服务环境的组合。  与 15 + Azure 公用 — 提供可供选择的地区，客户可以真正构建世界级超大规模应用程序占用的空间。  为使用这篇文章的示例应用程序的情况下，三种应用程序服务环境创建在单个 Azure 区域 （美国）。
- **应用程序服务环境的命名约定︰** 每个应用程序服务环境需要唯一的名称。  超过一个或两个应用程序服务的环境是有助于命名约定来帮助您识别每个应用程序的服务环境。  为示例应用程序使用了一个简单的命名约定。  三种应用程序服务环境的名称是*fe1ase*、 *fe2ase*和*fe3ase*。
- **应用程序的命名约定︰** 由于将部署应用程序的多个实例，为已部署的应用程序的每个实例需要一个名称。  应用程序服务环境的一个很少有人知道，但很方便的功能是相同的应用程序名称，可以使用跨多个应用程序服务环境。  因为每个应用程序服务环境都有唯一的域名后缀，开发人员可以选择重新在每个环境中使用完全相同的应用程序名称。  例如开发人员可以将应用程序名，如下所示为︰ *myapp.foo1.p.azurewebsites.net*、 *myapp.foo2.p.azurewebsites.net*、 *myapp.foo3.p.azurewebsites.net*等。 为示例应用程序虽然每个应用程序实例还具有一个唯一的名称。  所使用的应用程序实例名称是*webfrontend1*、 *webfrontend2*和*webfrontend3*。


## <a name="setting-up-the-traffic-manager-profile"></a>设置通信管理器配置文件 ##
后在多个应用程序服务环境上部署应用程序的多个实例，使用通信管理器可以注册单个应用程序实例。  示例应用程序通信管理器配置文件用于*可扩展 ase demo.trafficmanager.net*可发送到任何下列已部署的应用程序实例的客户︰

- **webfrontend1.fe1ase.p.azurewebsites.net:** 在第一个应用程序服务环境上部署示例应用程序的实例。
- **webfrontend2.fe2ase.p.azurewebsites.net:** 在第二个应用程序服务环境上部署示例应用程序的实例。
- **webfrontend3.fe3ase.p.azurewebsites.net:** 在第三个应用程序服务环境上部署示例应用程序的实例。

注册多个 Azure 应用程序服务终结点，所有运行在**同一个**Azure 的地区，最简单的方法是使用[Azure 资源管理器通信管理器支持]Powershell[ARMTrafficManager]。  

第一步是创建一个 Azure 流量管理器配置文件。  下面的代码演示如何配置文件创建的示例应用程序︰

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

请注意如何将*RelativeDnsName*参数设置为*可伸缩 ase 演示*。  这是如何创建和流量管理器配置文件的域的名称*可扩展 ase demo.trafficmanager.net* 。

*TrafficRoutingMethod*参数定义的负载平衡的策略的通信管理器将使用来确定如何跨所有可用的端点客户负载。  在此示例中*加权*的选择方法。  这将导致客户请求被分配到所有基于与每个终结点关联的相对权重的已注册应用程序终结点。 

与创建的配置文件，每个应用程序实例将作为本机 Azure 的终结点添加到配置文件。  下面的代码读取每个前端 web 应用程序中，参考，然后将每个应用程序添加为一个流量管理器终结点通过*TargetResourceId*参数。


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
请注意如何没有一个*添加 AzureTrafficManagerEndpointConfig*为每个单独的应用程序实例的调用。  每个 Powershell 命令中的*TargetResourceId*参数引用了三个已部署的应用程序实例之一。  流量管理器配置文件将在配置文件中注册的所有三个终结点之间分布负载。

所有三个终结点的*权重*参数使用相同的值 (10)。  这会导致流量管理器分配的客户请求跨所有三个应用程序实例相对平均。 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>指向应用程序的自定义的域流量管理器的域 ##
所需的最后一步是指向自定义应用程序流量管理器的域的域。  对于示例应用程序，这意味着指向*可扩展 ase demo.trafficmanager.net* *www.scalableasedemo.com* 。  这一步需要完成的管理自定义的域的域注册。  

使用注册器的域管理工具，一个 CNAME 记录需要创建该点处的流量管理器的域自定义的域。  下图显示了此 CNAME 配置如下所示的示例︰

![自定义域的 CNAME][CNAMEforCustomDomain] 

虽然未涉及本主题中，请记住每个单独的应用程序实例需要进行自定义的域以及注册它。  否则如果请求使其对应用程序实例，并且应用程序不具有自定义注册的应用程序域，则请求将失败。  

在此示例中的自定义的域是*www.scalableasedemo.com*，并且每个应用程序实例都有与其相关联的自定义的域。

![自定义的域][CustomDomain] 

概述的 Azure 应用程序服务的应用程序注册自定义的域，请参阅以下文章上[注册自定义的域][RegisterCustomDomain]。

## <a name="trying-out-the-distributed-topology"></a>尝试出分布式拓扑 ##
通信管理器和 DNS 配置的最终结果是*www.scalableasedemo.com*的请求将通过按以下顺序︰

1. 浏览器或设备将对*www.scalableasedemo.com*执行 DNS 查找
2. 在域注册机的 CNAME 条目将导致 DNS 查找重定向到 Azure 流量管理器。
3. DNS 查找以供*可伸缩 ase demo.trafficmanager.net*根据一个 Azure 流量管理器的 DNS 服务器。
4. 根据负载平衡策略 （使用前面创建的通信流管理器配置文件时的*TrafficRoutingMethod*参数），流量管理器将选择一个配置终结点，并将该终结点的 FQDN 返回到浏览器或设备。
5.  由于该终结点的 FQDN 是在应用程序服务环境上运行的应用程序实例的 Url，浏览器或设备需要 Microsoft Azure DNS 服务器来解析为 IP 地址的 FQDN。 
6. 浏览器或设备会向 HTTP/S 请求的 IP 地址。  
7. 该请求将到达一个应用程序服务环境上运行的应用程序实例之一。

下面的控制台图片显示成功解析为一个三个示例应用程序服务环境 （在此情况下第二个三个的应用程序服务环境的） 上运行的应用程序实例的示例应用程序的自定义域 DNS 查找︰

![DNS 查找][DNSLookup] 

## <a name="additional-links-and-information"></a>附加的链接和信息 ##
所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

在[Azure 资源管理器通信管理器支持]Powershell 的文档[ARMTrafficManager]。  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
