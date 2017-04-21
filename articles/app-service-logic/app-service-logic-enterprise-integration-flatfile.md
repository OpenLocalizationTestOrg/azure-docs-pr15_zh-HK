<properties
    pageTitle="了解如何进行编码或解码的企业集成包和逻辑的应用程序使用的平面文件 |Microsoft Azure 应用程序服务 |Microsoft Azure"
    description="使用企业集成包和逻辑的应用程序的功能进行编码或解码的平面文件"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-flat-files"></a>平面文件与企业集成

## <a name="overview"></a>概述

您可能想要编码的 XML 内容之前将其发送给业务合作伙伴在企业对企业 (B2B) 方案。 在 Azure 应用程序服务的应用程序的逻辑功能进行逻辑应用程序中，可以使用编码连接器的平面文件来执行此操作。 您创建的应用程序逻辑可以获得其 XML 内容从各种来源，包括从 HTTP 请求触发、 另一个应用程序，或甚至是从多个[连接器](../connectors/apis-list.md)之一。 逻辑应用程序有关的详细信息，请参阅的[逻辑应用程序文档](./app-service-logic-what-are-logic-apps.md "了解更多关于逻辑的应用程序")。  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>如何创建平面文件编码连接器

请按照以下步骤添加平面文件编码于逻辑应用程序连接器。

1. 创建逻辑应用程序并[将其链接到集成帐户](./app-service-logic-enterprise-integration-accounts.md "了解链接到逻辑应用程序集成的帐户")。 此帐户包含将用于编码的 XML 数据的架构。  
2. 添加逻辑应用程序**收到请求-当 HTTP 请求**触发。  
![触发器的屏幕截图来选择](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. 添加的平面文件编码操作，如下所示︰

    一。 选择**加上**签名。

    b。 选择**将操作添加**链接 （显示选择加号后）。

    c。 在搜索框中，输入*平板*筛选到您想要使用的所有操作。

    d。 从列表中选择**平面文件编码**选项。   
![屏幕抓图的平面文件编码选项](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. 在**平面文件编码**对话框中，选择**内容**文本框。  
![屏幕抓图的内容的文本框](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. 选择正文标记作为您要编码的内容。 Body 标签将填充内容的字段。     
![Body 标签的屏幕抓图](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. 选择**架构名称**列表框，然后选择您要用于编码的输入的内容的架构。    
![屏幕抓图的架构名称列表框](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. 保存您的工作。   
![屏幕快照的保存图标](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

此时，您已完成设置平面文件编码连接器。 在实际应用中，可能需要存储的已编码的数据的业务线应用程序，如销售队伍。 或者您可以发送编码的数据与贸易合作。 您可以轻松添加操作可以通过使用任何一种提供的其他连接器将编码操作的输出发送到销售队伍，或您的贸易伙伴。

现在可以通过 HTTP 端点，向发出请求并请求的正文中包括 XML 内容来测试您的连接器。  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>如何创建平面文件解码连接器

>[AZURE.NOTE] 要完成这些步骤，您需要有一个架构文件已上载到集成帐户。

1. 添加逻辑应用程序**收到请求-当 HTTP 请求**触发。  
![触发器的屏幕截图来选择](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. 添加的平面文件进行解码操作，如下所示︰

    一。 选择**加上**签名。

    b。 选择**将操作添加**链接 （显示选择加号后）。

    c。 在搜索框中，输入*平板*筛选到您想要使用的所有操作。

    d。 从列表中选择**平面文件解码**选项。   
![屏幕抓图的平面文件解码选项](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- 选择**内容**控件。 这将产生可以使用的内容进行解码的早期步骤中的内容的列表。 请注意，从传入的 HTTP 请求的*正文*可用作内容进行解码。 您还可以输入要解码直接插入**内容**控件的内容。     
- 选择*正文*标记。 注意正文标记现在在**内容**控件中。
- 选择您要用于解码内容的架构的名称。 下面的屏幕快照显示*OrderFile*是选定的架构名称。 此架构名称有以前上载到集成帐户。

 ![屏幕抓图的平面文件解码对话框](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- 保存您的工作。  
![屏幕快照的保存图标](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

此时，您已完成设置平面文件解码连接器。 在实际应用中，可能需要在业务线应用程序，如队伍中存储已解码的数据。 您可以轻松添加操作将解码操作的输出发送到销售队伍。

现在可以通过 HTTP 端点向发出请求，包括您要请求的正文中解码 XML 内容来测试您的连接器。  

## <a name="next-steps"></a>下一步行动
- [了解更多有关企业集成包](./app-service-logic-enterprise-integration-overview.md "了解企业集成包")。  
