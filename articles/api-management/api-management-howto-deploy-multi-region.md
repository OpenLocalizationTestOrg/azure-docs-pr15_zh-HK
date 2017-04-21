<properties
    pageTitle="如何将 Azure API 管理服务实例部署到 Azure 的多个区域"
    description="了解如何将 Azure API 管理服务实例部署到 Azure 的多个区域。" 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>如何将 Azure API 管理服务实例部署到 Azure 的多个区域

API 管理支持多区域部署方案，它可以在任意数量的所需的 Azure 区域分布的单一的 API 管理服务的 API 发布者。 这有助于减小请求延迟认定的地理位置分散的 API 的使用者，如果一个地区离线也可以提高服务的可用性。 

最初创建 API 管理服务时，它包含一个[单元][]并驻留在单个的 Azure 区域，被指定为主要区域。 通过 Azure 传统门户网站，可轻松添加更多的地区。 API 管理网关服务器部署到每个地区和调用通信将被路由至最接近的网关。 如果某个地区离线，流量是自动重定向到最接近的下一个网关。 

> [AZURE.IMPORTANT] 多区域部署才**[特优][]**级中可用。

## <a name="add-region"></a>将 API 管理服务实例部署到新的地区

若要开始，请单击 Azure 经典门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

导航到 API 管理服务实例 Azure 经典门户中的**缩放**选项卡。 

![缩放比例选项卡][api-management-scale-service]

若要部署到新的区域，主区域下面的下拉列表上单击，选择列表中的区域。

![添加区域][api-management-add-region]

一旦选中该区域，则选择新区域单位的下拉列表中的单位数。

![指定单位][api-management-select-units]

配置所需的区域和单位后，单击**保存**。

## <a name="remove-region"></a>从区域中删除 API 管理服务实例

要从区域中删除 API 管理服务实例，请导航到 API 管理服务实例 Azure 经典门户中的**缩放**选项卡。 

![缩放比例选项卡][api-management-scale-service]

单击右侧的所需区域移去的**X** 。  

![删除区域][api-management-remove-region]

一旦所需的区域将被删除，单击**保存**。


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[Azure API 管理入门]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[单位]: http://azure.microsoft.com/pricing/details/api-management/
[高级]: http://azure.microsoft.com/pricing/details/api-management/

