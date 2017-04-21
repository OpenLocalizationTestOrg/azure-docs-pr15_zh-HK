<properties
   pageTitle="Azure 的 Active Directory 图形 API |Microsoft Azure"
   description="图形 API 允许通过编程访问通过 REST API，终结点的 Azure 广告这概述和快速入门指南。"
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure 的 Active Directory 图形 API

> [AZURE.IMPORTANT] Azure 广告图形 API 功能也是通过[Microsoft Graph](https://graph.microsoft.io/)，一个统一的 API，从其他 Microsoft 服务如 Outlook、 OneDrive、 OneNote，计划者和 Office 图表，可通过单个终结点和使用一个访问令牌包括 Api 可用。

Azure 活动目录图形 API Azure 广告通过 REST API，终结点到提供编程访问。 应用程序可以使用图形 API 来执行创建、 读取、 更新和删除 (CRUD) 操作目录数据和对象。 例如，图形 API 支持以下常见操作用户对象︰

- 在目录中创建新用户

- 获取用户的详细的属性，例如它们的组

- 更新用户的属性，例如其位置和电话号码，或更改他们的密码

- 检查基于角色的访问权限的用户的组成员身份

- 禁用的用户帐户，或者完全删除它

除了用户对象，您可以执行类似的操作，如组和应用程序的其他对象。 若要在目录调用图形 API，应用程序必须注册 Azure 的广告，并配置为允许对目录的访问。 这通常被通过用户或管理员同意流。

若要开始使用 Azure 活动目录图形 API，请参阅[图形 API 快速入门指南](active-directory-graph-api-quickstart.md)，或查看[交互式图形 API 参考文档](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)。


## <a name="features"></a>功能

该图形 API 提供了以下功能︰

- **REST API 端点**︰ 图形 API 是使用标准的 HTTP 请求来访问的终结点所组成的 RESTful 服务。 Graph API 支持的请求和响应的 XML 或 Javascript 对象符号 (JSON) 内容类型。 有关详细信息，请参阅[Azure 广告图形 REST API 参考](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)。

- **使用 Azure AD 身份验证**︰ 图形 API 为每个请求必须通过追加 JSON Web 标记 (JWT) 请求授权标头中进行身份验证。 此标记会获取通过 Azure AD 标记终结点向发出请求并提供有效的凭据。 您可以使用 OAuth 2.0 客户端凭据流或授权代码授予流以获取令牌来调用关系图。 有关详细信息，[在 Azure 广告 OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)。

- **基于角色的授权 (RBAC)**: 使用安全组来执行图形 API 中的 RBAC。 例如，如果您需要确定用户是否有权访问特定的资源，应用程序可以调用[检查组成员身份 （传递）](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive)操作，返回 true 或 false。

- **差异的查询**︰ 如果您想要检查两个时间段，而无需频繁查询图形 API 之间的某个目录中的更改，您可以进行差异的查询请求。 这种类型的请求将返回只上一个差异的查询请求与当前请求之间所做的更改。 有关详细信息，请参阅[Azure 广告图形 API 差异查询](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query)。

- **目录扩展**︰ 如果您要开发的应用程序需要读取或写入的目录对象的唯一属性，可以注册，并通过图形 API 使用扩展值。 例如，如果您的应用程序需要为每个用户的 Skype ID 属性，您可以在目录中注册新的属性，将在每个用户对象上可用。 有关详细信息，请参阅[Azure 广告图形 API 目录架构扩展](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)。

- **通过权限范围的安全**︰ AAD 图形 API 将权限范围，使安全许可 AAD 数据访问和支持各种客户端应用程序类型，包括︰
 - 通过用户界面授予的访问权限委托给通过授权数据中已登录的用户 （委派）
  - 那些使用应用程序的定义基于角色的访问控制，例如客户端服务/守护程序 （应用程序角色）

    同时委派和应用程序角色的权限范围表示公开的图形 API 的权限，可以通过应用程序注册权限[Azure 的传统门户网站中的新功能](https://manage.windowsazure.com)的客户端应用程序请求。 客户端可以验证权限范围授予它们通过检查委派权限的访问令牌中接收到该作用域 ("scp") 申请和报销应用程序角色的权限的角色 （"角色"）。 了解有关[Azure 广告图形 API 权限范围](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)。


## <a name="scenarios"></a>方案

该图形 API，许多应用程序方案。 以下是最常见的︰

- **业务线 （单个租户） 应用程序**︰ 在这种情况下，企业级开发版适用于有 Office 365 订阅的组织。 开发人员正在构建的 web 应用程序交互使用 Azure 的广告执行任务这样将许可证分配给用户。 此任务需要访问图形 API，使开发人员注册单租户在 Azure 广告中的应用程序并配置读取和写入图形 api 权限。 然后应用程序被配置为使用自己的凭据或当前登录的用户来获取令牌来调用图形 API。

- **软件即服务应用程序 （多租户）**︰ 在这种情况下，独立软件供应商 (ISV) 开发承载的 web 的多租户应用程序的其他组织使用 Azure 的广告，提供用户管理功能。 这些功能需要对目录对象的访问，所以应用程序需要调用图形 API。 开发人员在 Azure AD 中注册应用程序、 配置它需要读取和写入权限的图形 API，然后允许外部访问，以便其他组织可同意在其目录中使用该应用程序。 在另一个组织内的用户进行身份验证，到第一次应用程序，它们将显示应用程序请求的权限与同意的情况下进行对话。  这些授权同意的情况下将应用程序请求的权限与图形 API 用户的目录中。 同意框架的详细信息，请参阅[同意框架的概述](active-directory-integrating-applications.md)。

## <a name="see-also"></a>请参见

[Azure 广告图形 API 快速入门指南](active-directory-graph-api-quickstart.md)

[广告图形其他文档](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory 开发人员指南 》](active-directory-developers-guide.md)
