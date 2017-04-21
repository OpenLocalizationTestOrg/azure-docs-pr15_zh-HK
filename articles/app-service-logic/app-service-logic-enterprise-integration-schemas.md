<properties
    pageTitle="架构和企业集成包概述 |Microsoft Azure"
    description="了解如何使用与企业集成包和逻辑的应用程序架构"
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
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>了解架构和企业集成包  

## <a name="why-use-a-schema"></a>为什么使用架构？
使用架构来确认您收到的 XML 文档是有效的具有预定义的格式中的预期数据。 使用架构来验证在 B2B 方案中交换的消息。

## <a name="add-a-schema"></a>添加架构
从 Azure 门户︰  

1. 选择**更多的服务**。  
![屏幕截图的 Azure 门户网站，突出显示"其他服务"](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. 筛选器搜索框中，输入**集成**，并从结果列表中选择**集成帐户**。     
![筛选器搜索框中的屏幕快照](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. 选择您要向其中添加架构的**集成帐户**。    
![集成的帐户列表中的屏幕快照](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. 选择**架构**麻将牌。  
![屏幕截图的 IEP 集成帐户，使用"架构"突出显示](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>添加架构文件小于 2 MB  

1. 在 （从前面的步骤中） 中打开**架构**刀片式服务器，选择**添加**。  
![屏幕抓图的架构刀片式服务器，使用"添加"按钮突出显示](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. 输入您的架构的名称。 然后，若要上载的架构文件，请选择**架构**文本框旁边的文件夹图标。 上载过程完成后，请选择**确定**。    
!["添加架构"，与"小文件"突出显示的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>添加架构文件大小超过 2MB （最多 8 MB）  

这一过程取决于 blob 容器访问级别︰**公共**或**禁止匿名访问**。 要确定此访问级别，在**Azure 存储浏览器**里下**Blob 容器**，选择所需的 blob 容器。 选择**安全**，然后选择**访问级别**选项卡。

1. 如果**公钥**blob 的安全访问级别，请执行以下步骤。  
  ![屏幕截图的 Azure 存储资源管理器中，用"Blob 容器"、"安全"和"公用"突出显示](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    一。 将架构上传到存储，并复制该 URI。  
    ![具有突出显示的 URI 存储帐户的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b。 在**添加架构**中，选择**较大的文件**，并提供**内容的 URI**文本框中的 URI。  
    ![架构，使用"添加"按钮和"大型文件"突出显示的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. 如果 blob 的安全访问级别是**禁止匿名访问**，请执行以下步骤。  
  ![屏幕截图的 Azure 存储资源管理器中，用"Blob 容器"、"安全"和"没有匿名访问"突出显示](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    一。 上载到的存储架构。  
    ![存储帐户的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b。 生成架构的共享的访问签名。  
    ![存储帐户，使用共享的访问签名选项卡上突出显示的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c。 在**添加架构**中，选择**较大的文件**，并提供共享的访问签名**内容的 URI**文本框中的 URI。  
    ![架构，使用"添加"按钮和"大型文件"突出显示的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. 在 EIP 集成帐户的**架构**刀片式服务器，您现在应该看到新添加的架构。  
![屏幕截图的 EIP 集成帐户，使用"架构"，并突出显示新架构](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>编辑架构
1. 选择**架构**麻将牌。  
2. 选择您要编辑打开刀片式服务器**架构**中的架构。
3. 在**架构**刀片式服务器，选择**编辑**。  
![屏幕抓图的架构刀片式服务器](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. 选择您想要使用打开文件选择器对话框中编辑的架构文件。
5. 在文件选择器中选择**打开**。  
![文件选取器的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. 您会收到通知，指示已成功上载。  

## <a name="delete-schemas"></a>删除架构
1. 选择**架构**麻将牌。  
2. 选择您想要删除打开刀片式服务器**架构**中的架构。  
3. 在**架构**刀片式服务器，选择**删除**。
![屏幕抓图的架构刀片式服务器](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. 若要确认您的选择，请选择**是**。  
!["删除架构"确认消息的屏幕抓图](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. 最后，请注意，刷新**架构**刀片式服务器中的架构的列表，并且不再列出您已删除架构。  
![屏幕截图的 EIP 集成帐户，使用"架构"突出显示](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>下一步行动

- [了解更多有关企业集成包](./app-service-logic-enterprise-integration-overview.md "了解企业集成包")。  
