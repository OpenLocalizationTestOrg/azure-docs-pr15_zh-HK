<properties
    pageTitle="使用 SCIM 启用自动提供的用户和应用程序从 Azure 的 Active Directory 组 |Microsoft Azure"
    description="Azure 的 Active Directory 可以自动设置用户和组添加到与 SCIM 协议规范中定义的接口 fronted 由 Web 服务任何应用程序或标识存储区"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>如何使用 SCIM 以使用户和应用程序从 Azure 的 Active Directory 组的自动资源调配

##<a name="overview"></a>概述

用户和组添加到任何与[SCIM 2.0 协议规范](https://tools.ietf.org/html/draft-ietf-scim-api-19)中定义的接口 fronted 由 Web 服务的应用程序或标识商店，azure Active Directory 可以自动调配。 Azure 的 Active Directory 可以发送请求以创建、 修改和删除指派的用户和组添加到此 Web 服务，然后可以将这些请求翻译为目标的身份存储的操作。 

![][1]
*通过 Web 服务标识存储到 Azure Active Directory 从资源调配的图︰*

此功能可结合具有"[携带您自己的应用程序](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)"功能在 Azure AD 中启用单一登录和自动资源调配的提供或 fronted 由 SCIM web 服务应用程序的用户。

有两个用例 SCIM Azure Active Directory 中为︰

* **设置用户和组添加到应用程序的支持 SCIM** -支持 SCIM 2.0 和使用 OAuth 载体令牌进行身份验证的应用程序将处理的 Azure 中的广告。

* **生成应用程序，支持其他基于 API 的提供自己的资源调配解决方案**-对于非 SCIM 应用程序，您可以创建 SCIM 终结点 Azure AD SCIM 终结点和内容之间进行转换 API 应用程序为用户提供支持。  为了帮助 SCIM 终结点的开发中，我们提供了 CLI 库和代码示例，向您展示如何执行提供 SCIM 端点和翻译 SCIM 邮件。  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>设置用户和组添加到的应用程序支持 SCIM

Azure 的活动目录可以配置为自动分配提供用户和组添加到实现[跨域身份管理 2 (SCIM) 系统](https://tools.ietf.org/html/draft-ietf-scim-api-19)Web 应用程序服务并接受 OAuth 持有者进行身份验证的令牌。 在 SCIM 2.0 规范，应用程序必须满足以下要求︰

* 创建用户和/或组，按照 3.3 部分 SCIM 协议的支持。  

* 修改用户和/或组与修补程序请求根据部分 3.5.2 SCIM 协议的支持。  

* 检索部分 3.4.1 SCIM 协议根据已知的资源的支持。  

*  查询用户和/或组，根据部分 3.4.2 SCIM 协议的支持。  默认情况下，通过 externalId 将会询问用户和组的显示名称进行查询。  

* 查询用户 id，并根据部分 3.4.2 SCIM 协议管理器的支持。  

* 查询组通过 ID 和部分 3.4.2 SCIM 协议根据成员的支持。  

* 接受 OAuth SCIM 协议根据第 2.1 节授权持有者标记。

您应该咨询您应用程序的提供程序或与这些要求的兼容性的语句的应用程序提供商的文档。
 
###<a name="getting-started"></a>入门教程

支持上述 SCIM 配置文件的应用程序可以连接到 Azure Active Directory 使用 Azure AD 应用库中的"自定义"应用程序功能。 一旦连接，Azure AD 运行同步进程每 5 分钟对其查询应用程序的 SCIM 终结点分配的用户和组，并创建或修改它们根据工作分配详细信息。

**要连接的应用程序支持 SCIM:**

1.  在 web 浏览器中启动 https://manage.windowsazure.com 在 Azure 的管理门户。
2.  浏览到**Active Directory > 目录 > [您目录] > 应用程序**，并选择**添加 > 库中添加应用程序**。
3.  选择左边的**自定义**选项卡，为您的应用程序输入一个名称并单击复选标记图标以创建应用程序对象。

![][2]

4.  在结果屏幕中，选择**配置帐户设置**第二个按钮。
5.  在**资源调配端点 URL**字段中，输入应用程序的 SCIM 终结点的 URL。
6.  如果 SCIM 终结点需要从 Azure 广告不颁发 OAuth 持有者标记，然后复制所需的 OAuth 载体令牌**身份验证令牌 （可选）**字段中。 将此字段保留为空，然后 Azure 广告将包括从每个请求的 Azure AD 颁发 OAuth 载体令牌。 应用程序使用 idenity 提供程序可以验证此 Azure 广告 Azure AD-颁发令牌。
7.  单击**下一步**，然后单击**开始测试**按钮具有 Azure Active Directory 尝试连接到 SCIM 终结点上。 如果失败，将显示诊断信息。  
8.  如果尝试连接到应用程序成功中，然后单击**下一步**的剩余屏幕、 并单击**完成**以退出对话框。
9.  在结果屏幕中，选择第三个**指定帐户**按钮。 在得到用户和组部分中分配的用户或组所需资源调配给应用程序。
10. 一旦分配了用户和组，请单击屏幕顶部附近的**配置**选项卡。
11. 在**帐户设置**下确认状态被设置为 On。 
12. 在**工具**，为启动设置过程中单击**重新启动帐户设置**。

请注意设置过程将开始向 SCIM 终结点发送请求之前，可能会经过 5-10 分钟。  尝试连接的摘要提供应用程序的仪表板选项卡上，并可以从该目录的报告选项卡下载资源调配活动的报告和任何资源调配错误。

##<a name="building-your-own-provisioning-solution-for-any-application"></a>构建您自己的设置的任何应用程序的解决方案

通过创建与 Azure Active Directory SCIM web 服务的接口，您可以启用单一登录和自动用户提供配置 API 的其余部分或 SOAP 用户几乎任何应用程序资源调配。

下面是它如何工作︰

1.  Azure 的广告提供了一个名为[Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)的公共语言基础结构库。 系统集成商和开发人员可以使用此库创建和部署 SCIM 基于 web 服务终结点可以连接到任何应用程序的标识存储 Azure 的广告。
2.  映射是标准的用户架构映射到用户架构和协议所需的应用程序的 web 服务中实现的。
3.  在 Azure 应用程序库中的自定义应用程序的一部分的广告中注册端点 URL。
4.  用户和组分配给此应用程序在 Azure 的广告。 在分配中，他们被置于要同步到目标应用程序的队列。 同步过程处理队列运行每隔 5 分钟。

###<a name="code-samples"></a>代码示例

若要使此过程更容易，一套[代码示例](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)提供了这产生 SCIM web 服务终结点，并展示自动资源调配。 一个示例是包含行的以逗号分隔的值，表示用户和组维护一个文件的提供程序。  另一种是 Amazon Web 服务标识和访问管理服务上运行的提供程序。  

**系统必备组件**

* Visual Studio 2013年或更高版本
* [使.NET 的 azure SDK](https://azure.microsoft.com/downloads/)
* Windows 计算机支持 ASP.NET 框架 4.5 用作 SCIM 终结点。 此计算机必须可以从云访问
* [试用版或许可版本的 Azure AD 特优 Azure 订阅](https://azure.microsoft.com/services/active-directory/)
* Amazon AWS 示例要求从[Visual Studio 的 AWS Toolkit](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html)的库。 请参阅自述文件，了解其他详细信息的示例包括

###<a name="getting-started"></a>入门教程

实现可以接受从 Azure 广告资源调配请求 SCIM 终结点的最简便方法是生成和部署代码示例将输出到一个逗号分隔值 (CSV) 文件的资源调配的用户。

**若要创建一个示例 SCIM 终结点︰**

1.  下载的代码的示例包中[https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2.  将软件包解压缩并将其放置在您的 Windows 计算机，如 C:\AzureAD-BYOA-Provisioning-Samples\ 的位置上。
3.  在此文件夹中，启动 Visual Studio 中的 FileProvisioningAgent 解决方案。
4.  选择**工具 > 库程序包管理器 > 程序包管理器控制台**，和执行 FileProvisioningAgent 项目以解决解决方案引用下面的命令︰

    安装软件包 Microsoft.SystemForCrossDomainIdentityManagement 安装软件包 Microsoft.IdentityModel.Clients.ActiveDirectory 安装软件包 Microsoft.Owin.Diagnostics 安装软件包 Microsoft.Owin.Host.SystemWeb

5.  生成的 FileProvisioningAgent 项目。
6.  启动命令提示符应用程序窗口中的 （如管理员），并使用**cd**命令将目录更改到**\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug**文件夹。
7.  运行下面，< ip 地址 > 替换 Windows 机器的 IP 或域名名称的命令。

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  在 Windows 下**Windows 设置 > 网络和 Internet 设置**，选择**Windows 防火墙 > 高级设置**，并创建一个**入站规则**以允许入站的访问端口 9000。
9.  如果 Windows 计算机通过路由器，路由器将需要配置为执行网络访问转换其端口 9000，暴露给 internet 与 Windows 的计算机上的端口 9000 之间。 这是 Azure 的广告能够访问此终结点云中的必需。


**在 Azure AD 中注册示例 SCIM 终结点︰**

1.  在 web 浏览器中启动 https://manage.windowsazure.com 在 Azure 的管理门户。
2.  浏览到**Active Directory > 目录 > [您目录] > 应用程序**，并选择**添加 > 库中添加应用程序**。
3.  选择左边的**自定义**选项卡，输入一个名称，例如"SCIM 测试 App"，并单击复选标记图标以创建应用程序对象。 请注意，应用程序对象创建打算表示您将资源调配给并实现单一登录，并不只是 SCIM 终结点的目标应用程序。

![][2]

4.  在结果屏幕中，选择**配置帐户设置**第二个按钮。
5.  在对话框中，输入到 internet 的 URL 和 SCIM 端点的端口。 这将是像 http://testmachine.contoso.com:9000 或 http://<ip-address>:9000/，其中 < ip 地址 > 是互联网公开 IP 地址。  
6.  单击**下一步**，然后单击**开始测试**按钮具有 Azure Active Directory 尝试连接到 SCIM 终结点上。 如果失败，将显示诊断信息。  
7.  如果成功，将尝试连接到您的 Web 服务，然后剩余的屏幕中，单击**下一步**，单击**完成**以退出对话框。
8.  在结果屏幕中，选择第三个**指定帐户**按钮。 在得到用户和组部分中分配的用户或组所需资源调配给应用程序。
9.  一旦分配了用户和组，请单击屏幕顶部附近的**配置**选项卡。
10. 在**帐户设置**下确认状态被设置为 On。 
11. 在**工具**，为启动设置过程中单击**重新启动帐户设置**。

请注意设置过程将开始向 SCIM 终结点发送请求之前，可能会经过 5-10 分钟。  尝试连接的摘要提供应用程序的仪表板选项卡上，并可以从该目录的报告选项卡下载资源调配活动的报告和任何资源调配错误。

验证示例的最后一步是在您的 Windows 计算机上的 \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug 文件夹中打开 TargetFile.csv 文件。 一旦运行资源调配过程，此文件将显示的所有详细信息分配和配置用户和组。

###<a name="development-libraries"></a>开发库

若要开发自己符合 SCIM 规范的 Web 服务，首先熟悉 Microsoft 帮助加速开发过程中提供的以下库︰ 

**1:** 公共语言基础结构库提供了用于与基于该基础结构，如 C# 的语言。  其中一个这些库， [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)，声明一个接口中，Microsoft.SystemForCrossDomainIdentityManagement.IProvider，如下图所示。  使用库的开发人员将实现该接口，可称，一般情况下，为提供程序的类。  库使开发人员能够轻松地将符合 Web 服务部署到 SCIM 规范，或者托管在 Internet Information Services 或任何可执行的公共语言基础结构组件。  到该 Web 服务的请求将被转换为调用提供程序的方法，通过编程由开发人员在某些标识存储上运行。    

![][3]

**2:**  [速成版路由处理](http://expressjs.com/guide/routing.html)程序可用于分析 node.js 请求对象表示调用 （按定义 SCIM 规范），对 node.js Web 服务。   

###<a name="building-a-custom-scim-endpoint"></a>构建自定义 SCIM 终结点

使用上面描述的库，使用这些库的开发人员可以承载或 Internet Information Services 内任何可执行的公共语言基础结构组件，其服务。  下面是用于承载内可执行程序集，请在地址 http://localhost:9000 服务的示例代码︰ 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

值得注意的是，此服务必须具有 HTTP 地址和服务器身份验证证书的根证书颁发机构是下列之一︰ 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* 转到爸爸
* Verisign
* WoSign

服务器身份验证证书可以被绑定到端口在 Windows 主机上使用网络外壳程序实用程序，如下所示︰ 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
在这里，certhash 参数提供的值是证书的指纹，而为应用程序标识的参数提供的值是任意的全局唯一标识符。  

若要承载该服务在 Internet Information Services，开发人员将名启动为该程序集的默认命名空间中的类与构建公共语言基础结构代码库程序集。  以下是此类的示例︰ 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>终结点处理的身份验证

从 Azure Active Directory 的请求包括 OAuth 2.0 载体令牌。   接收请求的任何服务应视为代表预期的 Azure Active Directory 租户，Azure Active Directory 图表 Web 服务访问 Azure Active Directory 颁发者进行身份验证。  在令牌颁发者由 iss 声明，如"iss"标识:"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/"。  在此示例中，该声明值，基址 https://sts.windows.net，将 Azure Active Directory 标识为颁发者，虽然相对地址段，cbb1a5ac-f33b-45fa-9bf5-f37db0fed422，是 Azure Active Directory 组织代表其发出该标记的唯一标识符。  如果令牌是用于访问 Azure Active Directory 图表 Web 服务，该服务，00000002-0000-0000-c000-000000000000，标识符应标记的 aud 声明的值中。  

使用由 Microsoft 提供的用于生成 SCIM 服务公共语言基础结构库的开发人员可以按照下面的步骤使用 Microsoft.Owin.Security.ActiveDirectory 软件包的 Azure Active Directory 中的请求进行身份验证︰ 

**1:** 在一个提供程序，让它返回时启动该服务时调用的方法来实现 Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior 属性︰ 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:** 将以下代码添加到该方法具有对任何贴有由 Azure Active Directory 来颁发代表指定的租户，Azure Active Directory 图表 Web 服务访问令牌通过身份验证的服务的终结点的任何请求︰ 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>用户和组模式

Azure 的 Active Directory 可以设置两种类型的资源到 SCIM Web 服务。  这些类型的资源的用户和组。  

由 urn: ietf:params:scim:schemas:extension:enterprise:2.0:User，此协议规范中包含的架构标识符标识的用户资源︰ http://tools.ietf.org/html/draft-ietf-scim-core-schema。  下面表 1 中提供了 Azure Active Directory 中的用户对 urn: ietf:params:scim:schemas:extension:enterprise:2.0:User 资源的属性的属性的默认映射。  

组资源由架构标识符，http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group。  表 2，下面所示的 Azure Active Directory 中的属性 http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group 资源组的属性的默认映射。  

###<a name="table-1-default-user-attribute-mapping"></a>表 1︰ 默认用户属性映射

| Azure Active Directory 用户 | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | 活动 |
| 显示名称 | 显示名称 |
| 传真 TelephoneNumber | phoneNumbers [类型 eq"fax"] 值 |
| givenName | name.givenName |
| 职务 | 标题 |
| 邮件 | 电子邮件 [类型 eq"工作"] 值 |
| mailNickname | externalId |
| 管理器 | 管理器 |
| 移动 | phoneNumbers [类型 eq"移动电话"] 值 |
| 对象 Id | 标识 |
| 邮政编码 | 地址 [类型 eq"工作"].postalCode |
| 代理服务器地址 | [单击此处输入"其他"eq] 的电子邮件。值 |
| 物理-传递-OfficeName | [单击此处输入"其他"eq] 的地址。设置格式 |
| 街道地址 | 地址 [类型 eq"工作"].streetAddress |
| 姓氏 | name.familyName |
| 电话号码 | phoneNumbers [类型 eq"工作"] 值 |
| 用户 PrincipalName | 用户名 |


###<a name="table-2-default-group-attribute-mapping"></a>表 2︰ 默认组属性映射

| Azure 的 Active Directory 组 | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| 显示名称 | externalId |
| 邮件 | 电子邮件 [类型 eq"工作"] 值 |
| mailNickname | 显示名称 |
| 成员 | 成员 |
| 对象 Id | 标识 |
| 代理地址 | [单击此处输入"其他"eq] 的电子邮件。值 |


##<a name="user-provisioning-and-de-provisioning"></a>用户供应，和消除资源调配

显示下的图 SCIM 服务发送 Azure Active Directory 将可管理的生命周期中另一种身份的用户的邮件存储。  该图还显示 SCIM 服务使用由 Microsoft 提供的用于生成此类服务的公共语言基础结构库实现的方式将这些请求转化为对提供程序的方法的调用。  

![][4]
*图︰ 用户供应，和消除资源调配顺序*

**1:** Azure 的 Active Directory 将 externalId 属性值匹配 mailNickname 属性值在 Azure Active Directory 中的用户查询用户的服务。  与此类似，jyoung 将作为样本的 Azure Active Directory 中的用户 mailNickname 的超文本传输协议请求将以表示查询︰ 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

服务是使用由 Microsoft 提供的有关实施 SCIM 服务公共语言基础结构库生成的该请求将翻译服务提供商的查询方法的调用。  以下是该方法的签名︰ 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

以下是 Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters 接口的定义︰ 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

对于上述示例的用户使用 externalId 属性的给定值的查询，查询方法传递的参数的值将按如下所述︰ 

* 参数。AlternateFilters.Count: 1
* 参数。AlternateFilters.ElementAt(0)。AttributePath:"externalId"
* 参数。AlternateFilters.ElementAt(0)。比较运算符︰ ComparisonOperator.Equals
* 参数。AlternateFilter.ElementAt(0)。ComparisonValue:"jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin。了申请 Id"] 

**2:** 如果对 externalId 属性值匹配 mailNickname 属性值在 Azure Active Directory 中的用户具有权限的用户的服务查询的响应不会返回任何用户，Azure Active Directory 将请求该服务提供对应于一个 Azure Active Directory 中的用户。  以下是此类请求的示例︰ 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

为实施 SCIM 服务由 Microsoft 提供的公共语言基础结构库将该请求转换给服务提供商的 Create 方法的调用。  Create 方法具有此签名︰ 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

如果是提供用户的请求，将 Microsoft.SystemForCrossDomainIdentityManagement 的实例资源参数的值。 Microsoft.SystemForCrossDomainIdentityManagement.Schemas 库中定义的 Core2EnterpriseUser 类。  如果要设置用户的请求成功，则该方法的实现应返回的实例 Microsoft.SystemForCrossDomainIdentityManagement。 Core2EnterpriseUser 类，标识符属性设置为新设置的用户的唯一标识符的值。  

**3:** 要更新 fronted 通过 SCIM 标识存储区中存在的已知用户，Azure Active Directory 将继续通过与类似此请求从服务请求该用户的当前状态︰ 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

中使用的公共语言基础结构库由 Microsoft 提供的有关实施 SCIM 服务构建一个服务，该请求将被转换为对服务提供商的检索方法的调用。  以下是检索方法的签名︰ 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

对于一个请求，以检索用户的当前状态的上述示例中，参数参数的值与提供的对象的属性的值将按如下所述︰ 

* 标识符:"54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier:"urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:** 如果引用属性进行更新，然后 Azure Active Directory 将查询服务，以确定标识存储区中的引用属性的当前值 fronted 服务已经符合 Azure Active Directory 中的该属性的值。  对于用户，都将以这种方式查询的当前值的唯一属性是管理器属性。  下面是一个请求，以确定一个特定的用户对象的管理器属性当前是否具有特定值的示例︰ 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

值 id、 属性查询参数，表示，如果用户对象存在满足表达式作为参数值的筛选器查询，则服务需要 urn: ietf:params:scim:schemas:core:2.0:User 或 ietf:params:scim:schemas:extension:enterprise:2.0:User urn︰ 资源，包括该资源的 id 属性的值以响应。  当然，id 属性的值已知的请求程序 — — 它包含在筛选器查询参数值的;要求提供它的目的是资源的实际请求最少表示形式满足用于指示存在任何此类对象的筛选器表达式。   

服务是使用由 Microsoft 提供的有关实施 SCIM 服务公共语言基础结构库生成的该请求将翻译服务提供商的查询方法的调用。  参数的参数的值与提供的对象的属性的值将如下所示︰ 

* 参数。AlternateFilters.Count: 2
* 参数。AlternateFilters.ElementAt(x)。AttributePath:"id"
* 参数。AlternateFilters.ElementAt(x)。比较运算符︰ ComparisonOperator.Equals
* 参数。AlternateFilter.ElementAt(x)。ComparisonValue:"54D382A4-2050-4C03-94D1-E769F1D15682"
* 参数。AlternateFilters.ElementAt(y)。AttributePath:"管理器"
* 参数。AlternateFilters.ElementAt(y)。比较运算符︰ ComparisonOperator.Equals
* 参数。AlternateFilter.ElementAt(y)。ComparisonValue:"2819c223-7f76-453a-919d-413861904646"
* 参数。RequestedAttributePaths.ElementAt(0):"id"
* 参数。SchemaIdentifier:"urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

在这里，索引 x 的值可能是 0 和索引 y 的值可以是 1，或者 x 的值可能是 1 和 y 的值可能是 0，这取决于该筛选器查询参数的表达式的顺序。   

**5:** 这里是从 Azure Active Directory SCIM 服务来更新用户请求的示例︰ 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Microsoft 公共语言基础结构库，用于实施 SCIM 服务将转换服务供应商的更新方法调用请求。  以下是该方法的签名︰ 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



在上述示例中更新用户的请求，作为修补程序参数的值提供的对象将具有这些属性值︰ 

* ResourceIdentifier.Identifier:"54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier:"urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (为 PatchRequest2 PatchRequest)。Operations.Count: 1
* (为 PatchRequest2 PatchRequest)。Operations.ElementAt(0)。操作名称︰ OperationName.Add
* (为 PatchRequest2 PatchRequest)。Operations.ElementAt(0)。Path.AttributePath:"管理器"
* (为 PatchRequest2 PatchRequest)。Operations.ElementAt(0)。Value.Count: 1
* (为 PatchRequest2 PatchRequest)。Operations.ElementAt(0)。Value.ElementAt(0)。引用︰ http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (为 PatchRequest2 PatchRequest)。Operations.ElementAt(0)。Value.ElementAt(0)。值︰ 2819c223-7f76-453a-919d-413861904646

**6:** 若要取消设置 fronted SCIM 服务身份存储库中的用户，Azure Active Directory 将发送类似这样的请求︰ 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
服务是使用由 Microsoft 提供的有关实施 SCIM 服务公共语言基础结构库生成的该请求将翻译服务提供商的 Delete 方法调用。   该方法具有此签名︰ 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
ResourceIdentifier 参数的值与提供的对象将具有这些属性值在上述示例中消除提供用户请求的情况下︰ 

* ResourceIdentifier.Identifier:"54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier:"urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>组资源调配和消除资源调配

显示下的图 SCIM 服务发送 Azure Active Directory 将管理的生命周期中另一标识组的电子邮件存储。  这些消息不同于属于以下三种方式的用户的邮件︰ 

* 组资源的架构将被标识为 http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group。  
* 检索组的请求将规定成员属性是从对请求的响应中提供的任何资源中排除。  
* 确定是否引用属性具有特定值的请求将有关成员属性的请求。  

![][5]
*图︰ 组资源调配和消除资源调配顺序*

##<a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [自动化用户资源调配/撤消对 SaaS 应用程序](active-directory-saas-app-provisioning.md)
- [为用户提供自定义属性映射](active-directory-saas-customizing-attribute-mappings.md)
- [编写属性映射的表达式](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [作用域为用户提供的筛选器](active-directory-saas-scoping-filters.md)
- [帐户设置通知](active-directory-saas-account-provisioning-notifications.md)
- [有关如何集成 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
