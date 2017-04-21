<properties 
    pageTitle="云服务和证书管理 |Microsoft Azure" 
    description="了解如何创建和使用 Microsoft Azure 的证书" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>证书概述 Azure 云服务
证书用于在 Azure 云服务 （[服务证书](#what-are-service-certificates)） 并管理 API （为[管理证书](#what-are-management-certificates)时使用 Azure 的传统门户网站并不是 ARM） 进行身份验证。 本主题提供这两种证书类型，概述了如何[创建](#create)和[部署](#deploy)它们到 Azure。

在 Azure 中使用的证书是 x.509 v3 证书，并可由另一个受信任的证书进行签名，或它们可以是自签名的。 自行签署式证书由自己创建者，并为此进行签名，默认情况下不信任。 大多数浏览器都可以忽略此警告。 自行签署式证书只应由您自己开发和测试云服务时。 

使用 Azure 的证书可以包含一个私有或公共密钥。 证书必须提供一种方法以一种明确的方法确定它们的指纹。 此指纹在 Azure 的[配置文件](cloud-services-configure-ssl-certificate.md)用于标识应该使用云服务的证书。 

## <a name="what-are-service-certificates"></a>什么是服务证书？
服务证书附加到云服务以及启用服务和安全的通信。 例如，如果您部署了 web 角色，您可能需要提供可以公开的 HTTPS 终结点进行身份验证的证书。 服务证书，在服务定义中，定义会自动部署到虚拟机正在运行您的角色的实例。 

到 Azure 经典的门户网站使用 Azure 的传统门户网站或通过使用服务管理 API，您可以上载服务证书。 服务证书都与特定的云服务相关联，并且分配给服务定义文件中的部署。

服务证书可以与您的服务分开管理，并且可能由不同的人员管理。 例如，开发人员可能会上载指的是一位 IT 经理以前已上载到 Azure 的证书服务包。 一位 IT 经理可以管理和续订证书更改服务的配置，而无需上载新的服务包。 这是可能的因为该证书的存储区名称和位置的逻辑名称是服务定义文件中指定，而在服务配置文件中指定的证书指纹。 若要更新证书，它只是用来上载一个新证书并更改服务配置文件中的指纹值。

## <a name="what-are-management-certificates"></a>管理证书有哪些？
管理证书，可以使用服务管理 API 提供的 Azure 经典进行身份验证。 许多程序和工具 （如 Visual Studio 或 Azure SDK） 将使用这些证书来自动化配置和部署各种 Azure 服务。 这些真的无关以云服务。 

>[AZURE.WARNING] 小心！ 这些类型的证书允许他们可以管理与其关联的订阅通过身份验证的任何人。 

### <a name="limitations"></a>限制
没有限制为每个订阅的 100 管理证书。 另外，还有 100 下特定的服务管理员的用户 ID 的所有订阅管理证书的限制 如果帐户管理员的用户 ID 已被使用来添加 100 管理证书，并且需要更多的证书，您可以添加联管理员添加的其他证书。 

在添加之前 100 多个证书，请参阅是否您可以重用现有的证书。 使用共同的管理员向您的证书管理过程可能不必要的复杂性。


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>创建新的自签名的证书
您可以使用任何工具可用于创建自签名的证书，只要它们符合这些设置︰

* X.509 证书。
* 包含一个私有密钥。
* 创建密钥交换 （.pfx 文件）。
* 使用者名称必须与用于访问云服务的域相匹配。 
    > 无法获取 SSL 证书 cloudapp.net （或相关的任何 Azure） 域;证书的接受方名称必须与用来访问您的应用程序的自定义域名相匹配。 例如， **contoso.net**，不**contoso.cloudapp.net**。
* 2048 位加密的最小值。
* **只有服务证书**︰ 客户端证书必须驻留在*个人*证书存储区中。

有两种简单的方法创建在 Windows 中，证书与`makecert.exe`实用工具，或者使用 IIS。

### <a name="makecertexe"></a>Makecert.exe

此实用程序已被否决，此处不再记录。 请[此 MSDN 文章](https://msdn.microsoft.com/library/windows/desktop/aa386968)的详细信息，参阅。

### <a name="powershell"></a>PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] 如果您想要使用的证书，而不属于域的 IP 地址，-DnsName 参数中使用的 IP 地址。


如果您想要使用此[证书管理门户网站](../azure-api-management-certs.md)，将其导出至**.cer**文件的步骤︰

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)

有许多页在 internet 上的，介绍了如何使用 IIS 执行此操作。 [这里](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html)我发现，我认为很好地解释它很不错。 

### <a name="java"></a>Java
您可以使用 Java[创建证书](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate)。

### <a name="linux"></a>Linux
[本文](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)介绍如何使用 SSH 创建证书。

## <a name="next-steps"></a>下一步行动

[上载到 Azure 的传统门户网站服务证书](cloud-services-configure-ssl-certificate.md)（或[Azure 门户](cloud-services-configure-ssl-certificate-portal.md)）。

上载到 Azure 经典门户[管理 API 证书](../azure-api-management-certs.md)。

>[AZURE.NOTE] Azure 的门户网站不使用管理证书以访问 API，而是使用用户帐户。
