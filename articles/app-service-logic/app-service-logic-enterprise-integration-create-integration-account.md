<properties 
    pageTitle="集成帐户和企业集成包概述 |Microsoft Azure 应用程序服务 |Microsoft Azure" 
    description="了解所有关于集成帐户、 企业集成包和逻辑的应用程序" 
    services="logic-apps" 
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

# <a name="overview-of-integration-accounts"></a>集成科目的概览

## <a name="what-is-an-integration-account"></a>什么是集成帐户？
集成帐户是 Azure 帐户，允许企业集成的应用程序来管理项目，包括架构、 映射、 证书、 合作伙伴和协议。 您创建的任何集成应用程序需要使用一个集成的帐户才能访问架构、 映射或证书，例如。

## <a name="create-an-integration-account"></a>创建集成帐户 
1. 选择**浏览**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. 筛选器在搜索框中输入**集成**并从结果列表中选择**集成帐户**     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. 从页面顶部的菜单中选择*添加*按钮      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. 输入**名称**，请选择您想要使用，**订阅**或者创建新的**资源组**或选择现有资源组，请选择**位置**集成帐户将承载，选择**定价层**，然后选择**创建**按钮。   

  此时将在所选的位置提供集成帐户。 这应该在 1 分钟内完成。    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. 刷新页面。 您将看到您列出的新集成帐户。 祝贺您 ！  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>如何将集成帐户链接到应用程序逻辑
为了让您的逻辑应用程序访问映射、 架构、 协议和其他位于集成帐户中的项目，必须首先将集成帐户链接到逻辑应用程序。

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>下面是链接到一个逻辑应用程序的集成帐户的步骤 

#### <a name="prerequisites"></a>系统必备组件
- 集成帐户
- 逻辑应用程序

>[AZURE.NOTE]请确保您的集成帐户和应用程序逻辑中**Azure 的同一位置**开始之前

1. 从逻辑的应用程序的菜单中选择**设置**链接  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. 从设置刀片式服务器选择**集成帐户**项  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. 选择您想要链接到您的逻辑应用程序**选择集成帐户**从下拉列表框中的集成帐户  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. 保存您的工作  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. 您将看到指示集成帐户已被链接到您的逻辑应用程序和集成帐户中的所有项目现都已提供给您的逻辑应用程序的通知。  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

现在，集成帐户链接到您的逻辑应用程序时，您可以转到您的逻辑应用程序并使用 B2B 接口，例如︰ XML 验证、 平面文件编码/解码或转换使用 B2B 特征创建的应用程序。  
    
## <a name="how-to-delete-an-integration-account"></a>如何删除集成帐户？
1. 选择**浏览**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. 筛选器在搜索框中输入**集成**并从结果列表中选择**集成帐户**     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. 选择要删除的**集成帐户**  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. 选择菜单上的**删除**链接   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. 确认您的选择    

## <a name="how-to-move-an-integration-account"></a>如何移动集成帐户？
新的订阅和新的资源组，可以轻松移动集成帐户。 如果您需要移动集成帐户，请执行以下步骤︰

>[AZURE.IMPORTANT] 您将需要更新所有脚本之后移动集成帐户，请使用新的资源 Id。

1. 选择**浏览**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. 筛选器在搜索框中输入**集成**并从结果列表中选择**集成帐户**     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. 选择要删除的**集成帐户**  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. 选择菜单上的**移动**链接   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. 确认您的选择    

## <a name="next-steps"></a>下一步行动
- [了解更多有关协议](./app-service-logic-enterprise-integration-agreements.md "了解企业集成协议")  


 