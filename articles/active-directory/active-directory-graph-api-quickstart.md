<properties
   pageTitle="快速入门 Azure 广告图形 API |Microsoft Aure"
   description="Azure 活动目录图形 API Azure 广告通过 OData REST API，终结点到提供编程访问。 应用程序可以使用图形 API 来执行创建、 读取、 更新和删除 (CRUD) 操作目录数据和对象。"
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Azure 广告图形 API 的快速入门

Azure 活动目录 (AD) 图形 API Azure 广告通过 OData REST API，终结点到提供编程访问。 应用程序可以使用图形 API 来执行创建、 读取、 更新和删除 (CRUD) 操作目录数据和对象。 例如，您可以使用图形 API 创建新用户、 查看或更新用户的属性，更改用户的密码、 检查基于角色的访问权限的组成员身份、 禁用或删除该用户。 Graph API 的功能和应用程序方案的详细信息，请参阅[Azure 广告图形 API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)和[Azure 广告图形 API 系统必备组件](https://msdn.microsoft.com/library/hh974476.aspx)。 

> [AZURE.IMPORTANT] Azure 广告图形 API 功能也是通过[Microsoft Graph](https://graph.microsoft.io/)，一个统一的 API，从其他 Microsoft 服务如 Outlook、 OneDrive、 OneNote，计划者和 Office 图表，可通过单个终结点和使用一个访问令牌包括 Api 可用。

## <a name="how-to-construct-a-graph-api-url"></a>如何构建一个图形 API URL

Graph API 中才能访问目录数据和对象 （换言之，资源或实体） 要对其执行 CRUD 操作，您可以使用 Url 基于开放数据 (OData) 协议。 使用图形 API 中的 Url 由四个主要部分组成︰ 服务根、 租户标识符、 资源路径和查询字符串选项︰ `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`。 看下面的 URL 示例︰ `https://graph.windows.net/contoso.com/groups?api-version=1.6`。

- **服务根**︰ 在 Azure 广告图形 API 中，服务的根始终是 https://graph.windows.net。
- **租户标识符**︰ 这可以是一个已验证 （注册） 域名，在上例中，contoso.com。 它也可以是租户对象 ID 或"myorganiztion"我"别名。 有关详细信息，请参阅[解决实体和图形 API 中的操作](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)）。
- **资源路径**︰ 此部分中的 URL 标识的资源 （用户、 组、 特定用户或特定组等） 进行交互在上面的示例中，它是顶级的"组"资源设置的地址。 您还可以解决特定的实体，例如"用户 / {对象 Id}"或"用户范围内"。
- **查询参数**:？ 将从查询参数部分的资源路径部分分隔开来。 Graph API 中的所有请求均都需要"api 版本"查询参数。 Graph API 还支持的以下 OData 查询选项︰ **$filter**、 **$orderby**、 **$expand**， **$top**和**$format**。 当前不支持以下查询选项︰ **$count**、 **$inlinecount**和**$skip**。 有关详细信息，请参阅[支持查询、 筛选和分页 Azure 广告图形 API 中的选项](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options)。

## <a name="graph-api-versions"></a>Graph API 版本

"Api 版本"查询参数中指定的版本的 Graph API 请求。 对于 1.5 或更高版本，请使用数字版本值;api 版本 = 1.6。 对于早期版本中，您使用日期字符串遵循的格式 YYYY-月-日;例如，api 版本 = 2013年-11-08。 预览功能，使用字符串"beta";例如，api 版本 = 测试版。 有关图形 API 版本之间的差异的详细信息，请参阅[Azure 广告图形 API 版本控制](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning)。

## <a name="graph-api-metadata"></a>Graph API 元数据

返回图形 API 的元数据文件、 URL 为示例中的租户标识符后面添加"$metadata"段，以下 URL 返回的元数据使用图形浏览器的演示公司︰ `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`。 您可以在 web 浏览器来查看元数据的地址栏中输入此 URL。 返回的 CSDL 元数据文档描述实体和复杂类型、 它们的属性和函数和公开版本的 Graph API 所请求的操作。 忽略 api 版本参数，将返回元数据的最新版本。

## <a name="common-queries"></a>公共查询

[Azure 广告图形 API 的通用查询](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries)列出了可以使用 Azure 的广告图，包括可用于访问目录中的顶级资源的查询和查询，以在您的目录中执行操作的一般查询。

例如，`https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6`返回公司目录 contoso.com 有关的信息。

或者`https://graph.windows.net/contoso.com/users?api-version=1.6`列出目录 contoso.com 中的所有用户对象。

## <a name="using-the-graph-explorer"></a>使用图形浏览器

图形资源管理器中的 Azure 广告图形 API 可用于查询生成应用程序的目录数据。

> [AZURE.IMPORTANT] 编写或从目录中删除数据图形浏览器不支持。 您只能执行您使用图形浏览器的 Azure 的广告目录的读取的操作。

下面是您将会看到是否都定位到图形资源管理器中，选择使用演示公司，然后输入的输出`https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6`在 demo 目录中显示的所有用户︰

![Azure 广告图形 api 资源管理器](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**加载图形浏览器**︰ 要加载该工具，请定位到[https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/)。 单击**使用演示公司**从示例租户对数据运行图形资源管理器。 不需要凭据，以使用演示公司。 或者，可以单击**登录**和 Azure 的广告帐户凭据登录您的租户对运行图形浏览器。 如果对自己租户运行图形资源管理器，您或您的管理员需要在登录期间同意。 如果您拥有 Office 365 订阅，您自动拥有 Azure AD 租户。 用于登录到 Office 365 的凭据是事实上，Azure 的广告帐户，并使用图形资源管理器中，可以使用这些凭据。

**运行查询**︰ 要运行查询，请在请求文本框中键入查询，并单击**获取**或单击**输入**键。 结果将显示在响应框中。 例如，`https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6`将列出演示目录中的所有组对象。

请注意以下功能和图形浏览器的限制︰
- 在资源上的自动完成功能的设置。 看到这，单击**使用演示公司**，然后单击请求文本框 （其中公司 URL 显示）。 您可以选择从下拉列表中设置的资源。

- 支持"我"和"myorganization"地址别名。 例如，您可以使用`https://graph.windows.net/me?api-version=1.6`返回已登录的用户的用户对象或`https://graph.windows.net/myorganization/users?api-version=1.6`返回当前目录中的所有用户。 请注意，别名使用"我"，则返回错误演示公司因为没有签名在用户发出请求。

- 响应标头部分。 这可以用来帮助解决运行查询时出现的问题。

- 通过展开和折叠功能响应一个 JSON 查看器。

- 不是支持用于显示缩略图的照片。

## <a name="using-fiddler-to-write-to-the-directory"></a>使用 Fiddler 写入到的目录

出于本快速入门指南，您可以使用 Fiddler Web 调试程序为了实践对您执行写操作 Azure 的广告目录。 有关详细信息，以及安装 Fiddler，请参阅[http://www.telerik.com/fiddler](http://www.telerik.com/fiddler)。

在下面的示例中，您将使用 Fiddler Web 调试程序在 Azure 的广告目录中创建新的安全组 MyTestGroup。

**获取一个访问令牌**︰ 若要访问 Azure 广告图，客户端需要成功通过身份验证到 Azure 的广告第一次。 有关详细信息，请参阅[Azure AD 身份验证方案](active-directory-authentication-scenarios.md)。

**撰写和运行查询**︰ 完成以下步骤。

1. 打开 Fiddler Web 调试程序并切换到**编辑器**选项卡。
2. 因为您想要创建一个新的安全组，作为 HTTP 方法中，从下拉菜单中选择**开机自检**。 组对象上的操作和权限的更多信息，请参见[Azure 广告图形 REST API 参考](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)中的[组](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity)。
3. 在**开机自检**旁边的字段中，键入以下内容作为请求 URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`。

    > [AZURE.NOTE] 您必须替换 mytenantdomain Azure 的广告目录的域名。

4. 在正下方开机自检的下拉字段中，键入以下命令︰

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] 替换您&lt;访问令牌&gt;与 Azure 的广告目录的访问令牌。

5. 在**请求正文**字段中，键入以下命令︰

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    有关创建组的详细信息，请参阅[创建组](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup)。

Azure 广告实体和公开的关系图的类型的详细信息以及有关与关系图可以对其执行操作的信息，请参阅[Azure 广告图形 REST API 参考](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)。

## <a name="next-steps"></a>下一步行动

- 了解更多关于[Azure 广告图形 API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- 了解更多有关[Azure 广告图形 API 权限范围](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
