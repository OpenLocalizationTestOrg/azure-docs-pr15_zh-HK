<properties 
   pageTitle="在 Azure 应用程序服务应用程序逻辑中的 B2B 邮件跟踪 |Microsoft Azure" 
   description="本主题介绍了 B2B 处理的跟踪" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="track-b2b-messages"></a>B2B 邮件跟踪

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

## <a name="b2b-tracking-information"></a>B2B 的跟踪信息
B2B 通信涉及贸易合作伙伴之间所处理的消息。 关系被指两个贸易伙伴之间的协议。 建立通信后，需要有一种监视如果像预期的那样发生通信的方法。 

我们已实施以下的 B2B 方案的邮件跟踪︰ AS2、 EDIFACT、 和 X12。

## <a name="as2"></a>AS2
一旦创建 AS2 API 的应用程序的一个实例，该实例中，浏览，并选择**跟踪**。 此处可以查看和筛选所有 AS2 跟踪信息︰  

![][1]  

## <a name="edifact"></a>EDIFACT
一旦创建 EDIFACT API 的应用程序的一个实例，该实例中，浏览，并选择**跟踪**。 此处可以查看和筛选跟踪信息的所有 EDIFACT。 此外，互换级别、 组级别中，可以查看和事务的单一视图中的所有设置级别的数据。 

如果 EDIFACT 协议相关联的贸易合作伙伴管理 API 应用程序中的一部分创建了批，批处理部分将列出所有这些批次。 您可以选择一批以活动消息 （如果有的话），也已完成的信息，请参阅︰  

![][2]      

## <a name="x12"></a>X12
一旦创建实例 X12 的 API 的应用程序，浏览到该实例，然后选择**跟踪**。 此处可以查看和筛选跟踪信息的所有 X12。 此外，互换级别、 组级别中，可以查看和事务的单一视图中的所有设置级别的数据。

如果创建了批 X12 的一部分协议相关联的贸易合作伙伴管理 API 应用程序中，则批处理部分中列出所有这些批次。 您可以选择一批以活动消息 （如果有的话） 以及所完成的批次的信息，请参阅。

<!--Image references-->
[1]: ./media/app-service-logic-track-b2b-messages/AS2Tracking.png
[2]: ./media/app-service-logic-track-b2b-messages/EDIFACTTracking.png
