<properties
 pageTitle="逻辑应用程序模板 |Microsoft Azure"
 description="了解如何使用预先创建的逻辑应用程序模板帮助您入门"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>逻辑应用程序模板

## <a name="what-are-logic-app-templates"></a>什么是逻辑应用程序模板

逻辑应用程序模板是一个预构建的逻辑应用程序可用来快速开始创建您自己的工作流。 

这些模板是一个好的方法来发现可以使用逻辑应用程序生成的各种模式。 您可以使用这些模板-或者修改以符合您的情况。

## <a name="overview-of-available-templates"></a>可用的模板概述

有许多可用的模板当前发布逻辑应用程序平台中。 下面列出了一些示例类别，以及，使用的连接器类型。

### <a name="enterprise-cloud-templates"></a>企业云模板
模板集成 Dynamics CRM、 销售队伍、 框、 Azure Blob 和为您的企业的其他连接器的云的需求。 一些示例的可以做什么与这些模板包括组织潜在顾客和公司文件数据执行备份。

### <a name="enterprise-integration-pack-templates"></a>企业级集成包模板
VETER 配置 （验证、 提取、 转换、 丰富、 路由） 接收 EDI 文档通过 AS2 并将其转换为 XML，以及为 X12 和 AS2 消息处理 X12 的管线。

### <a name="protocol-pattern-templates"></a>协议模式模板
这些模板包含逻辑应用程序跨 FTP 和 SFTP 通过 HTTP 集成包含如请求-响应协议模式。 使用这些因为它们存在，或作为基础来创建更复杂的协议模式。  

### <a name="personal-productivity-templates"></a>个人工作效率模板
模式，以帮助提高个人生产力包括设置每日提醒到待办事项列表中，打开重要工作项以及如何自动执行长时间任务到单个用户审核步骤的模板。

### <a name="consumer-cloud-templates"></a>使用者云模板
社交媒体服务，如 Twitter、 可宽延时间和电子邮件，最终能够加强社会媒体营销战略相结合的简单模板。 其中还包括如多云将复制的模板，这有助于提高工作效率，通过保存传统重复性任务上花费的时间。 

## <a name="how-to-create-a-logic-app-using-a-template"></a>如何创建使用模板逻辑应用程序 

若要开始使用逻辑应用程序模板，转到逻辑应用程序设计器中。 如果要输入，设计器将打开一个现有的逻辑应用程序逻辑的应用程序将自动加载设计器视图中。 但是，如果您正在创建新的逻辑应用程序，您将看到下面的屏幕。  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

从该屏幕中，您可以选择启动具有空白逻辑应用程序或一个预构建的模板。 如果您选择一种模板，将提供其他信息。 在此示例中，我们使用*在收存箱，创建一个新文件时复制到 OneDrive*模板。  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

如果您选择要使用的模板，只需选择*使用此模板*按钮。 将要求您登录到您的帐户基于模板使用的接头上。 或者，如果您以前已经建立与这些连接器的连接，您可以选择继续如下所示。  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

建立连接并选择后*继续*，逻辑应用程序设计器视图中打开。  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

在上面的示例中，使用多个模板，情况一样某些强制属性字段可能填写内接头;然而，一些可能仍然能够正确地部署逻辑应用程序之前需要一个值。 如果您尝试部署而无需输入一些缺少的字段，则会通知您并显示错误消息。

如果您想要返回到模板查看器，在顶部导航栏中选择*模板*按钮。 通过切换回模板查看器，则会丢失任何未保存的进度。 在切换回模板查看器，您将看到一条警告消息，通知您此。  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>如何将从模板中创建一个逻辑应用程序部署

一旦加载您的模板并进行任何所需的更改，选择保存按钮的左上角。 这将保存并发布您的逻辑的应用程序。  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

如果您想更多的信息如何添加到现有的逻辑应用程序模板，更多的步骤，或使一般情况下，编辑阅读的详细信息在[创建逻辑应用程序](app-service-logic-create-a-logic-app.md)。