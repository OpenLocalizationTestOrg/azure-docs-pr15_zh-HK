<properties
    pageTitle="Azure 应用程序服务︰ 缩放应用程序服务应用程序"
    description="了解应用程序服务以提升应用程序的细节。"
    keywords="应用程序服务，azure 应用程序服务，比例，可伸缩的应用程序服务计划应用程序服务成本"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure 应用程序服务︰ 缩放应用程序服务应用程序

在 Azure 应用程序服务中承载的应用程序可以[获得巨大的规模](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/)。
但是，扩展应用程序是一个复杂的问题，并没有"一刀切"的解决方案。 正确地扩展到应用程序有 3 个重要领域，将导致您的应用程序的成功︰

1. 了解您的应用程序体系结构和其弱点。
    * 将应用程序状态吗？ 无状态吗？
    * 什么是您的应用程序的所有组件？
        * 在应用程序中的瓶颈在哪里？
    * 为您的应用程序负载时，什么会破坏第一？
2. 了解预期的负载和性能需求。
    * 应用程序是否需要为一千用户提供服务？还是 100 万？
    * 将通信来自一个地理位置或全局范围内？
    * 是否有季节性的变化？通信高峰期？
    * 应用程序应如何快速回应？ 1 秒吗？ 1 毫秒吗？
3. 了解并正确利用这一平台承载您的应用程序。
    * 我应该利用哪些功能来实现我的比例目标呢？

本部分将帮助您了解所有的因素和帮助制订一种战略，将利用必要的应用程序服务功能，以实现可伸缩性目标。

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
