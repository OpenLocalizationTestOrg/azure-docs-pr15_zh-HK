<properties
    pageTitle="部署 web 应用程序使用主机名和 ssl 证书使用 MSDeploy"
    description="使用 Azure 资源管理器模板来部署 web 应用程序使用 MSDeploy 和设置自定义的主机名和 SSL 证书"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>部署 web 应用程序与 MSDeploy、 自定义的主机名和 SSL 证书

本指南介绍的 Azure Web 应用程序创建一个端到端部署、 利用 MSDeploy 以及 ARM 模板中添加自定义的主机名和 SSL 证书。

有关创建模板的详细信息，请参阅[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)。

###<a name="create-sample-application"></a>创建示例应用程序

您将部署 ASP.NET web 应用程序。 第一步是创建一个简单的 web 应用程序 （或者您可以选择使用现有-在这种情况下，您可以跳过此步骤）。

打开 Visual Studio 2015年，然后选择文件 > 新建项目。 在出现的对话框中，选择 Web > ASP.NET Web 应用程序。 在模板下选择 Web，然后选择 MVC 模板。 选择_更改身份验证类型_设置为_无身份验证_。 这是只是为了使该示例应用程序尽可能简单。

此时，您需要基本 ASP.Net web 应用程序可以使用作为部署过程的一部分。

###<a name="create-msdeploy-package"></a>创建 MSDeploy 包

下一步是创建包将使 web 应用程序部署到 Azure。 若要执行此操作，保存项目，然后运行以下命令从命令行︰

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

这将创建的 PackageLocation 文件夹下的压缩的包。 应用程序现已准备就绪可以部署它现在可以构建出一个 Azure 资源管理器模板来做到这一点。

###<a name="create-arm-template"></a>创建 ARM 模板
首先，让我们从基本的 ARM 模板将创建一个 web 应用程序和宿主的计划 （请注意，参数和变量不显示为简洁起见） 开始。

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

接下来，您需要修改 web 应用程序资源使嵌套的 MSDeploy 资源。 这将允许您引用包以前创建并告诉 Azure 资源管理器使用 MSDeploy 来将包部署到 Azure WebApp。 以下代码显示了具有嵌套的 MSDeploy 资源的 Microsoft.Web/sites 资源︰

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

现在，您会看到 MSDeploy 资源采用**能**属性定义，如下所示︰

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

这**能**将存储帐户 uri 指向存储帐户，将上载到您包的 zip。 Azure 资源管理器将利用[共享访问签名](../storage/storage-dotnet-shared-access-signature-part-1.md)包向下拉动本地存储帐户从部署模板时。 此过程将自动通过 PowerShell 将上载程序包并调用 Azure 管理 API 来创建这些项的脚本需要和将这些插入模板参数 （*_artifactsLocation*和*_artifactsLocationSasToken*） 作为参数传递。 您将需要为该文件夹定义参数和包的文件名上载到的存储容器下。

接下来，您需要添加另一个嵌套的资源来设置主机名绑定，可以利用自定义的域中。 您首先需要确保您拥有主机名，并将其设置为验证通过 Azure，拥有它-请参阅[配置自定义域名在 Azure 应用程序服务](web-sites-custom-domain-name.md)。 完成后，就可以到 Microsoft.Web/sites 资源部分下模板中添加以下︰

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

最后，您需要添加另一个顶层级别的资源，Microsoft.Web/certificates。 此资源将包含您的 SSL 证书，将存在于您的 web 应用程序和托管计划在同一个级别。

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

您需要具有有效的 SSL 证书，才能设置此资源。 一旦具有有效证书，则需要为 base64 字符串提取 pfx 字节。 这中提取的一种选择是使用下面的 PowerShell 命令︰

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

您无法再这样作为参数传递给 ARM 部署模板。

此时 ARM 模板已准备就绪。

###<a name="deploy-template"></a>部署模板

最后的步骤是这组合在一起拼凑成完整的端到端部署。 若要更容易，您可以利用在 Visual Studio 中，以帮助进行上载的模板中所需的任何项目创建一个 Azure 资源组项目时添加**部署 AzureResourceGroup.ps1** PowerShell 脚本部署。 它要求您已创建了想要提前使用的存储帐户。 在本例中，我创建了 package.zip 要上载的共享的存储帐户。 该脚本将利用 AzCopy 将包上传到存储帐户。 在您的项目文件夹位置传递，脚本会自动将该目录中的所有文件都上载到已命名的存储容器。 在调用部署 AzureResourceGroup.ps1 后必须更新映射自定义主机名与 SSL 证书的 SSL 绑定。

以下 PowerShell 显示调用部署完成部署-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

此时您的应用程序应该已部署，您应该能够浏览到它通过 https://www.yourcustomdomain.com
