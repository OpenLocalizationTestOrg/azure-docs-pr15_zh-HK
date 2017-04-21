<properties
    pageTitle="开始使用 API 的应用程序和应用程序服务的 ASP.NET |Microsoft Azure"
    description="了解如何创建、 部署和使用 ASP.NET API 的应用程序在 Azure 应用程序服务，通过使用 Visual Studio 2015年。"
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>开始使用 API 的应用程序，ASP.NET 和 Swagger 在 Azure 应用程序服务

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

这是第一个在一系列的教程介绍了如何使用 Azure 应用程序服务的开发和承载 rest 风格的 Api 非常有用的功能。  本教程中介绍的 API 以 Swagger 格式的元数据的支持。

您将学习︰

* 如何创建和使用内置 Visual Studio 2015年工具部署在 Azure 应用程序服务的[API 的应用程序](app-service-api-apps-why-best-platform.md)。
* 如何使用 Swashbuckle NuGet 程序包来动态生成 Swagger API 元数据自动化 API 发现。
* 如何使用 Swagger API 元数据自动生成 API 使应用程序的客户端代码。

## <a name="sample-application-overview"></a>示例应用程序概述

在本教程中，您使用一个简单的待办事项列表示例应用程序。 应用程序的单页面应用程序 (SPA) 的前端、 ASP.NET Web API 中间层和 ASP.NET Web API 数据层。

![API 的应用程序的示例应用程序关系图](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

下面是[AngularJS](https://angularjs.org/)前端的一个屏幕快照。

![API 的应用程序示例应用程序的待办事项列表](./media/app-service-api-dotnet-get-started/todospa.png)

Visual Studio 解决方案包括三个项目︰

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** -前端︰ 调用中间层 AngularJS SPA。

* **ToDoListAPI** -中层︰ 调用数据层执行 CRUD 操作待办事项上的 ASP.NET Web API 项目。

* **ToDoListDataAPI** -数据层︰ ASP.NET Web API 项目待办事项上的 CRUD 操作。

三层体系结构是一种多个体系结构，可以使用 API 的应用程序实现并在此处仅用于演示目的。 每一层中的代码非常简单，只可能说明 API 的应用程序的功能;例如，数据层使用服务器内存而不是数据库作为其持久性机制。

在学完本教程，您必须设置这两个 Web API 项目并在应用程序服务 API 的应用程序在云中运行。

下一个教程系列中的部署到云的 SPA 的前端。

## <a name="prerequisites"></a>系统必备组件

* ASP.NET Web API 的教程说明假定您有如何使用 Visual Studio 中的 ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)的基本知识。

* Azure 帐户-您可以[免费开设 Azure 帐户](/pricing/free-trial/?WT.mc_id=A261C142F)或[激活 Visual Studio 订阅者权益](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)。

    如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)。 那里，您立即可以应用程序服务中创建短期初学者应用程序 — —**没有信用卡所需**的并没有承诺。

* 如果尚不具备，Visual Studio [.NET 的 Azure SDK](https://azure.microsoft.com/downloads/archive-net-downloads/) -SDK 的 2015年会自动安装 Visual Studio 2015年。

    * 在 Visual Studio 中，单击帮助-> 关于 Microsoft Visual Studio 并确保您具有"Azure 应用程序服务工具 v2.9.1"或更高版本。

    ![Azure 应用程序工具修复](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] 根据 SDK 依赖项的数量，您已经在您的计算机上，安装 SDK 可能需要很长时间，从几分钟到半小时或更长的时间。

## <a name="download-the-sample-application"></a>下载示例应用程序

1. 下载[Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)的存储库。

    可以单击**下载 ZIP**按钮或在您的本地计算机上复制资源库。

2. 在 Visual Studio 2015年或 2013年中打开任务列表解决方案。
   1. 您将需要信任每个解决方案。
        ![安全警告](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. 生成解决方案 （CTRL + SHIFT + B） 还原 NuGet 程序包。

    如果您想要查看操作中的应用程序部署之前，您可以本地运行。 请确保 ToDoListDataAPI 启动项目并运行解决方案。 您应该会看到您的浏览器中的 HTTP 403 错误。

## <a name="use-swagger-api-metadata-and-ui"></a>使用 Swagger API 的元数据和用户界面

对[Swagger](http://swagger.io/) 2.0 元数据 API 支持内置 Azure 应用程序服务。 每个 API 的应用程序可以指定为 Swagger JSON 格式 API 返回的元数据的 URL 终结点。 从该终结点返回的元数据可用于生成客户端代码。

ASP.NET Web API 项目可以动态生成 Swagger 元数据，使用[Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet 程序包。 Swashbuckle NuGet 程序包已安装在您下载的 ToDoListDataAPI 和 ToDoListAPI 项目。

在本教程的这一部分，看生成 Swagger 2.0 元数据，并试验测试基于 Swagger 元数据的 UI 的然后。

1. 将 ToDoListDataAPI 项目 （**** ToDoListAPI 项目） 设置为启动项目。

    ![将 ToDoDataAPI 设置为启动项目](./media/app-service-api-dotnet-get-started/startupproject.png)

2. 按 F5 或单击**调试 > 开始调试**在调试模式下运行该项目。

    浏览器将打开并显示 HTTP 403 错误页。

3. 在您浏览器的地址栏，添加`swagger/docs/v1`到末尾的行，然后按返回。 (URL 为`http://localhost:45914/swagger/docs/v1`。)

    这是 Swashbuckle 用于返回 Swagger 2.0 JSON 元数据 API 的默认 URL。

    如果您正在使用 Internet Explorer，浏览器会提示您下载的*v1.json*文件。

    ![下载 IE 中的 JSON 元数据](./media/app-service-api-dotnet-get-started/iev1json.png)

    如果您使用 Chrome，Firefox 或边，浏览器将显示在浏览器窗口中的 JSON。 不同的浏览器以不同的方式，处理 JSON 并且浏览器窗口中与该示例可能不是完全一样。

    ![中铬的 JSON 元数据](./media/app-service-api-dotnet-get-started/chromev1json.png)

    下面的示例显示第一节 Swagger 元数据 api，使用 Get 方法的定义。 此元数据是驱使您在以下步骤中，使用 Swagger 用户界面，用于在本教程后面部分自动生成客户端代码。

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. 关闭浏览器，停止调试 Visual Studio。

5. 在 ToDoListDataAPI 项目在**解决方案资源管理器**中，打开*App_Start\SwaggerConfig.cs*文件，然后向下滚动到 174 行取消注释下面的代码。

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    在安装项目中的 Swashbuckle 包时创建的*SwaggerConfig.cs*文件。 该文件提供了多种方法来配置 Swashbuckle。

    已取消注释的代码使您在以下步骤中使用 Swagger 用户界面。 使用 API 的应用程序项目模板创建一个 Web API 项目时，此代码被注释掉，默认情况下作为一种安全措施。

6. 再次运行项目。

7. 在您浏览器的地址栏，添加`swagger`到末尾的行，然后按返回。 (URL 为`http://localhost:45914/swagger`。)

8. Swagger 用户界面页出现时，请单击**任务列表**以查看可用的方法。

    ![Swagger 用户界面可用的方法](./media/app-service-api-dotnet-get-started/methods.png)

9. 单击列表中的第一个**获得**按钮。

10. 在**参数**部分中，输入一个星号作为值的`owner`参数，然后单击**重试出来**。

    在以后的教程中添加身份验证时，中间层将提供对数据层的实际用户 ID。 现在，所有的任务将具有星号作为其所有者 ID 而不启用身份验证的情况下运行的应用程序。

    ![Swagger 用户界面试验](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Swagger 用户界面调用任务列表获取方法并显示响应代码和 JSON 的结果。

    ![Swagger 用户界面试一下结果](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. **开机自检**，请单击，然后在**模型架构**下单击框。

    单击模型架构 prefills 的输入的框，您可以在其中指定 Post 方法的参数值。 （如果这不起作用在 Internet Explorer 中，使用不同的浏览器或手动输入的参数值下, 一步。）  

    ![Swagger 用户界面试一下开机自检](./media/app-service-api-dotnet-get-started/post.png)

12. 更改在 JSON`todo`参数输入框，使其类似于以下示例中，或者用来代替自己的说明文字︰

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. 单击**重试出来**。

    应做事项列表 API 返回用于指示成功 HTTP 204 响应代码。

14. 单击第一个**获取**按钮，然后在页上的相应部分中单击**尝试它**按钮。

    Get 方法响应现在包括新待办事项。

15. 可选︰ 还尝试将，删除，并由 ID 的方法获得。

16. 关闭浏览器，停止调试 Visual Studio。

Swashbuckle 适用于 ASP.NET Web API 的任何项目。 如果您想要将生成 Swagger 元数据添加到现有项目，只需安装 Swashbuckle 软件包。

>[AZURE.NOTE] Swagger 元数据包括每个 API 操作的唯一 ID。 默认情况下，Swashbuckle 可能会生成 Web API 的控制器方法的重复 Swagger 操作 Id。 这种情况下，如果您的控制器具有重载 HTTP 方法，如`Get()`， `Get(id)`。 有关如何处理重载的信息，请参阅[自定义 Swashbuckle 生成的 API 定义](app-service-api-dotnet-swashbuckle-customize.md)。 如果在 Visual Studio 中创建 Web API 项目使用 Azure API 应用程序模板，生成唯一的操作 Id 代码会自动添加到*SwaggerConfig.cs*文件中。  

## <a id="createapiapp"></a>在 Azure 创建 API 的应用程序，并将代码部署到它

在本节中，您将使用 Azure 的工具集成到 Visual Studio **Web 发布**向导在 Azure 中创建一个新的 API 应用程序。 ToDoListDataAPI 项目部署到新的 API 应用程序然后调用 API 通过运行 Swagger 用户界面。

1. 在**解决方案资源管理器**中用鼠标右键单击 ToDoListDataAPI 项目，然后单击**发布**。

    ![单击发布 Visual Studio 中](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  在**Web 发布**向导的**配置文件**步骤中，请单击**Microsoft Azure 应用程序服务**。

    ![单击 Azure 应用程序服务在发布站点](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. 登录到您的 Azure 帐户如果您有不这样做，或刷新您的凭据，如果它们过期。

4. 在应用程序服务对话框中，选择您要使用 Azure**订阅**，然后单击**新建**。

    ![在应用程序服务对话框中单击新建](./media/app-service-api-dotnet-get-started/clicknew.png)

    此时将显示**创建应用程序服务**对话框中的**托管**选项卡。

    正在部署一个已安装的 Swashbuckle 的 Web API 项目，因为 Visual Studio 假定您想要创建 API 的应用程序。 通过**API 的应用程序名称**标题并**更改类型**下拉列表设置为**API 的应用程序**这一事实表明这一点。

    ![在对话框的应用程序服务的应用程序类型](./media/app-service-api-dotnet-get-started/apptype.png)

5. 输入*azurewebsites.net*域中是唯一一个**API 的应用程序名称**。 您可以接受默认名称，Visual Studio 将建议。

    如果您输入一个其他人已经使用的名称，您将看到右边一个红色感叹号。

    API 的应用程序的 URL 将被`{API app name}.azurewebsites.net`。

6. 在**资源组**下拉列表，单击**新建**，然后输入"ToDoListGroup"或其他名称，如果您喜欢。

    资源组是 API 的应用程序、 数据库、 虚拟机，等如 Azure 资源的集合。 对于本教程，则最好创建一个新的资源组，因为这可以使它轻松地在一个步骤中删除所有为教程创建的 Azure 资源。

    此框允许您选择一个现有的[资源组](../azure-resource-manager/resource-group-overview.md)，或通过键入名称不同于您的订阅中的任何现有资源组中创建一个新。

7. 单击**应用程序服务计划**下拉列表旁边的**新建**按钮。

    屏幕抓图显示采样值的**API 的应用程序名称**、**订阅**和**资源组**— 您的值将会有所不同。

    ![创建应用程序服务对话框](./media/app-service-api-dotnet-get-started/createas.png)

    在以下步骤中，您将创建新的资源组的应用程序服务计划。 应用程序服务计划指定 API 应用程序运行的计算资源。 例如，如果您选择的免费层，API 应用程序上运行共享虚拟机，而为某些支付层运行在专用的虚拟机上。 有关应用程序服务计划的信息，请参阅[应用程序服务计划概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。

8. 在**配置应用程序服务计划**对话框中，输入"ToDoListPlan"或其他名称，如果您喜欢。

9. 在**位置**下拉列表中选择与您最近的位置。

    此设置指定您的应用程序将运行在哪个 Azure 数据中心。 选择一个位置接近地减少[延迟](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)。

10. 在**大小**下拉列表，单击**自由**。

    对于本教程，自由定价层将提供足够的性能。

11. 在**配置应用程序服务计划**对话框中，单击**确定**。

    ![单击确定在配置应用程序服务计划](./media/app-service-api-dotnet-get-started/configasp.png)

12. 在**创建应用程序服务**对话框中，单击**创建**。

    ![在创建应用程序服务对话框中单击创建](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio 创建 API 的应用程序和发布配置文件包含所有 API 的应用程序所需的设置。 然后它会打开**Web 发布**向导，您将使用部署项目。

    **Web 发布**向导将打开**连接**选项卡 （如下所示）。

    在**连接**选项卡上的**服务器**和**站点名称**设置指向您 API 的应用程序。 **用户名**和**密码**是 Azure 将为您创建的部署凭据。 完成部署后，Visual Studio 会打开浏览器的**目标 URL** （的**目标 URL**的唯一目的是）。  

13. 单击**下一步**。

    ![发布网站的连接选项卡中单击下一步](./media/app-service-api-dotnet-get-started/connnext.png)

    下一个选项卡是**设置**选项卡 （如下所示）。 在这里您可以更改生成配置选项卡，以部署[远程调试](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)的调试版本。 选项卡还提供了几个**文件发布选项**︰

    * 删除目标的其他文件
    * 在发布过程中进行预编译
    * 从 App_Data 文件夹中排除文件

    对于本教程，您不需要这些。 有关执行哪些操作的详细信息，请参阅[如何︰ 部署 Web 项目使用一键式发布在 Visual Studio 中](https://msdn.microsoft.com/library/dd465337.aspx)。

14. 单击**下一步**。

    ![在发布站点的设置选项卡中单击下一步](./media/app-service-api-dotnet-get-started/settingsnext.png)

    接下来是**预览**选项卡 （如下所示），其中给出了，您有机会查看哪些文件将从您的项目复制到 API 的应用程序。 如果为 API 应用程序已经部署到前面来部署项目，复制已更改的文件。 如果您想要查看列表的内容会被复制，您可以单击**开始预览**按钮。

15. 单击**发布**。

    ![发布网站的预览选项卡中单击发布](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio 将 ToDoListDataAPI 项目部署到新的 API 应用程序。 **输出**窗口中记录成功的部署，并到 API 的应用程序的 URL 打开的浏览器窗口中显示"成功创建"页。

    ![输出窗口成功部署](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![新的 API 创建成功的应用程序页](./media/app-service-api-dotnet-get-started/appcreated.png)

16. 添加到浏览器的地址栏中的 URL 的"swagger"，然后按 enter 键。 (URL 为`http://{apiappname}.azurewebsites.net/swagger`。)

    浏览器将显示相同的 Swagger 用户界面，您已经看到了更早版本，但现在在云中运行。 尝试使用 Get 方法，并看到你回默认值 2 的待办事项。 前面所做的更改已保存在本地计算机中的内存。

17. 打开[Azure 的门户](https://portal.azure.com/)。

    Azure 的门户是一个 web 界面，用于管理 Azure 资源如 API 的应用程序。

18. 单击**更多服务 > 应用程序服务**。

    ![浏览应用程序服务](./media/app-service-api-dotnet-get-started/browseas.png)

19. 在**应用程序服务**刀片式服务器，找到并单击您新的 API 应用程序。 （在 Azure 的门户中，向右打开的窗口称为*刀片*。

    ![应用程序服务的刀片](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    打开两个刀片。 1 个刀片具有 API 应用程序中，事件的概述和一个有很长的设置，您可以查看和更改列表。

20. 在**设置**刀片式服务器，查找**API**节，然后单击**API 定义**。

    ![在设置刀片式服务器 API 定义](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    **API 定义**刀片式服务器，可以指定以 JSON 格式返回 Swagger 2.0 元数据的 URL。 当 Visual Studio 创建 API 的应用程序时，它将设置 API 定义 URL 为默认值 Swashbuckle 生成的元数据的前面，看到这是 API 的应用程序的基 URL 加上`/swagger/docs/v1`。

    ![API 定义 URL](./media/app-service-api-dotnet-get-started/apidefurl.png)

    选择要为其生成客户端代码 API 应用程序时，Visual Studio 将此 URL 中检索的元数据。

## <a id="codegen"></a>生成数据层的客户端代码

将 Swagger 集成到 Azure API 的应用程序的优点之一是自动代码生成。 生成的客户端类更加轻松地编写代码，调用 API 的应用程序。

ToDoListAPI 项目已生成的客户端代码中，但是在下面的步骤会将其删除并重新生成它以查看如何进行代码生成。

1. 在 Visual Studio**解决方案资源管理器**里在 ToDoListAPI 项目中，删除*ToDoListDataAPI*文件夹。 **注意︰ 删除文件夹只不 ToDoListDataAPI 项目。**

    ![删除生成的客户端代码](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    使用您将要经过的代码生成过程创建此文件夹。

2. 用鼠标右键单击 ToDoListAPI 项目，然后单击**添加 > REST API 客户端**。

    ![在 Visual Studio 中添加 REST API 客户端](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. 在**添加 REST API 客户端**对话框中，单击**Swagger URL**，然后单击**选择 Azure 资产**。

    ![选择 Azure 的资产](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. 在**应用程序服务**对话框中，展开您在使用本教程并选择您 API 的应用程序，该资源组，然后单击**确定**。

    ![选择用于代码生成的 API 的应用程序](./media/app-service-api-dotnet-get-started/codegenselect.png)

    请注意，在返回到**添加 REST API 客户端**对话框时，文本框中已填充使用 API 定义 URL 值，您看到了前面的门户。

    ![API 定义 URL](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] 获取用于代码生成的元数据的替代方法是输入的 URL 直接而不是通过浏览对话框。 或者，如果要部署到 Azure 之前生成客户端代码，您可以在本地运行 Web API 项目，转到一个 URL，它提供的 Swagger 的 JSON 文件，保存文件，然后使用**选择现有的 Swagger 元数据文件**选项。

5. 在**添加 REST API 客户端**对话框中，单击**确定**。

    Visual Studio 创建命名 API 的应用程序的文件夹，并生成客户端类。

    ![生成的客户端的代码文件](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. 在 ToDoListAPI 项目中，打开要使用生成的客户端调用 API 的行 40 处的代码，请参阅*Controllers\ToDoListController.cs* 。

    下面的代码段演示如何代码实例化客户端对象，并调用 Get 方法。

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    构造函数参数获取端点 URL 从`toDoListDataAPIURL`应用程序设置。 在 Web.config 文件中，此值设置为本地 IIS Express URL 的 API 项目，以便您可以在本地运行应用程序。 如果您省略构造函数参数，默认终结点为生成的代码中的 URL。

7. 客户端类将生成一个不同的名称基于 API 的应用程序名称;更改*Controllers\ToDoListController.cs*中的代码，使什么在您的项目中生成的类型名称匹配。 例如，如果命名您的 API 的应用程序 ToDoListDataAPI071316，您将更改此代码︰

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

更改为︰

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>创建 API 应用程序承载的中间层

前面您[创建数据层 API 的应用程序和部署到该代码](#createapiapp)。  现在您遵循中间层 API 应用程序所在的同一过程。

1. 在**解决方案资源管理器**中用鼠标右键单击中间层 ToDoListAPI 项目 （没有数据层 ToDoListDataAPI），然后再单击**发布**。

    ![单击发布 Visual Studio 中](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  在**发布**向导的**配置文件**选项卡上，单击**Microsoft Azure 应用程序服务**。

3. 在**应用程序服务**对话框中，单击**新建**。

4. 在**创建应用程序服务**对话框中的**托管**选项卡上，接受默认设置**API 的应用程序名称**或输入*azurewebsites.net*域中是唯一的名称。

5. 选择您一直使用 Azure**订阅**。

6. **资源组**下拉列表，选择您在前面创建的相同的资源组。

7. 在**应用程序服务计划**下拉列表，选择您在前面创建相同的计划。 它将默认为该值。

8. 单击**创建**。

    Visual Studio 创建 API 的应用程序、 创建发布配置文件，并显示**Web 发布**向导**连接**步骤。

9.  在**Web 发布**向导的**连接**步骤中，单击**发布**。

    Visual Studio 将 ToDoListAPI 项目部署到新的 API 应用程序并打开浏览器访问 API 的应用程序的 URL。 将显示"已成功创建"页。

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>配置中间层调用数据层

如果您现在调用中间层 API 的应用程序，它将尝试调用使用的本地主机 URL 仍然在 Web.config 文件中的数据层。 在此部分中输入数据层 API 应用程序 URL 到中间层 API 应用程序中的环境设置。 当中间层 API 应用程序中的代码检索数据层 URL 设置时，环境设置将重写 Web.config 文件中的是什么。

1. 转到[Azure 的门户网站](https://portal.azure.com/)，并浏览到**应用程序 API**刀片式服务器 API 应用程序所创建的用来承载 TodoListAPI （中间层） 项目。

2. 在 API 的应用程序的**设置**刀片式服务器，单击**应用程序设置**。

3. 在 API 的应用程序的**应用程序设置**刀片式服务器，向下滚动到**应用程序设置**部分中，添加下面的键和值。 值将是您在本教程中发布首个 API 应用程序的 URL。

  	| **密钥** | toDoListDataAPIURL |
  	|---|---|
  	| **值** | https://{your 数据 API 层的应用程序名称}.azurewebsites.net |
  	| **示例** | https://todolistdataapi.azurewebsites.net |

4. 单击**保存**。

    ![单击保存应用程序设置](./media/app-service-api-dotnet-get-started/asinportal.png)

    当在 Azure 中运行代码时，此值现在将重写 Web.config 文件中的本地主机 URL。

## <a name="test"></a>测试

1. 在浏览器窗口中，浏览到您刚创建的 ToDoListAPI 新中间层 API 应用程序的 URL。 您可以通过单击门户中的 API 的应用程序的主刀片式服务器中的 URL 那里获取。

2. 添加到浏览器的地址栏中的 URL 的"swagger"，然后按 enter 键。 (URL 为`http://{apiappname}.azurewebsites.net/swagger`。)

    浏览器将显示相同的 Swagger 用户界面，您以前看到过的 ToDoListDataAPI，但现在`owner`不是 Get 操作的必填的字段，因为中间层 API 的应用程序为您到数据层 API 应用程序发送的值。 (中间层执行身份验证教程时，将发送实际的用户 Id`owner`参数; 现在它被硬编码一个星号的。)

3. 尝试获取方法和其他方法来验证中间层 API 应用程序成功调用数据层 API 的应用程序。

    ![Swagger 用户界面获取方法](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>故障排除

在学习本教程这里遇到问题的情况下有一些故障排除的建议︰

* 请确保您正在使用最新版本的[.NET 的 Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003)。

* 项目名称的两个是类似 (ToDoListAPI，ToDoListDataAPI)。 如果事情看起来如说明中所述，当您正在处理一个项目时，，请确保已打开正确的项目。

* 如果您在公司网络上，并试图通过防火墙部署到 Azure 应用程序服务，请确保端口 443 和 8172 可用于 Web 部署。 如果您无法打开这些端口，可以使用其他部署方法。  请参见[部署您的应用程序到 Azure 应用程序服务](../app-service-web/web-sites-deploy.md)。

* "工艺路线的名称必须是唯一的"错误--您可以得到这些如果您意外地到 API 的应用程序部署错误的项目，然后以后部署给它一个正确。 若要解决此问题，重新部署到 API 的应用程序，和**Web 发布**向导的**设置**选项卡上正确的项目选择**删除目标的其他文件**。

在 Azure 应用程序服务中运行 ASP.NET API 应用程序后，您可以了解 Visual Studio 功能简化故障排除的详细信息。 有关日志记录的信息，远程调试，和的详细信息，请参阅[疑难解答 Azure 应用程序服务在 Visual Studio 中的应用程序](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)。

## <a name="next-steps"></a>下一步行动

您已经看到如何将现有的 Web API 项目部署到 API 的应用程序，生成的 API 的应用程序，客户端代码和使用从.NET 客户端 API 应用程序。 本系列下一步的教程展示如何[使用 CORS 从 JavaScript 客户端 API 应用程序使用](app-service-api-cors-consume-javascript.md)。

有关客户端的代码生成的详细信息，请参阅 GitHub.com 上的[Azure/AutoRest](https://github.com/azure/autorest)存储库。 使用生成的客户端出现的问题的帮助，请打开一个[AutoRest 存储库中的问题](https://github.com/azure/autorest/issues)。

如果您想从头开始创建新的 API 的应用程序项目，则使用**Azure API 应用程序**模板。

![在 Visual Studio 中的 API 的应用程序模板](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

**Azure API 的应用程序**项目模板相当于选择**空**4.5.2 ASP.NET 模板中，单击该复选框以添加 Web API 支持和 Swashbuckle NuGet 程序包安装。 此外，该模板会添加一些旨在防止重复 Swagger 操作 Id 创建的 Swashbuckle 配置代码。 API 的应用程序项目创建后，您可以将其部署到 API 应用程序您在本教程中看到的那样。
