<properties
    pageTitle="将 Web 应用程序资源移动到另一个资源组"
    description="介绍了在其中您可以移动 Web 应用程序和应用程序服务从一个资源组到另一个方案。"
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>支持的移动配置

您可以移动使用[ARM 移动资源 Api](../resource-group-move-resources.md)的 Azure Web 应用程序资源。

Azure 的 Web 应用程序当前支持以下移动方案︰

* 将资源组 （web 应用程序、 应用程序服务计划和证书） 的全部内容移动到另一个资源组 
    * 注意︰ 目标资源组不能包含在该方案中的任何 Microsoft.Web 资源
* 移动单个 web 应用程序到不同的资源组中，同时仍然作为其宿主在其当前的应用程序服务计划 （应用程序服务计划停留在旧的资源组）
