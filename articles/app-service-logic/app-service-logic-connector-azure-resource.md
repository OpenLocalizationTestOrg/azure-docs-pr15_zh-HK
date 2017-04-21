<properties
   pageTitle="在应用程序逻辑中使用 Azure 资源连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置 Azure 资源接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>开始使用 Azure 资源连接器并将其添加到您的逻辑应用程序
>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2014年-12-01-预览架构版本。

使用 Azure 资源接口可以方便地在您的逻辑应用程序内部管理 Azure 的资源。

## <a name="create-the-azure-resource-connector"></a>创建 Azure 资源连接器
若要使用 Azure 资源连接器 API 的应用程序，您需要先创建它的实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择 Azure 资源管理器接口 API 的应用程序。

要配置此设置，您必须您需要设置了使用权限来执行任何操作要在 Azure 服务主体。 所有调用将您设置该服务主体都进行上代表。 这使您可以确定范围连接器使用只需要它执行的操作，而不安装其他。

David Ebbo 已编写[好的博客文章](http://blog.davidebbo.com/2014/12/azure-service-principal.html)如何对此进行设置。 请按照所有存在，并获得您的**租户 ID**、**客户端 ID**和**密码**。 这三个字段，再加上**订阅 ID**，都有何要求配置连接器。

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>逻辑应用程序设计器中使用 Azure 资源连接器
### <a name="trigger"></a>触发器
有两个在连接器中受支持的触发器︰

名称 | 说明
---- | -----------
事件发生 | 您的订阅中发生事件时触发。
跃点数超过阈值 |  当指标满足某个特定阈值时触发。

### <a name="action"></a>操作

同样，您可以在 Azure 订阅提供大量的操作︰

**资源**组可以︰

名称 | 说明
---- | -----------
清单资源组 | 列出所有订阅中的资源组。
获取资源组 | 由其 id 获取一个资源组。
创建资源组 | 创建或更新某一资源组。
删除资源组 | 删除某一资源组。

对于**资源**，您可以︰

名称 | 说明
---- | -----------
列表中的资源 | 按不同类型的筛选器列表在您的订阅中的资源。
获取资源 | 获取单个资源由其资源 id。
创建或更新资源 | 创建资源时，或者，更新现有资源。 您必须对该资源提供的所有属性。
资源操作 |  在资源上执行任何其他操作。 您需要知道的操作名称和 （如果有的话），此操作所需的负载。
删除资源 | 删除资源。

**资源**提供程序，您可以︰

名称 | 说明
---- | -----------
列表中的资源提供程序 | 列出所有可用的资源提供程序订阅中。

有关**资源组的部署**，您可以︰

名称 | 说明
---- | -----------
列表中的部署 | 列出所有资源组中部署。
获取部署 | 通过其 id 来获取模板部署。
创建部署 | 通过提供的模板来创建新的资源组部署。

有关资源的**事件**，您可以︰

名称 | 说明
---- | -----------
获取事件 | 在订阅或资源来获取事件。

有关资源的**度量标准**，您可以︰

名称 | 说明
---- | -----------
获取标准 | 一个指标获得的资源 id。

## <a name="do-more-with-your-connector"></a>用做更多您的连接器
创建连接器时，可以将其添加到使用逻辑应用程序的业务流程中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

>[AZURE.NOTE] 如果您想要开始使用 Azure 逻辑应用程序在 Azure 帐户注册之前，转到[重试逻辑的应用程序](https://tryappservice.azure.com/?appservice=logic)，立即可以在此创建短期的初学者的逻辑应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

查看在 Swagger REST API 参考[连接器和 API 的应用程序引用](http://go.microsoft.com/fwlink/p/?LinkId=529766)。

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
