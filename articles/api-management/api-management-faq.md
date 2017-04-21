<properties
    pageTitle="Azure API 管理常见问题 |Microsoft Azure"
    description="了解常见的问题、 模式以及在 Azure API 管理最佳做法的答案。"
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure API 管理常见问题解答

解答常见的问题、 模式和最佳做法获得 Azure API 管理。

## <a name="frequently-asked-questions"></a>常见问题及的解答

-   [如何可以是否 Microsoft Azure API 管理团队提出问题？](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [在预览功能时，它意味着什么？](#what-does-it-mean-when-a-feature-is-in-preview)
-   [如何确保安全 API 管理网关和我的后端服务之间的连接？](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [如何将我的 API 管理服务实例复制到新实例？](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [可以以编程方式管理我的 API 管理实例？](#can-i-manage-my-api-management-instance-programmatically)
-   [如何将用户添加到管理员组？](#how-do-i-add-a-user-to-the-administrators-group)
-   [原因是我想要添加在策略编辑器中不可用的策略？](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [如何在 API 管理中使用 API 版本控制？](#how-do-i-use-api-versioning-in-api-management)
-   [如何设置多个环境中单个 API？](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [可以使用 API 管理使用 SOAP 吗？](#can-i-use-soap-with-api-management)
-   [是该管理 API 网关 IP 地址常量？我是否可以使用它在防火墙规则？](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [可以使用 AD FS 安全配置 OAuth 2.0 授权服务器？](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [API 管理部署到多个地理位置中使用哪种路由方法？](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [可以使用 Azure 资源管理器模板来创建 API 管理服务实例？](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [可以为后端使用自行签署式 SSL 证书吗？](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [为什么当我尝试克隆一个 GIT 存储库获取身份验证失败？](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [API 管理是否使用 Azure ExpressRoute？](#does-api-management-work-with-azure-expressroute)
-   [移动 API 管理服务从一个订阅到另一个？](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>如何可以是否 Microsoft Azure API 管理团队提出问题？

您可以联系我们通过使用以下选项之一︰

-   在我们的[API 管理 MSDN 论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt)中发布您的问题。
-   发送电子邮件至<apimgmt@microsoft.com>。
-   向我们发送请求功能在[Azure 反馈论坛](https://feedback.azure.com/forums/248703-api-management)。

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>在预览功能时，它意味着什么？

在预览功能时，它意味着我们正在积极寻求反馈功能为您的工作方式。 在预览功能已具备所有功能，但可能我们将进行更改以响应客户反馈重大。 我们建议您不依赖于在生产环境中的预览功能。 如果您有任何反馈的预览功能，请让我们知道通过中的联系人选项之一[如何可以我提问的 Microsoft Azure API 管理团队？](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)。

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>如何确保安全 API 管理网关和我的后端服务之间的连接？

有几种方法可以保证 API 管理网关和后端服务之间的连接。 您可以：

-   使用 HTTP 基本身份验证。 有关详细信息，请参阅[配置 API 设置](api-management-howto-create-apis.md#configure-api-settings)。
- 使用 SSL 相互身份验证，[如何来保护后端服务使用 Azure API 管理中的客户端证书身份验证](api-management-howto-mutual-certificates.md)中所述。
- 在后端服务使用 IP 添加到白名单。 如果您有标准或特优层 API 管理实例，网关的 IP 地址保持不变。 您可以设置允许该 IP 地址白名单。 在 Azure 门户中的仪表板上，可以获得 API 管理实例的 IP 地址。
- 连接到 Azure 虚拟网络 API 管理实例。 有关详细信息，请参阅[如何设置 Azure API 管理中的 VPN 连接](api-management-howto-setup-vpn.md)。

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>如何将我的 API 管理服务实例复制到新实例？

如果您想要将 API 管理实例复制到一个新的实例，可以有多种选项。 您可以：

-   使用备份和恢复管理 API 中的函数。 有关详细信息，请参阅[如何实现灾难恢复，通过服务备份和还原在 Azure API 管理](api-management-howto-disaster-recovery-backup-restore.md)。
-   创建您自己的备份和恢复使用[REST API，API 管理](https://msdn.microsoft.com/library/azure/dn776326.aspx)功能。 使用 REST API 来保存和还原所需的服务实例的实体。
-   使用 Git，下载服务配置，然后将其上传到一个新的实例。 有关详细信息，请参阅[如何配置您使用 Git 的 API 管理服务配置并保存](api-management-configuration-repository-git.md)。

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>可以以编程方式管理我的 API 管理实例？

是的可以通过以编程方式管理 API 管理︰

-   [API 管理 REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx)。
-   [Microsoft Azure ApiManagement 服务管理库 SDK](http://aka.ms/apimsdk)。
-   [服务部署](https://msdn.microsoft.com/library/mt619282.aspx)和[服务管理](https://msdn.microsoft.com/library/mt613507.aspx)PowerShell 的 cmdlet。

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>如何将用户添加到管理员组？

下面是如何将用户添加到管理员组:

1. 登录到[Azure 的门户](https://portal.azure.com)。
2. 转到具有要更新的 API 管理实例的资源组。
3. 在 API 管理分配给用户的**Api 管理参与者**角色。

现在，新添加的参与者可以使用 Azure PowerShell [cmdlet](https://msdn.microsoft.com/library/mt613507.aspx)。 下面介绍了如何以管理员身份登录︰

1. 使用`Login-AzureRmAccount`用于登录。
2. 将上下文设置为使用具有该服务的订阅`Set-AzureRmContext -SubscriptionID <subscriptionGUID>`。
3. 通过使用获得的单一登录 URL `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`。
4. 使用 URL 访问管理门户。


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>原因是我想要添加在策略编辑器中不可用的策略？

如果您想要添加的策略会出现变灰或着色策略编辑器中，确保您处于正确的范围内的策略。 每个策略语句适用于要使用的特定作用域和策略部分。 要查看策略部分和策略的作用域，请参阅[API 管理策略](https://msdn.microsoft.com/library/azure/dn894080.aspx)中的策略的使用部分。


### <a name="how-do-i-use-api-versioning-in-api-management"></a>如何在 API 管理中使用 API 版本控制？

您有几种方法可以用 API 管理 API 版本控制︰

-   在 API 管理中，您可以配置 Api 来表示不同的版本。 例如，您可能有两个不同的 Api、 MyAPIv1 和 MyAPIv2。 开发人员可以选择开发人员想要使用的版本。
-   此外可以使用不包括版本段，例如，https://my.api 的服务 URL 来配置您的 API。 然后，在每个操作的[重写 URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL)模板上配置版本段。 例如，您可以操作使用一个称为 /resource 和一个称为 /v1/Resource 的[重写 URL](api-management-howto-add-operations.md#rewrite-url-template)模板的[URL 模板](api-management-howto-add-operations.md#url-template)。 您可以更改版本段值分别为每个操作。
-   如果您想要将"默认"版本段保留在 API 的服务 URL，在选定的操作，设置使用的[后端服务设置](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService)的策略来更改后端请求路径的策略。

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>如何设置多个环境中单个 API？

若要设置多个环境，例如，测试环境和生产环境中，在一个单一的 API，您可以有两种选择。 您可以：

-   在同一个租户主机不同的 Api。
-   主机上不同租户使用相同的 Api。

### <a name="can-i-use-soap-with-api-management"></a>可以使用 API 管理使用 SOAP 吗？

现在提供了[SOAP 传递](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/)支持。 管理员可以将他们的 SOAP 服务的 WSDL 导入和 Azure API 管理将创建 SOAP 的前端。 开发人员门户文档、 测试控制台、 策略和分析都可用 SOAP 服务。

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>是该管理 API 网关 IP 地址常量？ 我是否可以使用它在防火墙规则？

在标准和特优层中，API 管理租户的公共 IP 地址 (VIP) 是静态的租户，但出现一些异常的生存期。 在这些情况下的 IP 地址更改︰

-   删除并重新创建该服务。
-   服务预订是挂起 （例如，对于 nonpayment)，然后恢复。
-   添加或删除 Azure 虚拟网络 （可以只在特优层使用虚拟网络）。

针对多区域部署区域地址更改如果区域是空出，然后恢复 （您可以仅在高级层使用多区域部署）。

多区域部署配置的特优层租户分配每个地区的一个公共 IP 地址。

Azure 门户中的租户页上，您可以获得您的 IP 地址 （或地址，在多区域部署）。

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>可以使用 AD FS 安全配置 OAuth 2.0 授权服务器？

若要了解如何使用 Active Directory 联合身份验证服务 (AD FS) 安全配置 OAuth 2.0 授权服务器，请参阅[API 管理中使用 ADF](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/)。

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>API 管理部署到多个地理位置中使用哪种路由方法？

API 管理部署到多个地理位置中使用[性能通信路由方法](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method)。 传入的通信被路由到最接近的 API 网关。 如果一个地区离线，传入通信自动路由到下一个最接近的网关。 了解路由的方法，在[通信管理器路由方法](../traffic-manager/traffic-manager-routing-methods.md)。

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>可以使用 Azure 资源管理器模板来创建 API 管理服务实例？

是的。 请参阅[Azure API 管理服务](http://aka.ms/apimtemplate)快速启动模板。

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>可以为后端使用自行签署式 SSL 证书吗？

是的。 下面介绍了如何对后端使用自签名的安全套接字层 (SSL) 证书︰

1. 通过使用 API 管理创建一个[后端](https://msdn.microsoft.com/library/azure/dn935030.aspx)的实体。
2. 将**skipCertificateChainValidation**属性设置为**true**。
3. 如果您不再希望允许自签名的证书，删除后端实体，或将**skipCertificateChainValidation**属性设置为**false**。

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>为什么当我尝试克隆一个 Git 存储库获取身份验证失败？

如果您使用 Git 凭据管理器，或者如果您正在尝试使用 Visual Studio 克隆一个 Git 存储库中，您可能会遇到 Windows 凭据对话框中的已知问题。 对话框中限制密码长度为 127 个字符，并且它将截断该 Microsoft 生成密码。 我们正在缩短密码。 现在，请使用 Git Bash 克隆 Git 存储库。

### <a name="does-api-management-work-with-azure-expressroute"></a>API 管理是否使用 Azure ExpressRoute？

是的。 API 管理 Azure ExpressRoute 与协同工作。

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>移动 API 管理服务从一个订阅到另一个？

是的。 若要了解如何，请参阅[移动到新的资源组或预订的资源](../resource-group-move-resources.md)。
