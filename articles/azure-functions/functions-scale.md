<properties
   pageTitle="如何缩放 Azure 函数 |Microsoft Azure"
   description="了解如何 Azure 函数扩展以满足您事件驱动的工作负载的需要。"
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="azure 函数、 函数、 事件处理、 webhooks、 动态计算、 无服务器体系结构"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>如何缩放 Azure 的函数

## <a name="introduction"></a>介绍

Azure 函数的一个优点是在需要时才消耗计算资源。 这意味着您不支付为空闲状态的虚拟机或需要预留产能时，您可能需要它。 相反，当代码运行时，可进行扩展以处理负载，然后扩展，代码不运行时平台中分配计算能力。

这一新功能的机制是动态的服务计划。  

本文概述了对动态服务方案的工作原理以及该平台如何按需运行您的代码进行扩展。

如果您还不熟悉 Azure 函数，请务必检查更好地了解它的功能是[Azure 函数概述](functions-overview.md)文章。

## <a name="configure-azure-functions"></a>配置 Azure 的功能

两个主设置与缩放比例︰

* [Azure 应用程序服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)或动态服务计划
* 执行环境的内存大小

所需的函数发生更改，具体取决于您选择的服务计划的成本。 提供动态服务计划，成本是一个函数的执行时间、 内存大小和次数。 费用累计仅当您实际运行代码。

应用程序服务计划承载您对现有的虚拟机，也可用于运行其他代码的函数。 这些虚拟机用于支付每个月后，没有对其执行函数另收费。

## <a name="choose-a-service-plan"></a>选择服务计划

创建函数时，您可以选择运行它们的动态服务计划或[应用程序服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)上。
在应用程序服务计划中，函数将运行在专用的虚拟机，就像今天为基本的 web 应用程序处理、 标准版或高级 Sku。
此专用的虚拟机分配给您的应用程序和功能，始终是可用的是否积极执行代码或不。 这是一个不错的选择，如果您有现有的、 未充分利用的虚拟机已在运行其他代码，或您希望持续或几乎连续运行函数。 虚拟机将分离成本从运行库和内存大小。 因此，您可以限制许多长时间运行函数与一个或多个虚拟机上运行的成本费用。

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
