<properties
    pageTitle="在 Azure AD 签名密钥翻转 |Microsoft Azure"
    description="本文将讨论 Azure Active Directory 的签名密钥翻转最佳做法"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>签名密钥翻转 Azure Active Directory 中

本主题讨论您需要了解的有关公钥用于在 Azure 活动目录 (AD Azure) 中进行签名安全令牌。 值得注意的是，这些密钥变换在定期的基础上，并在出现紧急情况，可能鼠标滑过立即。 所有的应用程序使用 Azure 的广告应该能够以编程方式处理密钥翻转过程或建立定期手动翻转过程。 继续阅读以了解键的工作原理，如何评估向应用程序变换的影响，以及如何更新您的应用程序，或建立一个定期手动翻转流程来处理密钥翻转，如有必要。

## <a name="overview-of-signing-keys-in-azure-ad"></a>在 Azure AD 签名密钥的概述

Azure AD 使用公钥加密算法基于行业标准本身以及它使用的应用程序之间建立信任关系。 在实际术语中，这适用于以下方式︰ Azure 广告使用签名密钥包含公钥和私钥的密钥对。 当用户登录到使用 Azure AD 身份验证的应用程序时，Azure 广告创建安全令牌，其中包含有关用户的信息。 此标记是由 Azure 广告发送回应用程序之前使用它的私钥签名。 若要验证的令牌是有效的和实际上起源于从 Azure 广告，应用程序必须验证使用公开的 Azure AD 租户的[OpenID 连接发现文档](http://openid.net/specs/openid-connect-discovery-1_0.html)或者 SAML/WS-进[联合元数据文档](active-directory-federation-metadata.md)中包含的公钥标记的签名。

为安全起见，Azure 广告的签名密钥将在定期的基础上，并在紧急情况下，无法进行转存立即。 与 Azure AD 集成任何应用程序应准备好处理密钥翻转事件，无论其可能发生的频繁程度。 如果它不存在，并且您的应用程序尝试使用过期的密钥来验证令牌签名，登录请求将会失败。

还有很多个有效的密钥的 OpenID 连接发现文档和联合元数据文档中可用。 您的应用程序应该能够使用任何文档中指定的键，因为可能很快就滚一个键，另可能将其替换，等等。

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>如何评估如果将影响您的应用程序以及如何采取相应措施

您的应用程序如何处理密钥翻转取决于变量，如应用程序或使用何种身份协议和库的类型。 以下各节评估是否最常见的一种应用程序受到密钥翻转和指南提供有关如何更新应用程序以支持自动滚动更新或手动更新密钥。

* [本机客户端应用程序访问的资源](#nativeclient)
* [Web 应用程序 / Api 访问资源](#webclient)
* [Web 应用程序 Api 保护资源，构建使用 Azure 应用程序服务](#appservices)
* [Web 应用程序 / 保护资源使用.NET OWIN OpenID，WS 进或连接 WindowsAzureActiveDirectoryBearerAuthentication 中间件 Api](#owin)
* [Web 应用程序 / 保护资源使用.NET 核心 OpenID 连接或 JwtBearerAuthentication 中间件 Api](#owincore)
* [Web 应用程序 / 保护资源使用 Node.js 护照 azure 广告模块 Api](#passport)
* [Web 应用程序的 Api 保护资源并创建 Visual Studio 2015年](#vs2015)
* [保护资源和使用 Visual Studio 2013年创建的 web 应用程序](#vs2013)
* [保护资源和使用 Visual Studio 2013年创建 web Api](#vs2013_webapi)
* [保护资源和使用 Visual Studio 2012 创建的 web 应用程序](#vs2012)
* [保护资源和使用 Visual Studio 2010，2008 o 使用 Windows 标识基础创建的 web 应用程序](#vs2010)
* [Web 应用程序 / Api 保护资源使用任何其他库或手动实现的任何受支持的协议](#other)

本指南是**不**适用于︰

* 从 Azure AD 应用程序库 （包括自定义） 添加应用程序必须与签名密钥相关的单独指导。 [更多的信息。](active-directory-sso-certs.md)
* 在部署应用程序代理服务器通过发布的应用程序不必担心签名密钥。

### <a name="nativeclient"></a>本机客户端应用程序访问的资源

应用程序只能访问资源 （例如 Microsoft Graph、 KeyVault、 Outlook API 和其他 Microsoft Api） 通常只获得令牌并将其转交给资源所有者。 考虑到他们不会保护任何资源，他们不检查标记，因此不需要确保正确地对其进行签名。

本机客户端应用程序，桌面或移动，是否属于此类别并不会因此影响鼠标经过图像。

### <a name="webclient"></a>Web 应用程序 / Api 访问资源

应用程序只能访问资源 （例如 Microsoft Graph、 KeyVault、 Outlook API 和其他 Microsoft Api） 通常只获得令牌并将其转交给资源所有者。 考虑到他们不会保护任何资源，他们不检查标记，因此不需要确保正确地对其进行签名。

Web 应用程序和 web Api 的使用仅应用程序流 (客户端凭据 / 客户端证书)、 属于此类别并不会因此影响鼠标经过图像。

### <a name="appservices"></a>Web 应用程序 Api 保护资源，构建使用 Azure 应用程序服务

Azure 应用程序服务的身份验证 / 授权 (EasyAuth) 功能已具有必要的逻辑来自动处理密钥翻转。

### <a name="owin"></a>Web 应用程序 / 保护资源使用.NET OWIN OpenID，WS 进或连接 WindowsAzureActiveDirectoryBearerAuthentication 中间件 Api

如果应用程序使用.NET OWIN OpenID，WS 进或连接 WindowsAzureActiveDirectoryBearerAuthentication 中间件，它已具有必要的逻辑来自动处理密钥翻转。

您可以确认您的应用程序正在使用这些通过寻找任何应用程序的 Startup.cs 或 Startup.Auth.cs 中下段

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Web 应用程序 / 保护资源使用.NET 核心 OpenID 连接或 JwtBearerAuthentication 中间件 Api

如果您的应用程序使用.NET 核心 OWIN OpenID 连接或 JwtBearerAuthentication 中间件，它已具有必要的逻辑来自动处理密钥翻转。

您可以确认您的应用程序正在使用这些通过寻找任何应用程序的 Startup.cs 或 Startup.Auth.cs 中下段

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Web 应用程序 / 保护资源使用 Node.js 护照 azure 广告模块 Api

如果应用程序使用的 Node.js 护照 ad 模块，它已具有必要的逻辑来自动处理密钥翻转。

您可以确认您应用程序护照广告通过搜索应用程序的 app.js 在下面的代码段

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Web 应用程序的 Api 保护资源并创建 Visual Studio 2015年

如果在 Visual Studio 2015年使用 web 应用程序模板生成应用程序并从**更改身份验证**菜单中选择**工作和学校的科目**，它已具有必要的逻辑来自动处理密钥翻转。 这种逻辑，嵌入到 OWIN OpenID 连接中间件，检索和缓存的 OpenID 连接发现文档的键并定期刷新它们。

如果手动添加到您的解决方案的身份验证，您的应用程序可能没有所需的密钥翻转逻辑。 您将需要编写您自己，或按照中的步骤[Web 应用程序 / Api 使用任何其他库或手动实现的任何受支持的协议。](#other)。

### <a name="vs2013"></a>保护资源和使用 Visual Studio 2013年创建的 web 应用程序

如果在 Visual Studio 2013年使用 web 应用程序模板生成应用程序并从**更改身份验证**菜单中选择**组织的帐户**，它已具有必要的逻辑来自动处理密钥翻转。 这种逻辑与项目关联的两个数据库表中存储您的组织的唯一标识符和签名的密钥信息。 在项目的 Web.config 文件中，您可以找到数据库的连接字符串。

如果手动添加到您的解决方案的身份验证，您的应用程序可能没有所需的密钥翻转逻辑。 您将需要编写您自己，或按照中的步骤[Web 应用程序 / Api 使用任何其他库或手动实现的任何受支持的协议。](#other)。

以下步骤将帮助您验证正常逻辑运行在您的应用程序。

1. 在 Visual Studio 2013，打开的解决方案中，，然后单击右边的窗口上的**服务器资源管理器**选项卡上的。
2. 展开**数据连接**、 **DefaultConnection**，，然后单击**表**。 定位的**IssuingAuthorityKeys**表格，单击鼠标右键，然后单击**显示表数据**。
3. 在**IssuingAuthorityKeys**表中，将有至少一个行，对应的指纹值的键。 删除表中的所有行。
4. **承租人**表中，用鼠标右键单击，然后单击**显示表格数据**。
5. 在**承租人**表中，将至少一个行中，它对应于一个唯一的目录组织标识符。 删除表中的所有行。 如果您不删除的**承租人**表和**IssuingAuthorityKeys**表中的行，将在运行时可能会出错。
6. 生成并运行该应用程序。 您已登录到您的帐户后，您可以停止应用程序。
7. 返回**服务器资源管理器**，看一看**IssuingAuthorityKeys**和**承租人**表中的值。 您会注意到，它们已被自动重新填充联合元数据文档中的相应信息。

### <a name="vs2013"></a>保护资源和使用 Visual Studio 2013年创建 web Api

如果在 Visual Studio 2013 使用 Web API 模板中创建 web API 应用程序，然后**更改身份验证**菜单中，选择**组织帐户**，您已经在您的应用程序有必要的逻辑。

如果您手动配置身份验证，请按照下面的说明以了解如何配置您的 Web API 来自动更新其密钥信息。

下面的代码段演示如何从联合元数据文档，获取最新的密钥，然后使用[JWT 标记处理程序](https://msdn.microsoft.com/library/dn205065.aspx)验证令牌。 代码段假设您将使用自己的缓存机制来保持密钥来验证将来令牌从 Azure 广告，无论是在数据库、 配置文件或在其他地方。

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>保护资源和使用 Visual Studio 2012 创建的 web 应用程序

如果您的应用程序的内置 Visual Studio 2012，您大概用的标识和访问工具来配置应用程序。 也很有可能您使用的[验证颁发者名称注册表 (VINR)](https://msdn.microsoft.com/library/dn205067.aspx)。 VINR 负责维护受信任的身份提供程序 (Azure AD) 有关的信息和用于验证通过它们所颁发的令牌的密钥。 VINR 还可以轻松自动更新下载与您的目录，相关的最新联合元数据文档存储在 Web.config 文件中的密钥信息检查配置是否与最新的文档，然后更新应用程序根据需要使用新的密钥。

如果您创建应用程序使用的任何代码示例或由 Microsoft 提供的演练文档，您的项目中已包含密钥翻转逻辑。 您将发现项目中已存在以下代码。 如果您的应用程序没有按照下面添加它并验证它是否正常工作的步骤，此逻辑。

1. 在**解决方案资源管理器**中添加适当的项目**System.IdentityModel**程序集的引用。
2. 打开**Global.asax.cs**文件，然后添加以下 using 指令︰
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. 将下面的方法添加到**Global.asax.cs**文件︰
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. 调用**Global.asax.cs**的**Application_Start()**方法中的**RefreshValidationSettings()**方法，如下所示︰
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

一旦您已执行这些步骤，则将使用联合元数据文档，包括最新的项的最新信息更新应用程序的 Web.config。 每次应用程序池循环使用; 在 IIS 中，将发生此更新默认情况下 IIS 被设置回收应用程序每 29 小时。

请按照下面的步骤来验证密钥翻转逻辑正常工作。

1. 验证应用程序是否使用上面的代码后，打开**Web.config**文件，然后定位到**<issuerNameRegistry>**块，专门寻找以下几行︰
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. 在**<add thumbprint=””>**设置，更改任何字符替换为另一个不同的指纹值。 保存**Web.config**文件。

3. 生成应用程序，然后再运行它。 如果可以完成登录过程，您的应用程序成功地从目录的联合元数据文档下载所需的信息的更新密钥。 如果您在签名的问题，确保您的应用程序中的更改是否正确，请[添加登录到您的 Web 应用程序使用 Azure 广告](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)主题，阅读或下载并检查下面的代码示例︰[多租户的 Azure Active Directory 的云应用程序](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b)。


### <a name="vs2010"></a>保护资源和使用 Visual Studio 2008 或 2010年创建的 web 应用程序和 Windows 标识基础 (WIF) 1.0 版的.NET 3.5

如果生成 WIF 1.0 版上的应用程序，则没有提供的机制来自动刷新您的应用程序配置为使用新的密钥。

- *最简单的方法*使用 FedUtil 工具纳入 WIF SDK，可以检索最新的元数据文档和更新您的配置。
- 更新您的应用程序，对.NET 4.5 中，其中包括在 System 命名空间中找到的 WIF 的最新版本。 然后，可以使用[验证颁发者名称注册表 (VINR)](https://msdn.microsoft.com/library/dn205067.aspx)为执行自动更新应用程序的配置。
- 执行手动翻转按照提供的说明本指南文档的结尾处。

使用 FedUtil 来更新您的配置的说明︰

1. 请确认您有 WIF v1.0 开发计算机上安装 Visual Studio 2008 或 2010年的 SDK。 如果尚未安装它，可以[从此处下载](https://www.microsoft.com/en-us/download/details.aspx?id=4451)。
2. 在 Visual Studio，打开的解决方案中，然后用鼠标右键单击相应的项目并选择**更新联合元数据**。 如果此选项不可用，FedUtil 和/或 WIF v1.0 SDK 尚未安装。
3. 出现提示时，选择要开始更新您的联合元数据的**更新**。 如果您有权访问承载应用程序的服务器环境，您可以选择使用 FedUtil 的[更新计划自动元数据](https://msdn.microsoft.com/library/ee517272.aspx)。
4. 单击**完成**以完成更新过程。

### <a name="other"></a>Web 应用程序 / Api 保护资源使用任何其他库或手动实现的任何受支持的协议

如果您使用的其他某个库或手动实现的任何受支持的协议，您将需要查看的库或您实现，以确保正在从 OpenID 连接发现文档或联合元数据文档检索密钥。 一种方法来检查这是出对 OpenID 的发现文档或联合元数据文档的任何调用您的代码库的代码中搜索。

如果由某个位置存储的关键应用程序中的硬编码，您也可以手动检索密钥和更新其相应地通过执行手动翻转按照提供的说明本指南文档的结尾处。 在这篇文章来避免未来的中断**强烈建议您提高您的应用程序来支持自动翻转**使用任何方法概述和开销增加了 Azure 的广告它是变换节奏还是有紧急的带外翻转。

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>如何进行测试以确定是否将影响您的应用程序

您可以验证应用程序是否支持自动密钥翻转下载这些脚本并遵循中的说明通过[此 GitHub 资料库。](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>如何执行手动翻转，如果应用程序不支持自动翻转

如果您的应用程序**不**支持自动滚动更新，您将需要建立定期监视 Azure 广告的签名密钥，并相应地执行手动翻转的过程。 [此 GitHub 储存库](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)包含脚本以及如何执行此操作的说明。
