<properties 
    pageTitle="如何创建使用 Azure 的资源管理器模板 ILB ASE |Microsoft Azure" 
    description="了解如何创建使用 Azure 资源管理器模板内部负载平衡器 ASE。" 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>如何创建使用 Azure 的资源管理器模板 ILB ASE

## <a name="overview"></a>概述 ##
可以使用虚拟网络内部地址而不是公用的 VIP 创建应用程序服务的环境。  此内部地址可由 Azure 组件调用内部负载平衡器 (ILB)。  可以使用 Azure 门户创建 ILB ASE。  它也可以创建使用 Azure 资源管理器模板通过自动化。  本文讲解的步骤和创建 ILB ASE Azure 资源管理器模板所需的语法。

有自动化 ILB ASE 的创建中所涉及的三个步骤︰
1. 第一次基 ASE 创建虚拟网络使用内部负载平衡器的地址，而不公用的 VIP。  作为本步骤的一部分，根域名分配给 ILB ASE。
2. 一旦创建了 ILB ASE，上载一个 SSL 证书。  
3. 已上载的 SSL 证书显式分配到 ILB ASE 作为它"的默认"的 SSL 证书。  此 SSL 证书将用于 SSL 通信，并向 ILB ASE 的应用程序时应用程序解决使用常见的根域分配到 ASE (例如 https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>创建 ILB ASE 的基 ##
Azure 资源管理器模板示例，以及与其关联的参数文件，GitHub 上还有[这里][quickstartilbasecreate]。

大部分的*azuredeploy.parameters.json*文件中的参数创建 ILB ASEs 以及绑定到公用 VIP ASEs 共有。  Out 参数的特殊注意，调用下面的列表，或者是唯一的在创建 ILB ASE 时︰


- *interalLoadBalancingMode*︰ 在大多数情况下设置为 3，这意味着 HTTP/HTTPS 通信量在端口 80/443，并且控制数据通道端口侦听 ASE 上的 FTP 服务将绑定到分配虚拟网络内部地址 ILB。  如果改为此属性设置为 2，仅 FTP 服务相关将被绑定到 ILB 地址，端口 （控制和数据通道），而 HTTP/HTTPS 通信量将保留在公用的 VIP。
-  *dnsSuffix*︰ 此参数定义将分配给 ASE 的默认根域。  在 Azure 应用程序服务的公用变体，所有 web 应用程序的默认根域是*azurewebsites.net*。  但是由于 ILB ASE 客户虚拟网络的内部，它毫无意义，用于公共服务的默认根域。  相反，ILB ASE 应具有默认的根域用于公司的内部虚拟网络中的意义。  例如，假想 Contoso 公司可能使用默认的根域的*内部 contoso.com*倾向于仅可解析，Contoso 的虚拟网络中可访问的应用程序。 
-  *ipSslAddressCount*︰ 因为 ILB ASEs 仅有一个 ILB 地址，此参数将自动默认为 0 *azuredeploy.json*文件中的值。  没有显式 IP SSL 地址为 ILB ASE，因此 ILB ASE 的 IP SSL 地址池中必须设置为零，否则会出现配置错误。 

*Azuredeploy.parameters.json*文件已经为 ILB ASE 填写之后, 可以再使用下面的代码段 Powershell 创建 ILB ASE。  更改文件路径，以匹配 Azure 资源管理器模板文件在您的计算机上的位置。  也请务必提供您自己的值 Azure 资源管理器部署名称和资源组的名称。

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

在 Azure 资源管理器中提交模板将花费几个小时要创建 ILB ASE。  创建完成后，ILB ASE 将显示在门户中的用户体验触发部署订阅的列表的应用程序服务的环境中。

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>上载并配置的"默认值"SSL 证书 ##

一旦创建了 ILB ASE，与"默认"SSL 证书用于建立 SSL 连接到应用程序应与 ASE SSL 证书。  继续假想 Contoso 公司示例中，如果 ASE 的默认 DNS 后缀是*内部 contoso.com*，然后连接到*https://some-random-app.internal-contoso.com*要求有效的 SSL 证书 **.internal contoso.com*。 

有多种方式来获取有效的 SSL 证书包括内部 Ca 购买来自外部的颁发者的证书，使用自签名的证书。  无论 SSL 证书的来源，需要正确配置下列证书属性︰

- *主题*︰ 此属性必须设置为 **.your 根域 here.com*
- *主题备用名称*︰ 此属性必须包括**.your 根域 here.com*，和* *.scm.your-根的域-here.com*。  第二项理由是将使用的窗体*your-app-name.scm.your-root-domain-here.com*地址做为与每个应用程序相关联的 SCM/Kudu 站点的 SSL 连接。

用手中的有效 SSL 证书，需要两个额外的准备步骤。  SSL 证书必须转换/保存为一个.pfx 文件。  请记住，该.pfx 文件需要包括所有中间和根证书，还需要使用密码进行保护。

那么结果.pfx 文件需要转换成 base64 字符串，因为将使用 Azure 资源管理器模板上载的 SSL 证书。  由于 Azure 资源管理器的模板是文本文件，.pfx 文件需要转换成 base64 字符串以便可以作为模板的参数。

Powershell 代码段显示了示例生成自签名的证书，证书导出为.pfx 文件，将.pfx 文件转换成 base64 编码的字符串，然后保存 base64 编码字符串保存到一个单独的文件。  Base64 编码的 Powershell 代码已经得到了修改从[Powershell 脚本博客][examplebase64encoding]。

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
一旦 SSL 证书已成功生成并转换成 base64 编码的字符串，GitHub 上的示例 Azure 资源管理器模板配置[默认的 SSL 证书][configuringDefaultSSLCertificate]可以使用。

*Azuredeploy.parameters.json*文件中的参数如下所示︰

- *appServiceEnvironmentName*︰ 正在配置 ILB ASE 的名称。
- *existingAseLocation*︰ 文本字符串，它包含的 Azure 地区部署 ILB ASE 的位置。  例如:"美国"。
- *pfxBlobString*: based64 编码的.pfx 文件的字符串表示形式。  使用前面所示的代码段，将复制字符串包含在"exportedcert.pfx.b64"，并为*pfxBlobString*属性的值粘贴到此位置。
- *密码*︰ 密码用于保护该.pfx 文件。
- *certificateThumbprint*︰ 该证书的指纹。  如果您从 Powershell 检索此值 (例如*$certificate。指纹*从早期代码段)，您可以使用的值为-是。  但是如果您从 Windows 证书对话框复制值，请记住要去除掉多余的空格。  *CertificateThumbprint*应如下所示︰ AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*︰ 您自己选择的友好字符串标识符用于标识证书。  该名称用作*Microsoft.Web/certificates*实体表示的 SSL 证书 Azure 资源管理器的唯一标识符的一部分。  名称**必须**结束与以下后缀︰ \_yourASENameHere_InternalLoadBalancingASE。  证书用于保护 ILB 启用 ASE 观测的门户使用这个后缀。


*Azuredeploy.parameters.json*的缩写的示例如下所示︰


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

一旦已填写的*azuredeploy.parameters.json*文件，可使用下面的 Powershell 代码段配置默认的 SSL 证书。  更改文件路径，以匹配 Azure 资源管理器模板文件在您的计算机上的位置。  也请务必提供您自己的值 Azure 资源管理器部署名称和资源组的名称。

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

在 Azure 资源管理器中提交模板需要大约四十分钟分钟每 ASE 前端以应用更改。  例如，与使用两个前端、 缺省大小 ASE，模板需要约一小时二十分钟完成。  运行时模板 ASE 不能缩放。  

完成模板后，可以通过 HTTPS 访问 ILB ASE 的应用程序，将使用默认的 SSL 证书保护的连接。  使用的应用程序的名称以及默认主机名组合解决 ILB ASE 的应用程序时，将使用默认的 SSL 证书。  例如*https://mycustomapp.internal-contoso.com*将使用的默认 SSL 证书 **.internal contoso.com*。

但是，一样在公共的多租户服务上运行的应用程序，开发人员可以还配置为单个应用程序，自定义的主机名，然后配置单个应用程序的唯一 SNI SSL 证书绑定。  


## <a name="getting-started"></a>入门教程

若要开始使用应用程序服务的环境，请参见[应用程序服务环境介绍](app-service-app-service-environment-intro.md)

所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
