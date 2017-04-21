<properties
    pageTitle="部署到 Azure 应用程序服务的应用程序"
    description="了解如何部署应用程序到应用程序的服务工作"
    keywords="部署，部署的应用程序服务，azure 应用程序服务"
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="dariagrigoriu"/>

# <a name="azure-app-service-deployment-overview"></a>Azure 应用程序服务部署概述

Azure 应用程序服务提供了丰富而集成的功能集来支持创建功能强大且灵活的部署工作流。 应用程序部署可以利用选项，其中包括持续集成或本地源代码发布，WebDeploy 和 FTP。 推荐的生产应用程序部署的方法是部署插槽交换。 部署插槽表示与生产应用程序的临时和集成环境。 可以配置和验证，web 流量的目标部署插槽和流量可以按需部署到生产不需要停机换和自动预热。 部署工作流的步骤可以轻松地自动通过发行管理产品，如 Visual Studio 的版本管理。 这可用于协调与其他解决方案资源 （如数据存储） 定期和复制跨多个部门的部署。 

[AZURE.INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]
