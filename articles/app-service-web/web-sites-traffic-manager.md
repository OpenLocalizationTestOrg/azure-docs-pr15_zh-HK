<properties
    pageTitle="控制 Azure 的 web 应用程序通信使用 Azure 通信管理器"
    description="因为它与 Azure 的 web 应用程序，本文提供了摘要信息的 Azure 流量管理器。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>控制 Azure 的 web 应用程序通信使用 Azure 通信管理器

> [AZURE.NOTE] 因为它与 Azure 应用程序服务 Web 应用程序，本文提供了用于 Microsoft Azure 流量管理器中的摘要信息。 可以通过访问在本文末尾的链接找到有关 Azure 流量管理器本身的详细信息。

## <a name="introduction"></a>介绍
Azure 流量管理器可用于控制如何将 web 客户端的请求分配给在 Azure 应用程序服务 web 应用程序。 Web 应用程序终结点添加到 Azure 流量管理器配置文件后，Azure 流量管理器跟踪的 web 应用程序 （运行、 已停止或已删除） 的状态，以便它可以决定哪种这些终结点应接收通信。

## <a name="load-balancing-methods"></a>负载平衡方法
Azure 的流量管理器使用三种不同的负载平衡方法。 因为它们与 Azure 的 web 应用程序在下面的列表描述。

* **故障转移**︰ 如果 web 应用程序克隆在不同地区，可以使用此方法来配置一个 web 应用程序来服务所有 web 客户端的通信，并在不同区域，以使第一个 web 应用程序变得不可用的情况下提供都服务的通信量配置另一个 web 应用程序。

* **循环**︰ 如果 web 应用程序克隆在不同地区，可以使用此方法来流量平均分散在不同区域中的 web 应用程序。

* **性能**︰ 性能方法分发基于最短的往返行程时间对客户端的通信。 性能方法可以用于 web 应用程序中的同一个区域内或在不同的地区。

##<a name="web-apps-and-traffic-manager-profiles"></a>Web 应用程序和通信管理器配置文件
要配置 web 应用程序通信的控制，Azure 流量管理器中，使用一个三种负载平衡方法前面所述，创建一个配置文件，并添加您要控制对配置文件的通信的终结点 （在此情况下，web 应用程序）。 您的 web 应用程序状态 （正在运行、 已停止或删除） 定期传递到配置文件以便 Azure 流量管理器可以相应地直接通信。

当使用 Azure Azure 流量管理器，请记住以下几点︰

* 对于在同一区域内的 web 应用程序只部署，故障转移和循环功能，而无需考虑 web 应用程序模式已经提供了 Web 应用程序。

* 对于在同一个地区与另一个 Azure 的云服务一起使用 Web 应用程序的部署，您可以组合两种类型的终结点以使混合方案。

* 在配置文件中只能指定一个新的 web 应用程序终结点可每个地区。 当您选择作为一个区域的终结点的 web 应用程序时，在该地区剩下的 web 应用程序变得不可用选择为该配置文件。

* 在 Azure 流量管理器配置文件中指定的 web 应用程序终结点将出现在 web 应用程序的配置文件中配置页上的**域名**部分下，但不是会有可配置。

* 配置文件中添加 web 应用程序后，如果设置了一个门户页面的 web 应用程序的仪表板上的**网站 URL**将显示使 web 应用程序的自定义的域 URL。 否则，它将显示流量管理器配置文件 URL (例如， `contoso.trafficmgr.com`)。 使 web 应用程序和通信管理器 URL 直接域名称将显示在 web 应用程序的配置页中的**域名称**部分。

* 您的自定义域名能像预期的那样，但在将其添加到 web 应用程序，还必须配置您的 DNS 映射来指向流量管理器的 URL。 有关如何设置一个自定义 Azure 的 web 应用程序域的信息，请参阅[配置 Azure 的网站自定义域名](web-sites-custom-domain-name.md)。

* 您只能添加到 Azure 流量管理器配置文件中的标准模式中的 web 应用程序。

## <a name="next-steps"></a>下一步行动

概念和技术的 Azure 通讯管理器概述，请参阅[通信量管理器概述](../traffic-manager/traffic-manager-overview.md)。

关于使用的 Web 应用程序的流量管理器的详细信息，请参阅博客文章[使用 Azure 流量管理器与 Azure 网站](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx)和[Azure 流量管理器现在可以集成与 Azure 网站](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/)。
