<properties
    pageTitle="部署模板 PowerShell Azure 堆栈中使用 |Microsoft Azure"
    description="了解如何部署虚拟机使用资源管理器模板和 PowerShell。"
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>部署使用 PowerShell 的 Azure 堆栈中的模板

使用 PowerShell 将 Azure 资源管理器模板部署到 Azure 堆栈 POC。  资源管理器模板部署和调配单、 协调操作中应用程序的所有资源。

## <a name="run-azurerm-powershell-cmdlets"></a>运行 AzureRM PowerShell cmdlet

在此示例中，您运行一个脚本来将虚拟机部署到 Azure 堆栈 POC 使用资源管理器模板。  继续之前，请确保您已[安装并配置 PowerShell](azure-stack-connect-powershell.md)  

本示例模板中使用 VHD 是默认市场图像 （WindowsServer 2012-R2-数据中心）。

1.  转到<http://aka.ms/AzureStackGitHub>，搜索**101 简单-windows 虚拟机**模板，并将其保存到下列位置︰ c:\\模板\\azuredeploy-101-简单的 windows-vm.json。

2.  继续，运行下面的部署脚本。 替换您的用户名和密码*的用户名*和*密码*。 在后续的使用，增加以防止覆盖您的部署的*$myNum*参数的值。

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  打开 Azure 堆栈门户网站，单击**浏览**，单击**虚拟机**，并寻找新的虚拟机 (*myDeployment001*)。

## <a name="video-example-hybrid-virtual-machine-deployment"></a>视频示例︰ 混合虚拟机部署

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>下一步行动

[部署与 Visual Studio 模板](azure-stack-deploy-template-visual-studio.md)
