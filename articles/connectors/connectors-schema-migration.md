<properties
    pageTitle="如何将逻辑应用程序迁移到架构版本 2015年-08-01-预览 |Microsoft Azure 应用程序服务"
    description="轻松地可以将逻辑应用程序迁移到新的架构版本。 您只需执行下列步骤。"
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>如何将逻辑应用程序迁移到架构版本 2015年-08-01-预览

若要将现有逻辑应用程序移动到新架构中，执行下列操作︰  
1. 在 Azure 门户中打开您的逻辑应用程序  
2. 单击更新架构︰

 ![API 图标][step1]   
显示 [更新模式] 页面，并提供到新架构的改进提供详细信息的文档的链接︰ ![API 图标][step2]

>[AZURE.NOTE] 选择**更新架构**时，我们自动运行迁移步骤，并为您提供代码输出。 可以使用此更新您的定义，但是，确保遵循良好的编码制度，如下面的**最佳做法**一节所述。

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>将逻辑应用程序迁移到新的架构版本时的最佳做法︰  

- 将迁移的脚本复制到新的逻辑应用程序-不会覆盖的旧一已经完成测试并确认迁移应用程序之前就能如期作用。
- 您的逻辑应用程序**之前**使在生产环境中测试
- 迁移完成之后，开始更新您的逻辑应用程序可以使用[托管的 Api](./apis-list.md) ，在可能的情况。 例如，您可以开始使用收存箱 v2，whereever 使用的收存箱 v1。


## <a name="whats-next"></a>接下来是什么
-  [了解如何手动迁移您的逻辑应用程序](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






