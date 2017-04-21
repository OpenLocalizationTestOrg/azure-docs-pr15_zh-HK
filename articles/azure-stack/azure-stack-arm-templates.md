<properties
    pageTitle="在 Azure 堆栈 （租户开发人员） 使用 Azure 资源管理器模板 |Microsoft Azure"
    description="了解如何在 Azure 堆栈中使用 Azure 资源管理器模板部署和调配所有单个、 协调操作中应用程序的资源。"
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
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>在 Azure 堆栈中使用 Azure 资源管理器模板

Azure 的资源管理器模板部署和调配所有单个、 协调操作中应用程序的资源。 定义应用程序和部署方式的资源。  您还可以重新部署模板进行更改，该资源组中的资源。

可以将这些模板部署 Microsoft Azure 堆栈门户，PowerShell、 命令行和 Visual Studio。

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

以下模板是在[GitHub](http://aka.ms/azurestackgithub)上可用︰

## <a name="deploy-sharepoint-non-high-availability"></a>部署 SharePoint （非高可用性）

使用 PowerShell DSC 扩展来创建 SharePoint 2013 场，其中包括以下︰

-   虚拟网络

-   三个存储帐户

-   两个外部负载平衡器

-   一个虚拟机配置为单个域与一个新目录林中的域控制器

-   一个虚拟机配置为 SQL Server 2014年独立服务器

-   一个虚拟机配置为一台计算机 SharePoint 2013 场

## <a name="deploy-ad-non-high-availability"></a>部署 AD （非高可用性）

使用 PowerShell DSC 扩展名创建包括以下 AD 域控制器服务器︰

-   虚拟网络

-   一个存储帐户

-   一个外部负载平衡器

-   一个虚拟机配置为单个域与一个新目录林中的域控制器

## <a name="deploy-adsql-non-high-availability"></a>部署 AD/SQL （非高可用性）

使用 PowerShell DSC 扩展来创建 SQL Server 2014年独立服务器，包括下列︰

-   虚拟网络

-   两个存储帐户

-   一个外部负载平衡器

-   一个虚拟机配置为单个域与一个新目录林中的域控制器

-   一个虚拟机配置为 SQL Server 2014年独立服务器

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

使用 PowerShell DSC 扩展配置现有虚拟机本地配置管理器 (LCM) 并将其注册到 Azure 自动化帐户 DSC 拉服务器。

## <a name="create-a-virtual-machine-from-a-user-image"></a>从用户映像创建虚拟机

从自定义用户映像创建虚拟机。 该模板还部署 （使用 DNS) 的虚拟网络、 公用 IP 地址和网络接口。

## <a name="simple-vm"></a>简单的虚拟机

部署简单 Windows 虚拟机，包括 （使用 DNS) 的虚拟网络、 公用 IP 地址和网络接口。

## <a name="cancel-a-running-template-deployment"></a>取消正在运行的模板部署

若要取消正在运行的模板部署，使用`Stop-AzureRmResourceGroupDeployment`PowerShell cmdlet。


## <a name="next-steps"></a>下一步行动

[将模板与该门户部署](azure-stack-deploy-template-portal.md)

[Azure 的资源管理器概述](../azure-resource-manager/resource-group-overview.md)

