<properties
    pageTitle="如何保护 Web API 使用 Azure Active Directory 和 API 管理后端"
    description="了解如何保护 Web API 使用 Azure Active Directory 和 API 管理后的端。" 
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

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>如何保护 Web API 使用 Azure Active Directory 和 API 管理后端

下面的视频显示了如何生成 Web API 后端和使用 Azure Active Directory 和 API 管理使用 OAuth 2.0 协议进行保护。  本文提供了一个概述和视频中的步骤的其他信息。 这 24 分钟视频将显示如何为︰

-   生成 Web API 后端并保护其安全与 AAD-1:30 开始
-   将 API 导入 API 管理-从 7:10
-   配置开发人员门户以调用 API-开始 9:09
-   配置桌面应用程序调用的 API-开始 18:08
-   配置预授权请求的开始 20:47 JWT 验证策略

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>创建 Azure 的广告目录

保护您的 Web API 备份使用 Azure Active Directory，您首先必须 AAD 租户。 在本视频中使用一个名为**APIMDemo**的租户。 若要创建 AAD 租户，登录到[Azure 传统门户网站](https://manage.windowsazure.com)，单击**新建**->**应用程序服务**->**Active Directory**->**目录**->**自定义创建**。 

![Azure 的活动目录][api-management-create-aad-menu]

在此示例中名为**APIMDemo**的目录创建与一个名为**DemoAPIM.onmicrosoft.com**的默认域。 此目录是整个视频使用。

![Azure 的活动目录][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>创建 Web API 服务受 Azure Active Directory

在此步骤中，使用 Visual Studio 2013年创建 Web API 后端。 从 1:30 开始的视频这一步。 若要创建 Web API 在 Visual Studio 中的后端项目，请单击**文件**->**新建**->**项目**，然后选择**ASP.NET Web 应用程序**从**Web**模板列表。 在该视频中的项目被命名为**APIMAADDemo**。 单击**确定**以创建项目。 

![Visual Studio][api-management-new-web-app]

单击**Web API**从**模板列表中选择**创建一个 Web API 项目。 若要配置 Azure 目录身份验证，请单击**更改身份验证**。

![新建项目][api-management-new-project]

单击**组织的帐户**，并指定**域**的 AAD 租户。 在此示例中的域是**DemoAPIM.onmicrosoft.com**。 从您的目录的**域**选项卡可以获取您的目录的域。

![域][api-management-aad-domains]

**更改身份验证**对话框中配置所需的设置，然后单击**确定**。

![更改身份验证][api-management-change-authentication]

当您单击**确定**Visual Studio 将尝试使用 Azure 的广告目录注册应用程序，则可能会提示您登录通过 Visual Studio。 为目录使用管理帐户登录。

![登录到 Visual Studio][api-management-sign-in-vidual-studio]

若要将此项目配置为 Azure Web API 复选框，**在云环境中**的主机，然后单击**确定**。

![新建项目][api-management-new-project-cloud]

可能会提示您登录到 Azure，，然后您可以配置 Web 应用程序。

![配置][api-management-configure-web-app]

在此示例中指定了名为**APIMAADDemo**的新**应用程序的服务计划**。

单击**确定**来配置 Web 应用程序和创建项目。

## <a name="add-the-code-to-the-web-api-project"></a>将代码添加到 Web API 项目

视频中的下一步将代码添加到 Web API 项目。 4:35 开始这一步。

在此示例中 Web API 实现基本的计算器服务使用的控制器和模型。 若要添加的服务模型，**模型**在**解决方案资源管理器**中用鼠标右键单击并选择**添加****类**。 命名类`CalcInput`并单击**添加**。

添加以下`using`语句到顶部的`CalcInput.cs`文件。

    using Newtonsoft.Json;

 下面的代码替换生成的类。

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

**控制器**在**解决方案资源管理器**中用鼠标右键单击并选择**添加**->**控制器**。 选择**Web API 2 控制器-空**，然后单击**添加**。 键入**CalcController**作为控制器名称并单击**添加**。

![添加控制器][api-management-add-controller]

添加以下`using`语句到顶部的`CalcController.cs`文件。

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

下面的代码替换生成的控制器类。 本代码实现了`Add`， `Subtract`， `Multiply`，和`Divide`的基本的计算器 api 操作。

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

按**f6 键**以生成并验证解决方案。

## <a name="publish-the-project-to-azure"></a>将项目发布到 Azure

在此步骤中 Visual Studio 项目发布到 Azure。 这一步的视频开始 5:45。

要将项目发布到 Azure，右击**APIMAADDemo**项目中，Visual Studio，然后选择**发布**。 保留默认设置**发布站点**对话框中，单击**发布**。

![Web 发布][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>权限授予 Azure 广告后端服务应用程序

作为 Web API 项目配置和发布过程的一部分您 Azure 的广告目录中创建新的应用程序的后端服务。 在此步骤中的视频，从 6:13，权限授予 Web API 后端。

![应用程序][api-management-aad-backend-app]

单击要配置所需的权限的应用程序的名称。 导航到**配置**选项卡，然后向下滚动到**到其他应用程序的权限**部分。 单击**应用程序权限** **Windows Azure** **Active Directory**旁边的下拉列表，**读取目录数据**，选中复选框，单击**保存**。

![添加权限][api-management-aad-add-permissions]

>[AZURE.NOTE] 如果在其他应用程序的权限下，未列出**Windows Azure** **Active Directory** ，单击**添加应用程序**，从列表中添加。

记下使用的**应用程序 Id URI**在后续步骤中为 API 管理开发人员门户配置 AD Azure 应用程序时。

![应用程序 Id 的 URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>将 Web API 导入 API 管理

从 API 发布门户网站，通过 Azure 经典门户访问配置 Api。 要找到出版商门户，单击**管理**API 管理服务的 Azure 经典门户。 如果尚未创建 API 管理服务实例，请在[您第一组 API 管理][]教程中参阅[创建 API 管理服务实例][]。

![出版商门户][api-management-management-console]

操作可以是[手动添加到 Api](api-management-howto-add-operations.md)，也可以导入。 在本视频中，Swagger 格式在 6:40 开始导入操作。

创建名为`calcapi.json`与下面的内容并将其保存至您的计算机。 确保`host`属性为 Web API 端的点。 在此示例中`"host": "apimaaddemo.azurewebsites.net"`使用。

{"swagger":"2.0"，"信息": {"标题":"计算器"，"说明":"Arithmetics 通过 HTTP ！"，"版本":"1.0"}，"主机":"apimaaddemo.azurewebsites.net"、"基路径":"/ api"，"方案":"http""路径": {"/ 添加？ = {a} & b = {b}": {"get": {"说明":"响应与之和的两个数字。"，"operationId":"添加两个整数"、"参数": [{"name":"a"，"进入":"查询"，"说明":"第一个操作数。 默认值是<code>51</code>。"、"必需": 真"默认值":"51""enum":"51"}，{"名称":"b""in":"查询"，"说明":"第二个操作数。 默认值是<code>49</code>。"、"必需": 真"的默认":"49""enum":"49"}]，"响应": {}}}，"/ sub?a = {a} & b = {b}": {"get": {"说明":"响应与区别两个数字。"，"operationId":"减两个整数"、"参数": [{"名称":"a"，"进入":"查询"，"说明":"第一个操作数。 默认值是<code>100</code>。"、"必需": 真"默认值":"100"，"enum":"100"}，{"名称":"b""in":"查询"，"说明":"第二个操作数。 默认值是<code>50</code>。"、"必需": true 时，"默认值":"50"，"枚举":"50"}]，"响应": {}}}，"/ div?a = {a} & b = {b}": {"get": {"说明":"使用的两个数字的商的响应"，"operationId":"除两个整数"、"参数": [{"名称":"a"，"进入":"查询"，"说明":"第一个操作数。 默认值是<code>100</code>。"、"必需": 真"默认值":"100"，"enum":"100"}，{"名称":"b""in":"查询"，"说明":"第二个操作数。 默认值是<code>20</code>。"、"必需": 真"的默认":"20"，"enum":"20"}]，"响应": {}}}，"/ mul?a = {a} & b = {b}": {"get": {"说明":"响应与产品的两个数字。"，"operationId":"正片叠底两个整数"、"参数": [{"名称":"a"，"进入":"查询"，"说明":"第一个操作数。 默认值是<code>20</code>。"、"必需": 真"默认值":"20"，"enum":"20"}，{"名称":"b""in":"查询"，"说明":"第二个操作数。 默认值是<code>5</code>。"、"必需": 真"默认值":"5"，"enum":"5"}]，"响应": {}}}}}

要导入该计算器 API，从**API 管理**菜单的左侧，单击**Api** ，然后单击**导入 API**。

![API 导入按钮][api-management-import-api]

执行以下步骤来配置 API 的计算器。

1. 单击**来自文件**，浏览到`calculator.json`文件保存，然后单击**Swagger**单选按钮。
2. 在**Web API URL 后缀**文本框中键入**计算**。
3. 在**产品 （可选）**框中单击，然后选择**初学者**。
4. 单击**保存**以导入该 API。

![添加新的 API][api-management-import-new-api]

一旦 API 导入后，API 的摘要页显示在发布服务器上的门户。

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>从开发人员门户未成功调用 API

在这种情况下，API 导入 API 管理，但不尚能调用成功地从开发人员门户因为后端服务使用 Azure AD 身份验证保护。 使用下列步骤 7:40 从视频中说明了这一点。

单击右上方的出版商门户**开发人员门户**。

![开发人员门户][api-management-developer-portal-menu]

单击**Api**然后单击**计算器**API。

![开发人员门户][api-management-dev-portal-apis]

单击**试试看**。

![试一试][api-management-dev-portal-try-it]

单击**发送**并注意**401 未经授权**响应状态。

![发送][api-management-dev-portal-send-401]

因为后端 API 受 Azure Active Directory，请求未获授权。 成功调用 API 开发之前必须配置门户授权使用 OAuth 2.0 的开发人员。 以下各节介绍了此过程。

## <a name="register-the-developer-portal-as-an-aad-application"></a>注册为 AAD 应用程序的开发人员门户

配置授权使用 OAuth 2.0 的开发人员的开发人员门户的第一步是注册为 AAD 应用程序开发人员门户。 说明了这一点开始在视频中 8:27。

该视频中， **APIMDemo**本示例中的第一步从导航到 Azure AD 租户并导航到**应用程序**选项卡。

![新的应用程序][api-management-aad-new-application-devportal]

单击**添加**按钮创建新的 Active Directory 的 Azure 应用程序，然后选择**添加我的公司正在开发的应用程序**。

![新的应用程序][api-management-new-aad-application-menu]

选择**Web 应用程序和/或 Web API**，输入一个名称，并单击下一步的箭头。 在此示例中使用**APIMDeveloperPortal** 。

![新的应用程序][api-management-aad-new-application-devportal-1]

对于**登录 URL**输入 API 管理服务的 URL，然后追加`/signin`。 在此示例中使用**https://contoso5.portal.azure-api.net/signin **。

**应用程序 Id**的 url 输入 API 管理服务的 URL，然后追加一些唯一的字符。 这可能是所需的任何字符，在本例中使用**https://contoso5.portal.azure-api.net/dp** 。 配置所需的**应用程序属性**，请单击复选标记以创建应用程序。

![新的应用程序][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>配置 API 管理 OAuth 2.0 授权服务器

下一步是配置在 API 管理 OAuth 2.0 授权服务器。 9:43 开始视频中演示了这一步。

从左侧的 API 管理菜单中单击**安全**， **OAuth 2.0**中，请单击，然后单击**添加授权**服务器。

![添加授权服务器][api-management-add-authorization-server]

在**名称**和**说明**字段中输入一个名称和可选说明。 这些字段用于标识 OAuth 2.0 授权服务器 API 管理服务实例中。 在此示例中使用**授权服务器演示**。 以后当您指定要用于身份验证的 API OAuth 2.0 服务器，您将选择该名称。

对于**客户端注册页 URL**输入占位符值如`http://localhost`。  **客户端注册页 URL**指向的页面的用户可以使用它来创建和配置 OAuth 2.0 支持的提供程序的帐户的用户管理他们自己的帐户。 在此示例中的用户不创建并配置其自己的帐户，以便使用一个占位符。

![添加授权服务器][api-management-add-authorization-server-1]

接下来，指定**端点 URL 授权**和**标记端点 URL**。

![授权服务器][api-management-add-authorization-server-1a]

可以从**应用程序终结点**的 AAD 应用程序的开发人员门户网站创建页中检索这些值。 为访问终结点定位至 AAD 应用程序的**配置**选项卡，然后单击**查看终结点**。

![应用程序][api-management-aad-devportal-application]

![查看终结点][api-management-aad-view-endpoints]

复制**OAuth 2.0 授权的终结点**，并将其粘贴到**授权端点 URL**文本框。

![添加授权服务器][api-management-add-authorization-server-2]

复制**OAuth 2.0 令牌端点**并将其粘贴到**令牌端点 URL**文本框。

![添加授权服务器][api-management-add-authorization-server-2a]

除了标记终结点中粘贴、 添加附加正文参数命名**资源**，并为值使用**应用程序 Id URI**从 AAD 应用程序时，将创建的后端服务发布 Visual Studio 项目。

![应用程序 Id 的 URI][api-management-aad-sso-uri]

接下来，指定的客户端凭据。 这些是您想访问，在这种情况下后, 端服务的资源的凭据。

![客户端凭据][api-management-client-credentials]

若要获取**客户端 Id**，请导航到后端服务的 AAD 应用程序的**配置**选项卡和复制**客户机 Id**。

若要获取**客户端密码**单击**选择持续时间**下拉列表的**密钥**部分中，指定一个时间间隔。 在此示例中使用 1 年。

![客户机 ID][api-management-aad-client-id]

单击**保存**以保存配置并显示该密钥。 

>[AZURE.IMPORTANT] 记下此注册表项。 一旦您关闭 Azure Active Directory 配置窗口，不能再次显示键。

将密钥复制到剪贴板、 切换回发布服务器门户、 粘贴密钥的**客户端密码**文本框，单击**保存**。

![添加授权服务器][api-management-add-authorization-server-3]

紧随客户端凭据是授权代码授予。 复制此授权码并切换回 Azure 广告开发人员门户应用程序配置页和粘贴授权授予为**答复 URL**字段中，并再次单击**保存**命令。

![回复 URL][api-management-aad-reply-url]

下一步是配置 AAD 应用程序的开发人员门户的权限。 单击**应用程序的权限**并**读取目录数据**的复选框。 单击**保存**以保存此更改，，然后单击**添加应用程序**。

![添加权限][api-management-add-devportal-permissions]

单击搜索图标、 开头为框中，键入**APIM**选择**APIMAADDemo**，并单击复选标记以将保存。

![添加权限][api-management-aad-add-app-permissions]

单击**APIMAADDemo** **委派权限**和**访问 APIMAADDemo**，选中复选框，单击**保存**。 这允许开发人员门户应用程序来访问后端服务。

![添加权限][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>启用计算器 API OAuth 2.0 用户的授权

现在，OAuth 2.0 服务器配置，您可以指定它在您的 API 的安全设置。 14:30 开始视频中演示了这一步。

在左边的菜单中，单击**Api**然后单击**计算器**来查看并配置其设置。

![计算器 API][api-management-calc-api]

导航到**安全**选项卡，选中**OAuth 2.0**复选框，从**授权服务器**下拉列表，选择所需的授权服务器，单击**保存**。

![计算器 API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>成功地从开发人员门户中调用计算器 API

既然 OAuth 2.0 授权配置 API 上，其操作可以成功调用从开发人员中心。 15:00 开始视频中演示了这一步。

向后定位到的开发人员门户中的计算器服务**添加两个整数**操作，然后单击**试一试**。 请注意对应于您刚添加的授权服务器的**授权**部分中的新项。

![计算器 API][api-management-calc-authorization-server]

从授权下拉列表中选择**授权代码**，并输入要使用的帐户的凭据。 如果您已登录到帐户可能不会提示您。

![计算器 API][api-management-devportal-authorization-code]

单击**发送**并记下**响应状态**的**200 确定**和响应内容中操作的结果。

![计算器 API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>配置桌面应用程序来调用 API

视频中的下一步过程开始于 16:30，配置简单的桌面应用程序来调用 API。 第一步是注册 Azure AD 中的桌面应用程序并使其能够访问该目录后, 端服务。 18:25 还没有调用操作计算器 API 的桌面应用程序的演示。

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>JWT 验证将策略配置为预授权请求

在视频的最后过程开始于 20:48，向您显示如何使用[验证 JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT)策略预授权通过验证每个传入请求的访问令牌的请求。 如果不通过验证 JWT 策略验证请求，则该请求 API 管理被阻止，不传递到后端。

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

配置和使用此策略的另一个演示，请参见[云介绍节目 177︰ 更多 API 管理功能](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/)和快进到 13:50。 快进到 15:00 以查看策略编辑器中配置的策略，然后为 18:50 从开发人员门户和无需的授权标记中调用操作的演示。

## <a name="next-steps"></a>下一步行动
-   签出 API 管理有关的更多[的视频](https://azure.microsoft.com/documentation/videos/index/?services=api-management)。
-   保护您的后端服务的其他方法，请参阅[相互证书身份验证](api-management-howto-mutual-certificates.md)，并[通过 VPN 或 ExpressRoute 连接](api-management-howto-setup-vpn.md)。

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[管理您的第一个 API]: api-management-get-started.md
