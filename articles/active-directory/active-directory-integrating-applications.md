<properties
   pageTitle="与 Azure Active Directory 集成应用程序 |Microsoft Azure"
   description="详细介绍了如何添加，更新，或在 Azure 活动目录 (AD Azure) 中删除应用程序。"
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/29/2016"
   ms.author="mbaldwin;bryanla" />

# <a name="integrating-applications-with-azure-active-directory"></a>与 Azure Active Directory 集成应用程序

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

企业开发人员和软件作为服务 (SaaS) 提供商可以开发商业云服务或业务线应用程序，可以集成在一起，Azure 活动目录 (AD Azure)，为其服务提供安全登录和授权。 将与 Azure AD 集成应用程序或服务，开发人员必须先注册他们的应用程序有关的详细信息使用 Azure 通过 Azure 的传统门户网站的广告。

这篇文章演示了如何添加、 更新或在 Azure AD 中删除应用程序。 您将了解不同类型的应用程序可以集成在一起，Azure 的广告，如何配置您的应用程序访问其他资源，如 web Api，等等。

两个表示已注册的应用程序以及它们之间的关系的 Azure 的 AD 对象有关的详细信息，请参阅[应用程序对象和服务主体对象](active-directory-application-objects.md);若要了解有关使用 Azure Active Directory 开发应用程序时应使用的品牌指导，请参见[品牌集成应用程序的准则](active-directory-branding-guidelines.md)。

## <a name="adding-an-application"></a>添加应用程序

任何想要使用 Azure 广告的功能的应用程序必须首先注册在 Azure AD 租户。 注册过程涉及到提供有关应用程序中，如有位置之后用户进行身份验证，, 发送答复的 URL 的 URL 的 Azure 广告详细信息的 URI 标识的应用程序，等等。

如果您正在构建的 web 应用程序只需要支持登录的用户在 Azure 的广告中，您可以只需按照下面的说明进行操作。 如果您的应用程序都需要凭据或 web API 访问权限或需要允许用户从其他 Azure AD 承租人来访问它，请参阅[更新应用程序](#updating-an-application)部分，若要继续配置您的应用程序。

### <a name="to-register-a-new-application-in-the-azure-classic-portal"></a>在 Azure 的传统门户网站注册一个新应用程序

1. 登录到[Azure 的传统门户网站](https://manage.windowsazure.com)。

1. 单击左侧菜单上的 Active Directory 图标，然后单击所需的目录上。

1. 在菜单上，单击**应用程序**。 如果没有应用程序已添加到您的目录，此页将只显示添加应用程序链接。 单击该链接，或者您可以单击命令栏上的**添加**按钮。

1. 在要执行页上，单击添加**我的公司正在开发的应用程序**的链接。

1. 在告诉我们有关您的应用程序页，必须指定您的应用程序，同时指示注册 Azure AD 的应用程序的类型的名称。  您可以从任何一个[web 应用程序客户端](active-directory-dev-glossary.md#client-application) / （可能还充当两者）[web 资源/API](active-directory-dev-glossary.md#resource-server)应用程序或[本机客户端](active-directory-dev-glossary.md#native-client)应用程序。 完成后，单击在页面的右下角的箭头图标。

1. 在应用程序属性页上提供的登录 URL 和应用程序 ID URI 如果正在注册 web 应用程序或只重定向 URI 的本机客户端应用程序，然后单击页面底部右下角中的复选框。

1. 已添加您的应用程序，并且您将被带到快速启动页为您的应用程序。 取决于您的应用程序是 web 或本机应用程序，您将看到如何将其他功能添加到您的应用程序的不同选项。 您的应用程序之后，可以开始更新您的应用程序，以使用户可以在其他应用程序中访问 web Api 签名或将多租户应用程序 （它允许其他组织来访问您的应用程序） 配置。

>[AZURE.NOTE] 默认情况下，新创建的应用程序注册配置为允许从您的目录的用户可以登录到您的应用程序。

## <a name="updating-an-application"></a>更新应用程序

一旦您的应用程序中注册 Azure 的广告，它可能需要更新，以提供对 web Api 的访问，可在其他组织，以及其他。 本节描述您可能需要配置更多应用程序的各种方式。 首先我们将开始概括的同意框架，即一定要了解您在构建资源/API 的应用程序将使用由开发人员在您的组织或其他组织生成的客户端应用程序。

在 Azure AD 身份验证工作的方式的详细信息，请参阅[Azure AD 身份验证方案](active-directory-authentication-scenarios.md)。

### <a name="overview-of-the-consent-framework"></a>同意框架概述

Azure 广告许可框架便于开发 web 的多租户和本机客户端应用程序需要访问 web Azure AD 租户，不同于客户端应用程序未受保护的 Api。 这些 web Api 包括图形 API，Office 365 和其他 Microsoft 服务，除了您自己的 web Api。 该框架基于用户或管理员同意的情况下让应用程序要求在它们可能涉及访问目录数据的目录中注册。

例如，如果 web 客户端应用程序需要调用一个 Office 365 的 web API/资源应用程序读取用户的日历信息，则该用户将需要同意客户端应用程序。 得到同意后，客户端应用程序将能够代表用户，Office 365 web API 调用并使用根据需要将日历信息。

同意框架是建立在 OAuth 2.0 以及其各种流程，如授予授权代码授权和客户端凭据，使用公用或机密的客户端。 通过使用 OAuth 2.0，Azure 广告便可以生成多种不同类型的客户端应用程序，如电话、 平板电脑、 服务器或 web 应用程序，并获得对所需的资源的访问。

同意框架有关的更多详细信息，请参阅[OAuth 2.0 在 Azure 广告](https://msdn.microsoft.com/library/azure/dn645545.aspx)、 [Azure AD 身份验证方案](active-directory-authentication-scenarios.md)和 Office 365 主题[理解与 Office 365 Api 的身份验证](https://msdn.microsoft.com/office/office365/howto/common-app-authentication-tasks)。

#### <a name="example-of-the-consent-experience"></a>同意的情况下体验的示例

下面的步骤将演示如何同意遇到应用程序开发人员和用户的工作。

1. 在 Azure 的传统门户网站中的 web 客户端应用程序的配置页面，设置应用程序所需权限的应用程序中的其他控件中使用下拉式菜单的权限。

    ![向其他应用程序的权限](./media/active-directory-integrating-applications/permissions.png)

1. 考虑到已更新应用程序的权限，该应用程序正在运行，并即将第一次使用它的用户。 如果应用程序没有已经获得访问权限或刷新令牌，则应用程序需要转到 Azure 广告的授权终结点，以获得授权码可以用来获得新的访问权限并刷新令牌。

1. 如果不已验证用户，系统将要求他们登录到 Azure 的广告。

    ![用户或管理员登录到 Azure 的广告](./media/active-directory-integrating-applications/useradminsignin.png)

1. 用户已登录后，用户是否需要显示的同意的情况下页决定 Azure 的广告。 此决定取决于是否在用户 （或其组织的管理员） 已授予应用程序同意。 如果尚未授予同意的情况下，Azure 的广告将会提示用户同意，并且将显示它需要正常工作所需的权限。 同意的情况下对话框中显示的权限集是 Azure 的传统门户网站中的其他应用程序控件的权限中所选内容相同。

    ![用户同意的情况下体验](./media/active-directory-integrating-applications/userconsent.png)

1. 用户授权同意后，授权代码返回到您的应用程序，可以兑换要获得访问令牌并刷新令牌。 有关此流程的详细信息，请参阅[Azure AD 身份验证方案](active-directory-authentication-scenarios.md)中的[web 应用程序的 web API 一节](active-directory-authentication-scenarios.md#web-application-to-web-api)一节。

### <a name="configuring-a-client-application-to-access-web-apis"></a>配置客户端应用程序来访问 web Api

为了使 web/机密的客户端应用程序能够参与要求身份验证授权授予流 （并获取一个访问令牌），它必须建立安全凭据。 支持的 Azure 门户的默认身份验证方法为客户端 ID + 对称密钥。 本节将介绍提供您的客户端凭据的机密密钥所需的配置步骤。

此外之前客户端可以, 访问 web API 所公开的资源应用程序 (例如︰ Azure 广告图形 API)，同意框架将确保客户获得所需要的权限授予基于请求的权限。 默认情况下，所有的应用程序可以从 Azure Active Directory (图形 API) 和 Azure 服务管理 API，选择权限，使用 Azure 广告"启用登录和读取的用户配置文件"权限，默认情况下已选中。 如果在 Office 365 Azure AD 租户中注册客户端应用程序，web Api 和 SharePoint 和 Exchange Online 的权限也将可供选择。 您可以选择所需的 web API 旁边的下拉列表菜单中的[两种类型的权限](active-directory-dev-glossary.md#permissions)︰

- 应用程序权限︰ 客户端应用程序需要访问 web API 直接为本身 （用户上下文）。 这种类型的权限需要管理员同意的情况下，还不能用于本机客户端应用程序。

- 委派权限︰ 客户端应用程序需要访问 web API 为已登录的用户，但受限于所选权限的访问。 除非权限配置为需要管理员同意的情况下，可以将此类型的权限授予用户。

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>若要添加凭据或访问 web Api 权限

1. 登录到[Azure 经典门户。](https://manage.windowsazure.com)

1. 单击左侧菜单上的 Active Directory 图标，然后单击所需的目录。

1. 在顶部的菜单中，单击**应用程序**，然后单击您要配置的应用程序。 快速启动页将显示单一登录和其他配置信息。 单击顶部的页将您应用程序的配置页面的**配置**链接。

1. 若要添加 web 应用程序凭据密钥，请向下滚动到"密钥"一节。  

    - 第一次单击"选定时间"下拉列表，然后选择 1 或 2 年的工期。 
    - 然后您将看到一个新行，用"有效期自"和"过期"日期填写。
    - 最右边的列将包含键值之后保存配置更改 （下图）。 一定要回到本部分并将其复制后击中保存，以便在客户端应用程序在运行时的身份验证期间会有使用它。

2. 若要添加权限才能从您的客户端访问资源的 Api，向下滚动到"到其他应用程序的权限"部分。 
    - 首先单击"添加应用程序"按钮。
    - 使用"显示"下拉列表中选择要从的资源类型。
    - 第一列可以从公开网站 API 可用的资源应用在您的目录中进行选择。 单击您感兴趣，该资源，然后单击右下角中的复选标记。
    - 选择后，您将看到添加到"其他应用程序的权限"列表中的资源。
    - 使用"应用程序权限"和"委派权限"下拉列表，选择您的客户端应用程序所需的权限。

1. 完成后，单击命令栏上的**保存**按钮。 如果为应用程序指定的键，它还会生成后单击保存。

>[AZURE.NOTE] 单击保存按钮也会自动基于您配置其他应用程序的权限的目录中设置应用程序的权限。  您可以通过查看应用程序属性选项卡查看这些应用程序的权限。

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>配置公开 web Api 资源应用程序

您可以开发 web API 并使其可用于客户端应用程序通过公开访问[范围](active-directory-dev-glossary.md#scopes)和[角色](active-directory-dev-glossary.md#roles)。 正确配置的 web API 将像其他 Microsoft web Api，包括图形 API 和 Office 365 Api 可用。 通过您的[应用程序的清单](active-directory-dev-glossary.md#application-manifest)，这是表示您的应用程序标识配置一个 JSON 文件公开访问范围和角色。  

下一节将演示如何公开访问作用域，通过修改资源应用程序的清单。

#### <a name="adding-access-scopes-to-your-resource-application"></a>将访问作用域添加到资源应用程序

1. 登录到[Azure 的传统门户网站](https://manage.windowsazure.com)。

1. 单击左侧菜单上的 Active Directory 图标，然后单击所需的目录。

1. 在顶部的菜单中，单击**应用程序**，然后单击您要配置的资源应用程序。 快速启动页将显示单一登录和其他配置信息。

1. 单击命令栏中的**管理清单**按钮，然后选择**下载清单**。

1. 打开 JSON 的应用程序清单文件并替换下面的 JSON 段的"oauth2Permissions"节点。 此段是举例说明如何公开称为"用户模拟"，这使资源所有者向客户端应用程序的一种委派访问资源的范围。 请确保您更改为您自己的应用程序的文本和值︰

        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in   user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],

    Id 值必须使用[GUID 生成工具](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx)创建新生成的 GUID 或以编程方式。 它代表了公开的 web API 权限的唯一标识符。 一旦正确配置您的客户端请求访问您的 web API 和 web API 调用，它将显示具有上面，在这种情况下将 user_impersonation 值设置为该作用域 (scp) 申请 OAuth 2.0 JWT 令牌。

    >[AZURE.NOTE] 其他作用域以后在必要时，可以公开。 请考虑您的 web API 可能会公开与各种不同功能的多个作用域。 现在您可以通过使用范围 (scp) 索赔中接收到的 OAuth 2.0 JWT 令牌控制访问 web API。

1. 保存更新的 JSON 文件并将其上传通过单击命令栏中的**管理清单**按钮，选择**上载清单**，浏览到您已更新的清单文件然后选择它。 一旦上传，web API 现在已配置为可供其他应用程序目录中。

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>若要验证 web API 公开给其他应用程序在您的目录中

1. 在顶部的菜单中，单击**应用程序**，选择您想要配置访问 web API 所需的客户端应用程序，然后单击**配置**。

1. 向下滚动到其他应用程序部分的权限。 选择应用程序下拉菜单中单击，您将能够选择 web API，您只需公开的权限。 从委派权限下拉菜单中，选择新的权限。

![显示任务列表的权限](./media/active-directory-integrating-applications/listpermissions.png)

#### <a name="more-on-the-application-manifest"></a>有关详细信息的应用程序清单
应用程序清单实际上作为一种机制来更新应用程序实体，它定义 Azure AD 应用程序标识配置，包括我们讨论过的 API 访问作用域的所有属性。 应用程序实体的详细信息，请参阅[图形 API 应用程序实体文档](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)。 在它，您会发现用来指定您的 API 权限的应用程序实体成员的完整参考信息︰  

- appRoles 成员，这是可用于定义 web API 的**应用程序权限**的[AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type)实体的集合  
- oauth2Permissions 成员，即[OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type)可用于定义 web API 的**委派权限**的实体的集合

有关应用程序清单的概念一般情况下，请参阅[了解 Azure 活动目录的应用程序清单](active-directory-application-manifest.md)。

### <a name="accessing-the-azure-ad-graph-and-office-365-apis"></a>访问 Azure 广告图形和 Office 365 的 Api

前面已提到，除了公开/访问 Api 对自己资源的应用程序，您还可以更新客户端应用程序访问由 Microsoft 资源公开的 Api。  Azure 广告图形 API，这叫做"Azure Active Directory"到其他应用程序的权限列表中，默认情况下，所有应用程序注册的 Azure 的广告空间。 如果要在 Office 365 通过设置 Azure AD 租户注册客户端应用程序，您还可以访问所有公开的 Api 对各种 Office 365 提供资源的权限。

有关访问作用域由公开的完整讨论︰  

- Azure 广告图形 API，请参阅[权限范围 |图形 API 概念](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)文章。
- Office 365 的 Api，请参阅[身份验证和授权使用通用同意框架](https://msdn.microsoft.com/office/office365/howto/application-manifest)的文章。 有关如何生成客户端应用程序与 Office 365 的 Api 集成的大讨论，请参阅[设置 Office 365 开发环境](https://msdn.microsoft.com/office/office365/HowTo/setup-development-environment)。

>[AZURE.NOTE] 由于当前的限制，本机客户端应用程序只能调用到 Azure 广告图形 API 如果他们使用"访问您的组织目录"权限。  此限制不适用于 web 应用程序中。

### <a name="configuring-multi-tenant-applications"></a>配置多租户应用程序

在添加到 Azure AD 的应用程序时，您可能希望仅由您的组织中的用户访问您的应用程序。 或者，您可能希望您的应用程序要访问的外部组织中的用户。 这些两个应用程序类型称为单租户和多租户应用程序。 您可以修改单个租户应用程序使其下方的此部分讨论的多租户应用程序的配置。

请务必注意单个租户和多租户应用程序之间的区别︰  

- 单个客户端应用程序用于一个组织。 它们通常是由企业开发人员编写的业务线 (LoB) 应用程序。 单个租户应用程序只需要在一个目录中，用户访问，因此，它只需要设置一个目录中。
- 许多组织在使用一个多租户应用程序。 它们是软件作为服务 (SaaS) web 应用程序通常由独立软件供应商 (ISV) 编写的。 多租户应用程序需要设置，它们将被使用，这就需要将它们通过 Azure 广告许可框架支持注册用户或管理员同意每个目录中。 请注意，所有本机客户端应用程序默认情况下的多租户安装资源所有者的设备上。 请参阅许可框架同意框架上一节中的详细概述。

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>启用外部用户可以授予您对其资源的应用程序访问

如果您正在编写您想要向您的客户或组织外部的合作伙伴提供的应用程序，您需要更新应用程序定义 Azure 的经典门户中。

>[AZURE.NOTE] 当启用多租户，则必须确保已验证域属于应用程序的应用程序 ID URI。 此外，返回的 URL 必须以 https:// 开头。 有关详细信息，请参阅[应用程序对象和服务主体对象](active-directory-application-objects.md)。

若要启用对外部用户访问您的应用程序︰ 

1. 登录到[Azure 经典门户。](https://manage.windowsazure.com)

1. 单击左侧菜单上的 Active Directory 图标，然后单击所需的目录。

1. 在顶部的菜单中，单击**应用程序**，然后单击您要配置的应用程序。 快速启动页将显示配置选项。

1. 展开**配置的多租户应用程序**部分的快速启动，然后单击**立即配置**部分中的链接启用访问。 将显示应用程序属性页。

1. 单击**是**按钮应用程序是多租户，然后单击命令栏上的**保存**按钮。

一旦有进行上述更改，用户和其他组织中的管理员将能够授予您对其目录和其他数据的应用程序访问权限。

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>触发 Azure 广告许可框架在运行时

若要使用许可框架，多租户客户端应用程序必须请求授权使用 OAuth 2.0。 [代码示例](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant)将向您展示如何 web 应用程序、 本机应用程序或守护程序服务器/应用程序请求授权码和调用 web Api 的访问令牌。

Web 应用程序可能也提供注册为用户体验。 如果提供了注册的经验，预计用户将单击注册按钮，将重定向到 Azure AD OAuth2.0 浏览器授权或用户信息 OpenID 连接终结点的终结点。 这些终结点允许应用程序通过检查 id_token 中获取有关新用户的信息。 在注册阶段用户将看到与同意的情况下提示类似于以上所示的同意框架部分的概述中。

或者，您的 web 应用程序可能还会提供一种体验，使管理员能够"注册我的公司"。 这种体验会还将用户重定向到 Azure AD OAuth 2.0 授权的终结点。 在这种情况下，通过在提示符下 = admin_consent 参数到授权的终结点来强制管理员同意的情况下体验，其中管理员将授予代表他们的组织的同意。 只有属于全局管理员角色的帐户通过身份验证的用户才可以提供的同意;其他人会收到一个错误。 在成功同意的情况下，响应将包含 admin_consent = true。 当获取一个访问令牌，您还会收到 id_token，在该组织提供的信息并注册您的应用程序的管理员。

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>启用 OAuth 2.0 隐式授予的单页面应用程序

单页面应用程序 (SPAs) 通常的结构与运行在浏览器中重新调用应用程序的 web API 结束执行其业务逻辑的大量 JavaScript 的前端。 对于 SPAs 承载在 Azure 的广告中，您将使用 OAuth 2.0 隐式授予 Azure 广告与用户进行身份验证并获取一个标记，可用于从应用程序的 JavaScript API 及其后端 web 客户端安全呼叫。 授予用户同意后，可以使用此相同的身份验证协议来获取令牌来保护客户端和其他站点之间的调用 API 为应用程序配置的资源。 若要了解更多有关隐式授权授予，并帮助您确定它是否适合您的应用程序方案，请参见[了解 Azure Active Directory 中的 OAuth2 隐式授予流](active-directory-dev-understanding-oauth2-implicit-grant.md)。

默认情况下，OAuth 2.0 隐式授予被禁用的应用程序。 可以通过设置为应用程序启用 OAuth 2.0 隐式授予`oauth2AllowImplicitFlow`"在其[应用程序清单](active-directory-application-manifest.md)，则表示您的应用程序标识配置一个 JSON 文件中的值。

#### <a name="to-enable-oauth-20-implicit-grant"></a>若要启用 OAuth 2.0 隐式授予 

1. 登录到[Azure 经典门户。](https://manage.windowsazure.com)
1. 单击左侧菜单上的**Active Directory**图标，然后单击所需的目录。
1. 在顶部的菜单中，单击**应用程序**，然后单击您要配置的应用程序。 快速启动页将显示单一登录和其他配置信息。
1. 单击命令栏中的**管理清单**按钮，然后选择**下载清单**。
打开 JSON 的应用程序清单文件并设置为"true"的"oauth2AllowImplicitFlow"值。 默认情况下，它是"假"。

    `"oauth2AllowImplicitFlow": true,`

1. 保存更新的 JSON 文件并将其上传通过单击命令栏中的**管理清单**按钮，选择**上载清单**，浏览到您已更新的清单文件然后选择它。 一旦上传，您的 web API 现在配置为使用 OAuth 2.0 隐式授予用户进行身份验证。


### <a name="legacy-experiences-for-granting-access"></a>要为其授予访问权限的传统经验

本部分介绍在 3 月 12，2014年前的旧式同意体验。 这种体验仍然支持，如下所述。 之前的新功能，可以只授予以下权限︰

- 其组织中的用户登录

- 用户登录并阅读其组织的目录数据 （作为应用程序仅）

- 登录的用户读取和写入其组织的目录数据 （如应用程序只）

您可以按照中[开发多租户 Web 应用程序使用 Azure AD](https://msdn.microsoft.com/library/azure/dn151789.aspx)授予注册 Azure AD 中的新应用程序的访问权限的步骤。 请务必注意新同意框架允许更强大的应用程序，并且还使用户能够同意这些应用程序，而不是只是管理员。

#### <a name="building-the-link-that-grants-access-for-external-users-legacy"></a>生成链接，授予访问权限的外部用户 （传统）

为了使外部用户可以注册您的应用程序使用其组织的帐户，您需要更新您的应用程序，以使他们授予访问权限的 Azure 广告上显示按钮链接到的网页。 署名为此注册按钮指南[品牌集成的应用程序的指南](active-directory-branding-guidelines.md)主题中讨论。 用户授予或拒绝的访问后，Azure AD 授予访问页将重定向浏览器返回到您的应用程序的响应。 有关应用程序属性的详细信息，请参阅[应用程序对象和服务原则](active-directory-application-objects.md)。

授权访问页创建的 Azure 的广告，并可以在 Azure 的传统门户应用程序的配置页面中找到指向它的链接。 若要转到配置页，单击 Azure AD 租户的顶级菜单中该**应用程序**链接，单击您想要配置，然后单击快速启动页的顶部菜单中**配置**该应用的程序。

您的应用程序的链接如下所示︰ `http://account.activedirectory.windowsazure.com/Consent.aspx?ClientID=058eb9b2-4f49-4850-9b78-469e3176e247&RequestedPermissions=DirectoryReaders&ConsentReturnURL=https%3A%2F%2Fadatum.com%2FExpenseReport.aspx%3FContextId%3D123456`。 下表描述了链接的组成部分︰

|参数|说明|
|---|---|
|客户机 Id|必填。 作为添加您的应用程序的一部分获得客户机 ID。|
|RequestedPermissions|可选。 您的应用程序正在请求时，将显示给用户授予您的应用程序的访问权限的访问级别。 如果未指定，请求的访问级别默认为单一登录只。 其他选项为 DirectoryReaders 和 DirectoryWriters。 这些访问级别的更多详细信息，请参阅应用程序的访问级别。|
|ConsentReturnUrl|可选。 您希望授予访问响应返回到该 URL。 此值必须是 URL 编码，并且必须在相同的域配置应用程序定义中的回复 URL。 如果未提供，授予访问响应将被重定向到您配置的回复 URL。|

指定单独的回复 url ConsentReturnUrl 将允许您的应用程序来实现独立的逻辑可以处理 （这通常在处理符号的 SAML 令牌） 的回复 url 不同的 URL 上的响应。 您还可以指定 ConsentReturnURL 中的其他参数的编码 URL;这些将被传递回作为查询字符串参数为您的应用程序重定向时。  可以使用此机制，以维护其他的信息或从 Azure AD 将访问权限授予您的应用程序请求绑定到响应。

#### <a name="grant-access-user-experience-and-response-legacy"></a>授予访问权限的用户体验和响应 （传统）

当应用程序重定向到授予访问链接时，下面的图像演示用户会遇到。

如果没有已登录的用户，他们将会提示您执行此操作︰

![登录到 AAD](./media/active-directory-integrating-applications/signintoaad.png)

对用户进行验证，一旦 Azure 广告将用户重定向到授权访问页︰

![授予访问权限](./media/active-directory-integrating-applications/grantaccess.png)

>[AZURE.NOTE] 只有公司的外部组织的管理员可以授予对您的应用程序的访问。 如果用户不是公司管理员，它们都有以将邮件发送到他们的公司管理员联系以请求此应用程序被授予访问权限选项。

之后客户已通过单击授予访问权限授予您的应用程序的访问权限，或者如果他们有拒绝访问通过单击取消，Azure 广告将响应发送到 ConsentReturnUrl 或配置的回复 URL。 此响应包含以下参数︰

|参数|说明|
|---|---|
|TenantId|已授予您的应用程序使用的 Azure AD 中的组织的唯一 ID。 此参数将只为指定客户是否授予访问权限。|
|同意|值将被设置为授予，如果应用程序被授予访问权限，或拒绝该请求被拒绝，如果。|

附加参数将返回到应用程序中，如果它们被指定为属于 ConsentReturnUrl 编码 URL。 下面是示例响应指示应用程序已被授权，授权访问请求和授予访问请求中包括提供索引号︰ `https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Granted&TenantId=f03dcba3-d693-47ad-9983-011650e64134`。

>[AZURE.NOTE] 访问权限授予响应将不包含安全令牌的用户;应用程序必须登录用户分别。

以下是对被拒绝的访问权限授予请求的示例响应︰`https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Denied`

#### <a name="rolling-app-keys-for-uninterrupted-access-to-the-graph-api-legacy"></a>滚动到图形 API （传统） 的不间断访问的应用程序键

在您的应用程序的生存期内，您可能需要更改到 Azure 的广告来获取访问令牌来调用图形 API 调用时使用的键。  通常更改密钥分为两类︰ 紧急鼠标指针悬停位置您的密钥已泄露或翻转当前密钥时即将到期。 应遵循下面的过程提供无中断地访问您的应用程序; 而刷新您的密钥 （主要为第二个案例）。

1. 在 Azure 经典门户中，单击目录租户，从顶部的菜单中，单击**应用程序**然后单击您要配置的应用程序。 快速启动页将显示单一登录和其他配置信息。

1. 单击顶部的菜单，以便您的应用程序属性的列表，请参阅中的**配置**，您将看到的您的密钥列表。

1. 键下的下拉菜单上单击说**选择持续时间**和选择 1 或 2 年，然后单击**保存**命令栏中。 这将生成您的应用程序的新密码密钥。 复制此新密码密钥。 此时同时现有的和新密钥可由您的应用程序从 Azure 的广告获取一个访问令牌。

1. 要返回到您的应用程序和更新的配置，开始使用新的密码密钥。 万一此更新的示例，请参阅[使用图形 API 来查询 Azure 的广告](https://msdn.microsoft.com/library/azure/dn151791.aspx)。

1. 您应立即推出此更改整个生产环境--跨其余部分之前对其上一个服务节点，首先验证它。

1. 更新完成后您生产的部署，您可以自由地回到 Azure 的传统门户网站，并删除旧密钥。

#### <a name="changing-app-properties-after-enabling-access-legacy"></a>启用访问 （传统） 后更改的应用程序属性

一旦您启用外部用户访问到您的应用程序，您可能仍继续在 Azure 经典门户应用程序的属性进行更改。 但是，客户已授予访问您的应用程序进行应用程序更改之前不会看到反映在 Azure 的经典门户中查看有关您的应用程序的详细信息的更改。 一旦应用程序可供客户，您需要进行某些更改时必须非常小心。 例如，如果您更新了应用程序 ID URI，然后您现有的客户用户授予访问权限，此更改之前将无法登录到您的应用程序使用他们公司或学校的帐户。

如果更改请求使用的应用程序功能，现在利用新的 API 调用使用此增加现有客户的更高的访问级别，RequestedPermissions 访问级别可能会拒绝响应图形 API 访问。  您的应用程序应处理这些情况，并询问客户授予访问权限请求与您的应用程序更多的访问级别。

## <a name="removing-an-application"></a>删除应用程序

本部分介绍如何从 Azure AD 租户删除应用程序。

### <a name="removing-an-application-authored-by-your-organization"></a>删除应用程序由您的组织创作
这些是 Azure AD 租户在主"应用程序"页面上的"应用程序我公司拥有"筛选器下显示的应用程序。 在技术术语中，这些是 Azure 的传统门户网站，通过手动或通过 PowerShell 或图形 API 以编程方式注册的应用程序。 更具体地说，它们表示这两个应用程序和服务主体对象在您的组织中。 有关详细信息，请参阅[应用程序对象和服务主体对象](active-directory-application-objects.md)。

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>从目录中删除单个租户应用程序

1. 登录到[Azure 经典门户。](https://manage.windowsazure.com)

1. 单击左侧菜单上的 Active Directory 图标，然后单击所需的目录上。

1. 在顶部的菜单中，单击**应用程序**，然后单击您要配置的应用程序。 快速启动页将显示单一登录和其他配置信息。

1. 单击命令栏中的**删除**按钮。

1. 在确认消息中单击**是**。

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>若要从目录中删除一个多租户应用程序

1. 登录到[Azure 经典门户。](https://manage.windowsazure.com)

1. 单击左侧菜单上的 Active Directory 图标，然后单击所需的目录上。

1. 在顶部的菜单中，单击**应用程序**，然后单击您要配置的应用程序。 快速启动页将显示单一登录和其他配置信息。

1. 对应用程序是多租户部分中，单击**否**。 这会将转换应用程序可以将单个租户，但应用程序仍会保留在已同意该组织。

1. 单击命令栏中的**删除**按钮。

1. 在确认消息中单击**是**。

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>删除由另一个组织授权的多租户应用程序
这些是在主"应用程序"页上的"应用程序公司使用"筛选器下显示的您 Azure AD 租户，专门在"应用程序我公司拥有"列表未列出的那些应用程序的子集。 用技术术语，它们同意过程中注册的多租户应用程序。 更具体地说，它们表示由一个服务主体对象将在您的组织中。 有关详细信息，请参阅[应用程序对象和服务主体对象](active-directory-application-objects.md)。

要想删除 （在无授予许可） 后多租户应用程序的访问到您的目录，公司管理员必须先 Azure 订阅删除通过 Azure 的传统门户网站的访问权限。 您只需导航到应用程序的配置页面，并单击底部的"管理控制"按钮。 另外，公司管理员可以使用[Azure AD PowerShell Cmdlet](http://go.microsoft.com/fwlink/?LinkId=294151)去访问。

## <a name="next-steps"></a>下一步行动

- 请参见可视化指导为您的应用程序的提示[品牌集成应用程序的准则](active-directory-branding-guidelines.md)。

- 应用程序的应用程序和服务主体对象之间的关系的详细信息，请参阅[应用程序对象和服务主体对象](active-directory-application-objects.md)。

- 若要了解有关该角色的应用程序清单的重头戏，请参阅[了解 Azure 活动目录的应用程序清单](active-directory-application-manifest.md)

- 请参阅有关的一些核心 Azure 活动目录 (AD) 开发概念定义[Azure 广告开发术语表](active-directory-dev-glossary.md)。

- 请访问[Active Directory 开发人员指南 》](active-directory-developers-guide.md)有关的所有开发人员概述相关内容。
