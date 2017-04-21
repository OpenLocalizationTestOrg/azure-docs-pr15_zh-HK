<properties
   pageTitle="了解 Azure 的活动目录的应用程序清单 |Microsoft Azure"
   description="Azure Active Directory 应用程序清单，其中表示应用程序的标识配置在 Azure AD 租户，并被用来促进 OAuth 授权、 同意的情况下体验和更多的详细覆盖范围。"
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>了解 Azure 活动目录的应用程序清单

集成与 Azure 活动目录 (AD) 的应用程序必须使用 Azure AD 租户，提供永久性标识配置为应用程序进行注册。 此配置在运行时，允许应用程序外包和代理通过 Azure AD 身份验证/授权的情况下启用商议。 关于 AD Azure 应用程序模型的详细信息，请参阅[添加、 更新，以及删除应用程序][ADD-UPD-RMV-APP]文章。

## <a name="updating-an-applications-identity-configuration"></a>更新应用程序的标识配置

有实际的多个选项可用于更新应用程序的标识配置，它的能力和程度上的困难，其中包括以下各不相同的属性︰

- ** [Azure 传统门户的][AZURE-CLASSIC-PORTAL]的 Web 用户界面**，您可以更新应用程序的最常见属性。 这是最快和最少错误易方式更新应用程序的属性的但并不表示赋予您完全访问所有属性，如下面两个方法。
- 对于更高级的方案需要更新 Azure 的经典门户中未公开的属性，您可以修改**应用程序清单**。 这是本文的重点，开始下一节中更详细地讨论。
- 还有可能为**编写的应用程序使用[Graph API] [GRAPH-API]**来更新您的应用程序，所需的最大工作量。 这可能是一个不错的选择也是如此，如果您正在编写管理软件，或者需要更新应用程序以自动方式定期的属性。

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>使用应用程序清单来更新应用程序的标识配置
通过[Azure 的传统门户网站][AZURE-CLASSIC-PORTAL]，您可以管理您的应用程序标识配置，通过下载和上载 JSON 文件表示形式，则调用的应用程序清单。 没有实际的文件存储在目录中。 应用程序清单是只是 HTTP GET 操作的 Azure 广告图形 API 应用程序实体，并上载是 HTTP 修补程序操作上的应用程序实体。

因此，要理解的格式和属性的应用程序清单，您将需要引用图形 API[应用程序实体][APPLICATION-ENTITY]文档。 尽管应用程序清单上载可执行的更新的示例包括︰

- 由 web API 公开**声明权限范围 (oauth2Permissions)** 。 请参阅[使用 Azure Active Directory 集成应用程序]中的"公开 Web Api 为其他应用程序"主题[INTEGRATING-APPLICATIONS-AAD]有关实现用户模拟信息使用 oauth2Permissions 委派的权限范围。 如前所述，应用程序实体属性记录在图形 API[实体和复杂类型]中[APPLICATION-ENTITY]参考文章，包括 oauth2Permissions 属性，它是一个集合类型[OAuth2Permission]的[APPLICATION-ENTITY-OAUTH2-PERMISSION]。
- **由您的应用程序公开声明应用程序角色 (appRoles)**。 应用程序实体的 appRoles 属性是一个集合类型[AppRole]的[APPLICATION-ENTITY-APP-ROLE]。 [基于角色的访问控制中使用 AD Azure 的云应用程序]，请参阅[RBAC-CLOUD-APPS-AZUREAD]实现示例的文章。
- **声明中已知客户端应用程序 (knownClientApplications)**，这将允许您从逻辑上将指定的客户端应用程序与资源/web API 的同意。
- 对登录用户 (groupMembershipClaims) 的**请求 Azure 广告发出声明成员身份的组**。  这也可以配置为颁发有关用户的目录角色成员身份的声明。 请参阅[授权在云应用程序中的使用 AD 组][AAD-GROUPS-FOR-AUTHORIZATION]实现示例的文章。
- **允许应用程序支持 OAuth 2.0 隐式授予**流 (oauth2AllowImplicitFlow)。 这种类型的授权流程使用嵌入的 JavaScript 网页或单个页面的应用程序 (SPA)。 隐式授权授予的详细信息，请参阅[了解 OAuth2 隐式授予在 Azure Active Directory 中的流动][IMPLICIT-GRANT]。
- **启用使用 X509 证书作为密钥**(keyCredentials)。 请参阅[生成 Office 365 中的服务和后台应用程序][O365-SERVICE-DAEMON-APPS]和[授权使用 Azure 资源管理器 API 的开发人员指南][DEV-GUIDE-TO-AUTH-WITH-ARM]实现示例的文章。
- **添加新的应用程序 ID URI**对于您的应用程序 (identifierURIs[])。 应用程序 ID Uri 用于唯一地标识应用程序在其 Azure AD 租户 （或跨多个 Azure AD 承租人，对于多租户方案时通过自定义的验证域限定）。 在请求对资源的应用程序，或获取资源应用程序访问令牌的权限时使用它们。 更新此元素时，相应的服务主体 servicePrincipalNames [] 集合，它存在于应用程序的主租户进行相同的更新。

应用程序清单还提供了一个好的方法来跟踪您的应用程序注册的状态。 因为它是以 JSON 格式可用，则可以将文件表示签入您以及您的应用程序源代码的源代码管理。

## <a name="step-by-step-example"></a>逐个步骤示例
现在，让我们更新您的应用程序到应用程序清单的标识配置所需的步骤。 我们将选中一个前面的示例，显示如何声明新的权限范围内对资源应用程序︰

1. 导航到[Azure 的传统门户网站][ AZURE-CLASSIC-PORTAL] ，并使用具有服务管理员或共同管理员特权的帐户登录。

2. 验证之后，向下滚动并选择 Azure"Active Directory"扩展在左侧的导航面板 (1)，然后单击后 Azure AD 租户，其中是您的应用程序注册 (2)。

    ![选择 Azure AD 租户][SELECT-AZURE-AD-TENANT]

3. 当目录页时，单击"应用程序"(1) 若要在组织中注册应用程序的列表，请参阅在网页顶部。 然后，找到您要更新的列表中，单击 (2) 应用的程序。

    ![选择 Azure AD 租户][SELECT-AZURE-AD-APP]

4. 既然选择了应用程序的主页面，请注意底部的页 (1)"管理清单"功能。 如果单击此链接时，将提示您下载或上载的 JSON 清单文件。 单击"下载清单"(2)。 这将紧接着是下载对话框，提示您确认通过单击"下载清单"(3)，则请打开或保存文件的本地 (4)。

    ![管理清单、 下载选项][MANAGE-MANIFEST-DOWNLOAD]

    ![下载清单][DOWNLOAD-MANIFEST]

5. 在此示例中，我们保存文件的本地，使我们可以在编辑器中打开，对 JSON，进行更改并重新保存。 这里是 JSON 结构在 Visual Studio 的 JSON 编辑器内的样子。 注意，它为[应用程序实体]遵循架构[APPLICATION-ENTITY]如我们前面提到︰

    ![更新清单的 JSON][UPDATE-MANIFEST]

    例如，假设我们想要实现/公开对我们资源的应用程序 (API) 调用"Employees.Read.All"的新权限，您将只是新每秒向添加元素 oauth2Permissions 集合，即︰

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    输入项必须是唯一的并因此必须生成新全局唯一 ID (GUID)`"id"`属性。 在这种情况下，由于我们指定`"type": "User"`，此权限可以同意通过 Azure AD 身份验证的任何帐户的租户已注册资源/API 应用程序中。 这将授予客户端应用程序权限的帐户代表对其进行访问。 在同意的情况下和在 Azure 的传统门户网站中显示用于说明和显示名称字符串。

6. 完成更新清单后，返回到 Azure 的广告应用程序页在 Azure 的传统门户网站，然后单击"管理清单"功能再次 (1)。 但这一次，请选择"上载清单"(2)。 下载相似，您将会遇到再次与第二个对话框，提示您指定用于 JSON 文件的位置。 单击"浏览文件..."(3)，然后使用"中选择文件上传至"对话框中选择的 JSON 文件 (4)，然后按"打开"。 对话框将消失，一旦选择"确定"复选标记 (5)，并将上载您的清单。  

    ![管理清单、 上载选项][MANAGE-MANIFEST-UPLOAD]

    ![将上载清单 JSON][UPLOAD-MANIFEST]

    ![将上载清单 JSON 的确认][UPLOAD-MANIFEST-CONFIRM]

现在，保存清单后，您可以注册到我们前面添加新的权限的客户端应用程序访问。 这一次可以使用 Azure 传统门户的 Web 用户界面，而无需编辑客户端应用程序的清单︰  

1. 先转到您要添加对新的 API 访问客户端应用程序的"配置"页，单击"添加应用程序"按钮。
2. 然后您将看到与注册的资源组织中应用程序 (Api) 的列表。 单击加号 / + 资源应用程序的名称以选中它旁边的符号。  
3. 然后单击右下角中的复选标记。
4. 当您返回到客户端的配置页的"添加应用程序"部分时，您将看到新的资源应用程序列表中。 如果鼠标悬停在该行右侧的"授予权限"部分中，您将看到显示的下拉列表。 单击列表，然后选择新的权限，才能将其添加到客户端的请求的权限的列表。 注意︰ 将在客户端应用程序的标识配置中，"requiredResourceAccess"的集合属性中存储此新权限。

![向其他应用程序的权限][PERMS-TO-OTHER-APPS]

![向其他应用程序的权限][PERMS-SELECT-APP]

![向其他应用程序的权限][PERMS-SELECT-PERMS]

就是这样。 现在，您的应用程序将使用其新身份配置来运行的。

## <a name="next-steps"></a>下一步行动
- 有关应用程序的应用程序和服务主体对象之间的关系的详细信息，请参阅[应用程序和服务在 Azure 广告的主体对象][AAD-APP-OBJECTS]。
- 请参阅[AD Azure 开发术语表][AAD-DEVELOPER-GLOSSARY]的一些核心 Azure 活动目录 (AD) 开发概念的定义。

请使用下面的 DISQUS 评论部分提供反馈，帮助我们改进和形状我们的内容。

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

