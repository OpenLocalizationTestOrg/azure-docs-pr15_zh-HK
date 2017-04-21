<properties
    pageTitle="工具与 PaaS 服务的 Azure 堆栈 |Microsoft Azure"
    description="了解如何开始使用 Azure 堆栈中的 PaaS 服务。"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Azure 堆栈的工具

您可以下载下面介绍的工具。 如果您想要得到的新的服务和工具，按照 #AzureStack 在 Twitter 上。

## <a name="template-tools"></a>模板工具

### <a name="azure-stack-github-templates"></a>Azure 堆栈 Github 模板
探索发展[Azure 堆栈 GitHub 模板](https://github.com/Azure/AzureStack-QuickStart-Templates)集合。 就像[Azure](https://github.com/Azure/azure-quickstart-templates)，这些"快速启动"Azure 资源管理器模板旨在入门简单的构建基块和示例，可以在 Microsoft Azure 堆栈技术预览证明的概念环境上部署。 包括第一方的工作负荷示例[ad-非-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha)， [sql-2014年-非-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha)， [sharepoint-2013年-非-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha)、 以及几个简单的 101 模板喜欢[101 简单 windows vm](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm)。


### <a name="marketplace-item-packaging-tool-and-sample"></a>市场上项打包工具和示例
[下载和使用此包装工具](http://www.aka.ms/azurestackmarketplaceitem)来创建您自己的自定义模板添加到 Azure 堆栈市场的市场项目。 在[项目中创建的市场](azure-stack-create-and-publish-marketplace-item.md)可以找到有关如何创建市场项并将其提供给您的承租人。

## <a name="developer-tools"></a>开发人员工具


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>使用 Visual Studio 和 Azure 堆栈 TP2 MAS CON01 虚拟机上
如果您想要使用 Visual Studio 在 VM 控制台上使用 Azure 堆栈模板，则必须安装所需的工具的正确版本。 使用以下过程为 TP2 安装受支持的版本。

1. 使用远程桌面连接登录到 MA CON01 虚拟机与 azurestack\azurestackadmin 凭据。
2. 安装并打开 Web 平台安装程序。
3. 查找和安装**Visual Studio 社区 2015 与 Microsoft Azure SDK-2.9.5**。
4. 使用**添加/删除程序**，卸载 SDK 的一部分作为获取安装**Microsoft Azure PowerShell （9 月）** 。
5. 打开 Web 平台安装程序。
6. 查找和安装**Microsoft Azure PowerShell 的 Azure 堆栈技术预览 2**。 
7. 重新启动 MAS CON01 虚拟机。
8. 使用远程桌面连接登录到 MA CON01 虚拟机与 azurestack\azurestackadmin 凭据。
9. 打开 Visual Studio 并验证，您可以连接到 Azure 堆栈环境、 获取模板，等等。 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell 是提供管理 Azure 和 Azure 堆栈与 Windows PowerShell 的 cmdlet 的模块。 可以使用 cmdlet 创建、 测试、 部署和管理的解决方案和通过堆栈 Azure 平台提供的服务。
[下载 Azure PowerShell SDK](http://aka.ms/azStackPsh)并[安装 PowerShell](azure-stack-connect-powershell.md)。

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure 跨平台命令行界面
快速安装 Azure 命令行界面 (Azure CLI) 用于创建和管理 Microsoft Azure 堆栈中的资源使用一套开源的基于 shell 的命令。

[下载 Windows CLI](http://aka.ms/azstack-windows-cli)

[下载的 Mac CLI](http://aka.ms/azstack-linux-cli)

[下载 Linux CLI](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + 如果你在 Mac 或 Linux 的机器上，还可以通过使用命令获得 CLI`npm install -g azure-cli@0.9.11`</br>
> + 如果您收到了证书验证问题，运行命令`set NODE_TLS_REJECT_UNAUTHORIZED=0`
