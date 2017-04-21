<properties 
    pageTitle="API 管理的关键概念" 
    description="了解有关 Api、 产品、 角色、 组和其他 API 管理的关键概念。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>如何导入的操作在 Azure API 管理 API 定义

在 API 管理中，可以创建新的 Api 和手动添加的操作或 API 可以导入和一步操作。

Api 和它们的操作可以使用导入下列格式。

-   WADL
-   Swagger

本指南演示如何创建一个新的 API 和导入一步的操作。 手动创建 API 和添加操作的信息，请参阅[如何创建 Api][] ，以及[如何添加操作的 api][]。

## <a name="import-api"> </a>API 导入

创建和配置发布服务器门户中的 Api。 若要访问发布者门户，单击**管理**API 管理服务的 Azure 传统门户。 如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

![出版商门户][api-management-management-console]

单击在左侧， **API 管理**菜单中的**Api** ，然后单击**导入 API**。

![导入 API][api-management-import-apis]

**导入 API**窗口有三个选项卡对应的三种方式提供的 API 规范。

-   **从剪贴板**允许您在指定的文本框中粘贴 API 规范。
-   **从文件**允许您浏览到并选择包含 API 规范的文件。
-   **从 URL**可以提供 API 的说明的 URL。

![API 导入格式][api-management-import-api-clipboard]

提供 API 规范后, 使用右侧的单选按钮来指示的规范格式。 支持以下格式。

-   WADL
-   Swagger

接下来，输入**Web API URL 后缀**。 这被追加到您的 API 管理服务的基 URL。 基 URL 是公共的 API 管理服务的每个实例上承载的所有 Api。 API 管理由其后缀区分 Api，因此后缀必须是唯一的在特定的 API 管理服务实例中每个 API。

所有值都输入后，单击**保存**以创建 API 和关联的操作。 

>[AZURE.NOTE] 对于导入基本计算器 API 以 Swagger 格式的教程，请参阅[管理 Azure API 管理在您第一组 API](api-management-get-started.md)。

## <a name="export-api"></a>导出 API

除了导入新的 Api，您可以导出您的 Api 定义发布门户。 若要执行此操作，请单击**导出的 API**的**摘要选项卡**您的**API**。

![导出 API][api-management-export-api]

可以使用 WADL 或 Swagger 导出的 Api。 选择所需的格式，单击**保存**并选择要在其中保存文件的位置。

![导出的 API 的格式][api-management-export-api-format]

## <a name="next-steps"></a>下一步行动

一旦 API 创建和导入的操作，您可以查看和配置任何其他设置，API 添加一种产品，并予以发布，这样就可供开发人员。 有关详细信息，请参阅以下指南。

-   [如何配置 API 设置][]
-   [如何创建和发布产品][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[如何将操作添加到 API]: api-management-howto-add-operations.md
[如何创建和发布产品]: api-management-howto-add-products.md
[如何创建 Api]: api-management-howto-create-apis.md
[如何配置 API 设置]: api-management-howto-create-apis.md#configure-api-settings
