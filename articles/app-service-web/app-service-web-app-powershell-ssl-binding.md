<properties
    pageTitle="使用 PowerShell 的 SSL 证书绑定"
    description="了解如何将 SSL 证书绑定到您使用 PowerShell 的 web 应用程序。"
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>使用 PowerShell 的 azure 应用程序服务 SSL 证书绑定 #

随着 Microsoft Azure PowerShell 1.1.0 版的发布已，可能使用户能够将现有的或新的 SSL 证书绑定到现有的 Web 应用程序添加新的 cmdlet。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

若要了解有关使用 Azure 资源管理器基于 Azure PowerShell cmdlet 来管理您的 Web 应用程序检查[Azure 资源管理器基于 PowerShell Azure Web 应用程序的命令](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>上传并绑定一个新的 SSL 证书 ##

方案︰ 用户希望将 SSL 证书绑定到其 web 应用程序之一。

了解 web 应用程序、 web 应用程序名称、 用户计算机、 密码证书，以及自定义的主机名称的证书.pfx 文件路径包含的资源组名称，我们可以使用下面的 PowerShell 命令来创建该 SSL 绑定︰

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

注意添加之前的 SSL 绑定到 web 应用程序，必须具有一个已配置的主机名 （自定义域）。 如果未配置的主机名，则会收到错误主机名 New AzureRmWebAppSSLBinding 在运行时不存在。 您可以直接从门户网站或使用 Azure PowerShell 添加主机名。 下面的 PowerShell 段可以运行新建 AzureRmWebAppSSLBinding 之前配置主机名。   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
请务必了解集 AzureRmWebApp cmdlet 将覆盖 web 应用程序的主机名。 因此上述 PowerShell 段追加到现有的 web 应用程序的主机名列表。  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>上传和绑定现有 SSL 证书 ##

方案︰ 用户要将以前上载的 SSL 证书绑定到其 web 应用程序之一。

我们可以获得证书已通过使用以下命令上载到特定资源组的列表

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

请注意，证书仅用于特定的位置和资源组，该用户需要重新上载证书，如果配置的 web 应用程序位于不同位置和其他资源组所需的证书 

了解资源组名称包含 web 应用程序、 web 应用程序名称、 证书指纹和自定义的主机名，我们可以使用下面的 PowerShell 命令来创建该 SSL 绑定︰

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>删除现有的 SSL 绑定  ##

方案︰ 用户要删除现有的 SSL 绑定。

了解资源组名称包含 web 应用程序、 web 应用程序名称，以及自定义的主机名，我们可以使用下面的 PowerShell 命令以移除该 SSL 绑定︰

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

请注意，如果已删除的 SSL 绑定时使用该证书的位置，默认情况下，该证书将被删除，如果用户想要保存他可以使用 DeleteCertificate 选项保留证书，该证书的最后一个绑定

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>引用 ###
- [Azure 的资源管理器基于 PowerShell 命令的 Azure Web 应用程序](app-service-web-app-azure-resource-manager-powershell.md)
- [应用程序服务环境简介](app-service-app-service-environment-intro.md)
- [使用 Azure PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager.md)
