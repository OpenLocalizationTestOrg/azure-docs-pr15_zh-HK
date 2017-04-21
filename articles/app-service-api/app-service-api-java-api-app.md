<properties
    pageTitle="生成和部署 Java API 的应用程序在 Azure 应用程序服务"
    description="了解如何创建一个 Java API 的应用程序的包并将其部署到 Azure 应用程序服务。"
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>生成和部署 Java API 的应用程序在 Azure 应用程序服务

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

本教程演示如何创建一个 Java 应用程序并将它部署到 Azure 应用程序服务 API 应用程序使用[Git]。 在本教程中的说明进行操作的后面可以在任何能够运行 Java 的操作系统上。 在本教程中的代码是使用[Maven]构建的。 [Jax RS]用于创建 RESTful 服务，并根据[Swagger]元数据标准，使用[Swagger 编辑器]生成。

## <a name="prerequisites"></a>系统必备组件

1. [Java 开发人员工具包 8]\(或更高版本)
1. [Maven]将开发计算机上安装
1. [Git]安装在开发计算机上
1. 对[Microsoft Azure]的付费或[免费试用]订阅
1. 如[把邮递员弄]一个 HTTP 测试应用程序

## <a name="scaffold-the-api-using-swaggerio"></a>Scaffold API 使用 Swagger.IO

使用 swagger.io 在线编辑器，您可以输入 Swagger JSON 或 YAML 表示您的 API 的结构的代码。 一旦设计 API 面，您可以导出为各种平台和框架代码。 在下一部分中，将修改基架的代码以包括模拟功能。 

本演示将开始与 Swagger JSON 将粘贴到 swagger.io 编辑器中，然后将用于生成 JAX RS 使用访问 REST API，终结点的代码体。 然后，您将编辑基架的代码以返回模拟数据，模拟 REST API，顶部的数据持久性机制构建。  

1. 将下面的 Swagger JSON 代码复制到剪贴板︰

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. 定位到[在线 Swagger 编辑器]。 一次，请单击**文件-> 粘贴 JSON**菜单项。

    ![粘贴 JSON 菜单项][paste-json]

1. 在联系人列表 API Swagger JSON 先复制粘贴。 

    ![将 JSON 代码粘贴到 Swagger][pasted-swagger]

1. 查看文档页面和 API 摘要编辑器中呈现。 

    ![查看 Swagger 生成文档][view-swagger-generated-docs]

1. 选择**生成服务器-> JAX RS**菜单选项来 scaffold 以后将编辑它来模拟实现中添加的服务器端代码。 

    ![生成代码菜单项][generate-code-menu-item]

    代码生成之后，您将提供下载的 ZIP 文件。 此文件包含启用了基架 Swagger 代码生成器代码和所有关联生成脚本。 解压缩到开发工作站上的一个目录整个库。 

## <a name="edit-the-code-to-add-api-implementation"></a>编辑要添加 API 实现的代码

在本节中，您将替换自定义代码由 Swagger 生成的代码的服务器端实现。 新的代码将返回数组列表的联系人实体调用客户端。 

1. 打开*Contact.java*模型文件中，它位于*src/gen/java/io/swagger/模型*文件夹中，使用[Visual Studio 代码]或您喜爱的文本编辑器。 

    ![打开联系人模型文件][open-contact-model-file]

1. 将以下构造函数添加到**联系人**类。 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. 打开位于*src/主/java/io/swagger/api/实现*文件夹中，使用[Visual Studio 代码]或您喜爱的文本编辑器的*ContactsApiServiceImpl.java*服务的实现文件。

    ![打开联系人服务代码文件][open-contact-service-code-file]

1. 此新的代码来添加模拟实现的服务代码覆盖文件中的代码。 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. 打开命令提示符处，将目录更改为您的应用程序的根文件夹。

1. 执行下面的 Maven 命令来生成代码，并使用 Jetty 应用服务器本地运行。 

        mvn package jetty:run

1. 您应该看到命令窗口中反映 Jetty 已在端口 8080 上启动代码。 

    ![打开联系人服务代码文件][run-jetty-war]

1. 使用[把邮递员弄]到"获取所有联系人"http://localhost:8080/api/联系人 API 方法进行请求。

    ![调用 API 的联系人][calling-contacts-api]

1. 使用[把邮递员弄]到位于 http://localhost:8080/api/联系人/2 处的"获取特定的联系人"API 方法进行请求。

    ![调用 API 的联系人][calling-specific-contact-api]

1. 最后，通过在控制台中执行下面的 Maven 命令生成 Java 战争 （Web 档案） 文件。 

        mvn package war:war

1. 一旦生成 WAR 文件，它将被放置到**目标**文件夹。 浏览到**目标**文件夹，并将 WAR 文件重命名为**ROOT.war**。 （请确保大小写匹配此格式）。

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. 最后，执行以下命令，创建一个**部署**文件夹，用于将 WAR 文件部署到 Azure 应用程序的根文件夹中。 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>将输出发布到 Azure 应用程序服务

在本节中将了解如何创建新 API 应用程序使用 Azure 门户，准备该 API 的应用程序托管 Java 应用程序，并将新创建的 WAR 文件部署到 Azure 应用程序服务来运行您新的 API 应用程序。 

1. 通过单击**新建-> Web + 手机-> API 的应用程序**菜单项，输入您的应用程序的详细信息，然后单击**创建**在[Azure 门户]，创建新的 API 应用程序。

    ![创建新的 API 应用程序][create-api-app]

1. 一旦您 API 的应用程序创建，打开您的应用程序**设置**刀片式服务器，，然后单击**应用程序设置**菜单项。 选择最新的 Java 版本中可用的选项，然后在**Web 容器**菜单中，选择最新的 Tomcat，然后单击**保存**。

    ![在刀片式服务器 API 应用程序中设置 Java][set-up-java]

1. 单击**部署凭据**设置菜单项，并提供用户名和密码，您想要将文件发布到您的 API 应用程序使用。 

    ![集中部署凭据][deployment-credentials]

1. 单击**部署源**设置菜单项。 一次，请单击**选择源**按钮，选择**本地 Git 存储库**选项，然后单击**确定**。 这将创建在 Azure，具有与您 API 的应用程序的关联中运行一个 Git 存储库。 将代码提交到*主*分支的 Git 存储库，每次您的代码将被发布为实时运行 API 的应用程序实例。 

    ![设置新本地 Git 存储库][select-git-repo]

1. 将新的 Git 存储库的 URL 复制到剪贴板。 这可以节省，它将是十分重要的一段时间。 

    ![设置您的应用程序的新 Git 存储库][copy-git-repo-url]

1. Git 推到联机库的 WAR 文件。 为此，导航到前面，以便可以轻松地提交到存储库中运行的应用程序服务中的代码创建的**部署**的文件夹。 一旦您在控制台窗口中，导航到 webapps 文件夹所在的文件夹，发出以下的 Git 命令，以启动进程并开始部署。 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    一旦发出**推送**请求时，您将被要求提供您之前创建的部署的凭据的密码。 输入您的凭据后，您会看到显示更新已部署的门户。

1. 如果您再次使用把邮递员弄来袭新部署 API 应用程序运行在 Azure 应用程序服务，您将看到行为是一致，而现在它像预期的那样，返回联系人数据且使用简单的代码更改为 Swagger.io 启用 Java 代码了基架。 

    ![使用联系人 REST API，您 Java Azure 中的活动][postman-calling-azure-contacts]

## <a name="next-steps"></a>下一步行动

在本文中，可以开始用 Swagger JSON 文件和一些启用 Java 代码从 Swagger.io 编辑器获得了基架。 在这里，简单的更改和 Git 部署过程导致了无功能的 API 应用程序用 Java 编写的。 下一个教程演示如何[从使用 CORS 的 JavaScript 客户端 API 应用程序使用][App Service API CORS]。 系列中的后续教程将说明如何实现身份验证和授权。

若要生成此示例，您可以了解有关[存储 Java SDK]来保持 JSON blob。 或者，可以使用[文档 DB Java 软件开发工具包]将联系人数据保存到 Azure 文档数据库。 

在 Azure 中使用 Java 的更多信息，请参见[Java 开发人员中心]。

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure 门户]: https://portal.azure.com/
[文档数据库的 Java SDK]: ../documentdb/documentdb-java-application.md
[免费试用版]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Java 开发人员中心]: /develop/java/
[Java 开发人员工具包 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Swagger 在线编辑器]: http://editor.swagger.io/
[把邮递员弄]: https://www.getpostman.com/
[存储 Java SDK]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger 编辑器]: http://editor.swagger.io/
[Visual Studio 代码]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
