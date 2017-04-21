<properties
    pageTitle="部署模板使用 Azure 堆栈中命令行 |Microsoft Azure"
    description="了解如何使用跨平台命令行界面 (CLI) 来部署模板从 ClientVM 内或之后使用 VPN 连接到 Azure 堆栈。"
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
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>部署模板中使用命令行的 Azure 堆栈

使用命令行来部署到 Azure 堆栈 POC 的 Azure 资源管理器模板。 Azure 的资源管理器模板部署和调配单、 协调操作中应用程序的所有资源。

## <a name="download-template"></a>下载模板        
若要测试部署的 CLI，下载的文件 azuredeploy.json 和 azuredeploy.parameters.json 从[创建存储帐户示例模板](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account)。

## <a name="deploy-template"></a>部署模板
导航到文件夹，这些文件被下载并运行下面的命令来部署模板︰

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

此命令将模板部署到资源组**cliRG** Azure 堆栈 POC 的默认位置。

## <a name="validate-template-deployment"></a>验证模板部署
若要查看此资源组和存储帐户，请使用以下命令︰

    azure group list

    azure storage account list

## <a name="next-steps"></a>下一步行动

[管理用户权限](azure-stack-manage-permissions.md)
