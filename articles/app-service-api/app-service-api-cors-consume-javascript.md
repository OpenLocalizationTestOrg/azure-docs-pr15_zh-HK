<properties
    pageTitle="支持应用程序服务中的 CORS |Microsoft Azure"
    description="了解如何使用 Azure Azure 应用程序服务中 CORS 支持。"
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
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>从使用 CORS 的 JavaScript API 应用程序使用

应用程序服务提供了对[跨原始资源共享 (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)，从而使 JavaScript 客户端以进行跨域调用的 Api 的托管 API 应用程序中的内置支持。 应用程序服务允许您配置您的 API CORS 访问而无需编写任何代码在您的 API。

本文包含两部分︰

* [如何配置 CORS](#corsconfig)节一般解释如何配置 CORS 的 API 的应用程序、 web 应用程序和移动应用程序。 它同样适用于所有支持的应用程序服务，包括.NET，Node.js 和 Java 的框架。 

* 文章开头[继续.NET 快速入门教程](#tutorialstart)部分，是演示 CORS 支持通过构建中[获取第一个 API 应用程序启动教程](app-service-api-dotnet-get-started.md)上的教程。 

## <a id="corsconfig"></a>如何配置 CORS 在 Azure 应用程序服务

在 Azure 的门户网站或通过[Azure 资源管理器](../azure-resource-manager/resource-group-overview.md)的工具，您可以配置 CORS。

#### <a name="configure-cors-in-the-azure-portal"></a>在 Azure 门户配置 CORS

8. 在浏览器转到[Azure 的门户](https://portal.azure.com/)。

2. 单击**应用程序服务**，然后单击您 API 的应用程序的名称。

    ![在门户网站中选择 API 的应用程序](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. 打开右侧的刀片式服务器**API 的应用程序****设置**刀片中, 找到**API**部分，然后单击**CORS**。

    ![在设置刀片式服务器选择 CORS](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. 在文字框中输入 URL 或您想要允许来自 JavaScript 调用的 Url。


    例如，如果部署到 web 应用程序名为 todolistangular 的 JavaScript 应用程序，请输入"https://todolistangular.azurewebsites.net"。 作为一种替代方法，您可以输入一个星号 （*） 指定接受的来源中的所有域。


13. 单击**保存**。

    ![单击保存](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    单击**保存**后，API 的应用程序将接受来自指定 Url 的 JavaScript 调用。

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>通过使用 Azure 资源管理器工具配置 CORS

您还可以通过使用[模板和 Azure 资源管理器](../resource-group-authoring-templates.md)中[Azure PowerShell](../powershell-install-configure.md)和[Azure CLI](../xplat-cli-install.md)命令行工具 API 使应用程序配置 CORS。 

CORS 属性设置 Azure 资源管理器模板的示例，打开[在本教程中的示例应用程序的存储库中的 azuredeploy.json 文件](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json)。 找到的部分的模板，如以下示例所示︰

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>继续.NET 快速入门教程

如果按照 Node.js 或 Java 入门教程系列用于 API 的应用程序，则您已完成获取启动的系列。 跳到[下一步行动](#next-steps)部分找到有关进一步学习使用 API 的应用程序的建议。

本文的其余部分是.NET 快速入门系列的延续，并假定您成功地完成[第一个教程](app-service-api-dotnet-get-started.md)。

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>ToDoListAngular 项目部署到新的 web 应用程序

在[第一个教程](app-service-api-dotnet-get-started.md)中，您创建一个中间层 API 应用程序和数据层 API 的应用程序。 在本教程中创建单页面应用程序 (SPA) web 应用程序，调用中间层 API 的应用程序。 对于 SPA 来起作用，您必须启用 CORS 上中间层 API 的应用程序。 

在[任务列表的示例应用程序](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)中，ToDoListAngular 项目是一个简单的 AngularJS 客户端调用的中间层 ToDoListAPI Web API 项目。 在*app/scripts/todoListSvc.js*文件中的 JavaScript 代码使用 AngularJS HTTP 提供程序调用 API。 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>创建新 web 应用程序的 ToDoListAngular 项目

要创建新的应用程序服务 web 应用程序并将项目部署到它的过程是类似于您用于[创建和部署 API 应用程序中第一个在本系列教程](app-service-api-dotnet-get-started.md#createapiapp)所看到的那样。 唯一的区别是，应用程序类型是**Web 应用程序**而不是**API 的应用程序**。  有关该对话框的屏幕快照，请参见 

1. 在**解决方案资源管理器**中用鼠标右键单击 ToDoListAngular 项目，然后单击**发布**。

3.  在**发布**向导的**配置文件**选项卡上，单击**Microsoft Azure 应用程序服务**。

5. 在**应用程序服务**对话框中，单击**新建**。

3. 在**创建应用程序服务**对话框中的**托管**选项卡上，输入*azurewebsites.net*域中是唯一一个**Web 应用程序名称**。 

5. 选择您想要使用 Azure**订阅**。

6. **资源组**下拉列表，选择您在前面创建的相同的资源组。

4. 在**应用程序服务计划**下拉列表中，选择您在前面创建相同的计划。 

7. 单击**创建**。

    Visual Studio 创建 web 应用程序、 创建发布配置文件，并显示**Web 发布**向导**连接**步骤。

    不要单击**发布**。 在下面的部分中，您可以配置新的 web 应用程序调用中间层 API 应用程序正在运行的应用程序服务中。 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Web 应用程序设置中设置的中间层 URL

1. 转到[Azure 门户](https://portal.azure.com/)，然后导航到**Web 应用程序**刀片式服务器为的 web 应用程序创建的用来承载 TodoListAngular （前端） 项目。

2. 单击**设置 > 应用程序设置**。

3. 在**应用程序设置**部分中，添加下面的键和值︰

  	|密钥|值|示例
  	|---|---|---|
  	|toDoListAPIURL|https://{your 中间层 API 的应用程序名称}.azurewebsites.net|https://todolistapi0121.azurewebsites.net|

4. 单击**保存**。

    当在 Azure 中运行代码时，此值将重写*Web.config*文件中的本地主机 URL。 

    获取设置值的代码是*index.cshtml*中︰

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    *TodoListSvc.js*中的代码使用的设置︰

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>ToDoListAngular web 项目部署到新的 web 应用程序

*  在 Visual Studio 中， **Web 发布**向导中，**连接**步骤中单击**发布**。

    Visual Studio 将 ToDoListAngular 项目部署到新的 web 应用程序并打开浏览器的 web 应用程序的 url。 

### <a name="test-the-application-without-cors-enabled"></a>测试应用程序，而启用了 CORS 

2. 在浏览器的开发工具，打开控制台窗口。

3. 在浏览器窗口中显示 AngularJS 用户界面，请单击**任务列表**链接。

    JavaScript 代码尝试调用中间层 API 的应用程序，但调用失败，因为在后端之外的其他域中运行的前端。 浏览器的开发人员工具控制台窗口会显示跨原点的错误信息。

    ![跨原始错误消息](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>对于中间层 API 应用程序中配置 CORS

在本节中，您将配置 ToDoListAPI API 的应用程序的中间层的 CORS 设置 Azure 中。 此设置将允许接收来自 ToDoListAngular 项目创建 web 应用程序的 JavaScript 调用的 API 应用程序的中间层。

8. 在浏览器中，转到[Azure 的门户](https://portal.azure.com/)。

2. 单击**应用程序服务**，然后单击 ToDoListAPI （中间层） API 的应用程序。

    ![在门户网站中选择 API 的应用程序](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. 打开右侧的刀片式服务器**API 的应用程序****设置**刀片中, 找到**API**部分，然后单击**CORS**。

    ![在门户网站中选择 CORS](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. 在文本框中，输入 ToDoListAngular （前端） web 应用程序的 URL。 例如，如果您部署到 web 应用程序名为 todolistangular0121 的 ToDoListAngular 项目，允许从 URL 调用`https://todolistangular0121.azurewebsites.net`。

    作为一种替代方法，您可以输入一个星号 （*） 指定接受的来源中的所有域。

13. 单击**保存**。

    ![单击保存](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    单击**保存**后，从指定的 URL 的 JavaScript 调用 API 的应用程序将会接受。 在此屏幕快照中，ToDoListAPI0223 API 的应用程序将接受 JavaScript 从 ToDoListAngular web 应用程序的客户端调用。

### <a name="test-the-application-with-cors-enabled"></a>使用启用了 CORS 测试应用程序

* 打开到 HTTPS URL 的 web 应用程序的浏览器。 

    这一次该应用程序允许您查看、 添加、 编辑和删除待办事项。 

    ![待办事项列表的示例应用程序的页](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>应用程序服务与 Web API CORS CORS

在 Web API 项目中，您可以安装[Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet 程序包以从您的 API 将接受哪些域 JavaScript 调用在代码中指定。
 
Web API CORS 支持是比应用程序服务 CORS 支持更灵活。 例如，在代码中可以指定不同接受的来源不同的操作方法时为应用程序服务 CORS 您指定一组接受来源的所有 API 的应用程序的方法。

> [AZURE.NOTE] 不要尝试在一个 API 应用程序中使用 Web API CORS 和应用程序服务 CORS。 应用程序服务 CORS 优先和 Web API CORS 将没有任何效果。 例如，如果启用应用程序服务中的一个来源域启用 Web API 代码中的所有源域，您 Azure API 的应用程序将只接受从 Azure 中指定的域进行调用。

### <a name="how-to-enable-cors-in-web-api-code"></a>如何在 Web API 代码启用 CORS

以下步骤概述了启用 Web API CORS 支持的过程。 有关详细信息，请参见[ASP.NET Web API 2 中启用跨原始请求](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)。

1. 在 Web API 项目，安装[Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet 程序包。

1. 包括`config.EnableCors()` **WebApiConfig**类，如下面的示例中所示的**注册**方法中的代码行。 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. 在您的 Web API 控制器添加`using`语句`System.Web.Http.Cors`命名空间中，并添加`EnableCors`特性给控制器类或单个操作方法。 在以下示例中，CORS 支持对整个控制器。

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>与 API 应用程序使用 Azure API 管理

如果您使用 API 应用程序使用 Azure API 管理，API 管理而不是 API 应用程序中配置 CORS。 有关详细信息，请参阅以下资源︰

* [Azure 的 API 管理概述 (视频︰ CORS 开始于 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [跨域策略的 API 管理](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>故障排除

在的情况下通过本教程时遇到一个问题，这里有一些故障排除建议。

* 请确保您正在使用最新版本的[Visual Studio 2015年的.NET 的 Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003)。

* 请确保您输入的`https`在 CORS 设置中，并确保您正在使用`https`运行前端 web 应用程序。

* 请确保您输入 CORS 设置，中间层 API 应用程序中，而不使前端 web 应用程序。

* 如果您要在应用程序代码和 Azure 应用程序服务配置 CORS，请注意，应用程序服务 CORS 设置将覆盖您在应用程序代码中做任何。 

了解更多有关 Visual Studio 功能可以简化故障排除的过程，请参阅[疑难解答 Azure 应用程序服务在 Visual Studio 中的应用程序](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)。

## <a name="next-steps"></a>下一步行动 

在本文中，您看到了如何启用应用程序服务 CORS 的支持，以便在不同的域中，客户端 JavaScript 代码可以调用 API。 若要了解有关 API 的应用程序的详细信息，请阅读[简介中的应用程序服务的身份验证](../app-service/app-service-authentication-overview.md)，并再转到[用户身份验证 API 应用程序的](app-service-api-dotnet-user-principal-auth.md)教程。
