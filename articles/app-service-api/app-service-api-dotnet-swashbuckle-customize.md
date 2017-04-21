
<properties 
    pageTitle="自定义 Swashbuckle 生成的 API 定义" 
    description="了解如何自定义 Swagger API 定义所生成的 Swashbuckle API 使应用程序在 Azure 应用程序服务。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>自定义 Swashbuckle 生成的 API 定义 

## <a name="overview"></a>概述

本文介绍了如何自定义 Swashbuckle 来处理常见方案，您可能需要更改默认行为︰

* Swashbuckle 生成控制器方法重载的重复操作标识符
* Swashbuckle 假定的唯一有效方法响应 HTTP 200 （正常） 
 
## <a name="customize-operation-identifier-generation"></a>自定义操作标识符生成

Swashbuckle 通过连接控制器名称和方法名称来生成 Swagger 操作标识符。 当您具有多个重载的方法时，此模式创建问题︰ Swashbuckle 生成重复操作 id，它是无效的 Swagger JSON。

例如，下面的控制器代码会导致 Swashbuckle 以生成三个 Contact_Get 操作 id。

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

您可以通过提供唯一的名称，如下所示，此示例的方法手动解决此问题︰

* 获取
* GetById
* GetPage

另一种是扩展 Swashbuckle，使其自动生成唯一的操作 id。

下面的步骤演示如何使用 Visual Studio API 应用程序预览项目模板包含在项目中的*SwaggerConfig.cs*文件自定义 Swashbuckle。  您还可以自定义 Swashbuckle 对 API 的应用程序的部署配置的 Web API 项目。

1. 创建自定义`IOperationFilter`实现 

    `IOperationFilter`接口提供一个可扩展性点的 Swashbuckle 用户想要自定义 Swagger 元数据过程的各个方面。 下面的代码演示如何更改操作 id 生成行为的一种方法。 该代码将参数名称追加到操作 id 名称。  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. 在*App_Start\SwaggerConfig.cs*文件中调用`OperationFilter`方法使以使用新的 Swashbuckle`IOperationFilter`实现。

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    被放弃的 Swashbuckle NuGet 程序包的*SwaggerConfig.cs*文件包含许多被注释掉的示例的扩展点。 此处未显示的其他意见。 

    进行此更改之后, 您`IOperationFilter`实现使用并导致生成的唯一的操作 id。
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>允许非 200 响应代码

默认情况下，Swashbuckle 假定 HTTP 200 (OK) 响应是从 Web API 方法的*只有*合法反应。 在某些情况下，您可能需要返回其他响应代码而不会导致客户端引发异常。  例如，下面的 Web API 代码演示您希望客户端接受 200 或作为有效响应 404 的方案。

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

在这种情况下，默认情况下生成 Swashbuckle Swagger 指定只能有一个合法的 HTTP 状态代码，HTTP 200。

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

由于 Visual Studio 使用 Swagger API 定义为客户端生成代码，它将创建客户端代码引发异常的 HTTP 200 以外的任何响应。 下面的代码是此示例 Web API 方法生成的 C# 客户端。

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle 提供的自定义列表的预期它所生成的 HTTP 响应代码的两种方式使用 XML 注释或`SwaggerResponse`属性。 属性是更加容易，而只是用 Swashbuckle 5.1.5 或更高版本。 在 Visual Studio 2013年的 API 应用程序预览新项目模板包括 Swashbuckle 5.0.0 版，因此，如果您使用该模板，并且不希望更新 Swashbuckle，您的唯一选项是使用 XML 注释。 

### <a name="customize-expected-response-codes-using-xml-comments"></a>自定义使用 XML 注释的预期的响应代码

使用此方法来指定响应代码，如果您的 Swashbuckle 版本早于 5.1.5。

1. 首先，添加您想指定的 HTTP 响应代码的方法的 XML 文档注释。 采用 Web API 的示例如上所示的操作，并对其应用的 XML 文档将导致如下例所示的代码。 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. *SwaggerConfig.cs*直接 Swashbuckle 使用的 xml 文件中添加的说明文档文件。

    * 打开*SwaggerConfig.cs*和*SwaggerConfig*类可以指定文档的 XML 文件的路径上创建一个方法。 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * 向下滚动到*SwaggerConfig.cs*文件中直到您看到的代码类似于下面的截屏被注释掉的行。 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * 取消注释以启用 Swagger 生成期间处理的 XML 注释的行。 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. 为了生成 XML 文档文件，进入项目的属性，使 XML 文档文件，如下面的屏幕快照中所示。 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

一旦您执行这些步骤，生成的 Swashbuckle Swagger JSON 将反映您在 XML 注释中指定的 HTTP 响应代码。 下面的屏幕快照演示了此新的 JSON 负载。 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

当使用 Visual Studio 为 REST API 重新生成客户端代码时，C# 代码将接受而不会引发异常，从而允许您使用代码来决定如何处理空值的联系人记录的返回 HTTP 确定和找不到状态代码。 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

[此 GitHub 存储库](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse)中找不到此演示的代码。 与 Web API 一起使用 XML 文档注释标记的项目还有的控制台应用程序项目中包含对此 API 生成的客户端。 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>自定义使用 SwaggerResponse 属性的预期的响应代码

[SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs)属性是用 Swashbuckle 5.1.5 及更高版本。 在的情况下必须在项目的早期版本，本部分首先介绍如何更新 Swashbuckle NuGet 程序包，以便您可以使用此属性。

1. 在**解决方案资源管理器**中右击 Web API 项目，然后单击**管理 NuGet 程序包**。 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. 单击*更新*按钮旁边的*Swashbuckle* NuGet 程序包。 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. 将*SwaggerResponse*属性添加到您要为其指定有效的 HTTP 响应代码的 Web API 操作方法。 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. 添加`using`的特性的命名空间的语句︰

        using Swashbuckle.Swagger.Annotations;
        
1. 浏览到您的项目的*/swagger/docs/v1* URL 和各种 HTTP 响应代码将显示在 Swagger JSON。 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

[此 GitHub 存储库](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes)中找不到此演示的代码。 与 Web API 一起使用*SwaggerResponse*属性修饰的项目还有的控制台应用程序项目中包含对此 API 生成的客户端。 

## <a name="next-steps"></a>下一步行动

本文已经展示了如何自定义 Swashbuckle 生成操作 id 和有效的响应代码的方式。 有关详细信息，请参阅[在 GitHub 上的 Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)。
 
