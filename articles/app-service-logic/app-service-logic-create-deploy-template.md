<properties
   pageTitle="创建逻辑应用程序部署模板 |Microsoft Azure"
   description="了解如何创建逻辑应用程序部署模板并将其发布管理"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>创建逻辑应用程序部署模板

在创建一个逻辑应用程序之后，可能要将其创建为 Azure 资源管理器模板。 这种方式，您可以轻松部署逻辑应用到任何环境或您可能需要的资源组。 有关资源管理器模板的介绍，一定要检查出[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)和[资源使用 Azure 资源管理器模板部署](../resource-group-template-deploy.md)上的文章。

## <a name="logic-app-deployment-template"></a>逻辑应用程序部署模板

逻辑应用程序有三个基本组件︰

* **逻辑应用程序资源**。 此资源包含定价计划、 位置和工作流定义等有关信息。
* **工作流定义**。 这是出现在代码视图中。 它包括定义的流和该引擎应如何执行的步骤。 这是`definition`逻辑应用程序资源的属性。
* **连接**。 以下是安全地存储任何连接器连接，如连接字符串和一个访问令牌的相关元数据的单独资源。 引用中的逻辑应用程序中的这些`parameters`的逻辑应用程序资源的部分。

使用类似[Azure 资源管理器](http://resources.azure.com)工具，可以查看所有现有的逻辑应用程序的这些零件。

要使逻辑应用程序能够使用资源组部署的模板，您需要定义的资源，并根据需要进行参数化处理。 例如，如果要部署到开发、 测试和生产环境，您可能需要在每个环境中使用不同的连接字符串转换为 SQL 数据库。 或者，您可能希望部署在不同的订阅或资源组。  

## <a name="create-a-logic-app-deployment-template"></a>创建逻辑应用程序部署模板

有一个有效的逻辑应用程序部署模板的最简单方法是使用[Visual Studio 工具，用于逻辑的应用程序](./app-service-logic-deploy-from-vs.md)。  Visual Studio 工具生成有效的部署模板，可在任何订阅或位置。

其他几个工具可以帮助您创建一个逻辑的应用程序部署模板时。 可以创作手动，就是通过使用已这里讨论创建参数，如所需的资源。 另一个选项是使用[逻辑应用程序模板创建者](https://github.com/jeffhollan/LogicAppTemplateCreator)PowerShell 模块。 本开放源代码模块首先计算逻辑应用程序以及任何连接，它使用的，然后生成模板资源与部署必需的参数。 例如，如果您有一个从 Azure 服务总线队列中接收消息并将数据添加到 SQL Azure 数据库的逻辑应用程序，该工具将保留的所有业务流程逻辑并参数化的 SQL 和服务总线的连接字符串，以便可以在部署中设置它们。

>[AZURE.NOTE] 连接必须是逻辑应用程序所在的资源组中。

### <a name="install-the-logic-app-template-powershell-module"></a>逻辑应用程序模板 PowerShell 模块安装

最简单的方法来安装本模块是通过[PowerShell 库](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)，通过使用命令`Install-Module -Name LogicAppTemplate`。  

也可以手动安装 PowerShell 模块︰

1. 下载最新版本的[逻辑应用程序模板创建者](https://github.com/jeffhollan/LogicAppTemplateCreator/releases)。  
1. 提取 PowerShell 模块文件夹中的文件夹 (通常`%UserProfile%\Documents\WindowsPowerShell\Modules`)。

模块使用任何租户和订阅的访问令牌，我们建议使用[ARMClient](https://github.com/projectkudu/ARMClient)命令行工具。  此[博客文章](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html)中更详细地讨论 ARMClient。

### <a name="generate-a-logic-app-template-by-using-powershell"></a>通过使用 PowerShell 生成逻辑应用程序模板

PowerShell 安装后，您可以通过使用下面的命令生成模板︰

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`Azure 的订阅 id。 此行第一次获取访问令牌通过 ARMClient，然后通过管道向 PowerShell 脚本中，然后在 JSON 文件中创建模板。

## <a name="add-parameters-to-a-logic-app-template"></a>将参数添加到逻辑应用程序模板

创建逻辑应用程序模板后，您可以继续添加或修改参数可能需要。 例如，如果您定义包括对 Azure 函数或嵌套的工作流，您打算部署的单个部署中的资源 ID，可以向模板中添加更多的资源并根据需要进行参数化处理 Id。 这同样适用于任何对自定义的 Api 或 Swagger 引用您希望部署与每个资源组的终结点。

## <a name="deploy-a-logic-app-template"></a>部署的逻辑应用程序模板

您可以使用任意数量的工具，包括 PowerShell，REST API，Visual Studio 版本管理和 Azure 门户模板部署部署您的模板。 请参见这篇文章有关的其他信息[资源使用 Azure 资源管理器模板部署](../resource-group-template-deploy.md)。 此外，建议您创建一个[参数文件](../resource-group-template-deploy.md#parameter-file)来存储该参数的值。

### <a name="authorize-oauth-connections"></a>OAuth 连接的用户授权

在部署之后，逻辑应用程序工作端到端使用有效的参数。 但是，OAuth 连接仍然需要获得授权，以生成有效的访问令牌。 您可以通过在设计器中打开该逻辑应用程序，然后授权连接来执行此操作。 或者，如果您要自动执行，您可以使用脚本同意 OAuth 的每个连接。 还有一个示例脚本在 GitHub 在[LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth)项目下。

## <a name="visual-studio-release-management"></a>Visual Studio 版本管理

部署和管理环境的一个常见方案是与逻辑应用程序部署模板使用 Visual Studio 的版本管理，如一种工具。 Visual Studio 团队服务包括[部署 Azure 资源组](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup)任务您可以添加到任何生成或释放管道。 您需要授权部署，[服务主体](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)，然后可以生成版本定义。

1. 在版本管理中，要创建一个新的定义，选择**空**着手定义为空。

    ![创建新的空定义][1]   

1. 选择所需的任何资源。 这可能是手动或作为生成过程的一部分生成的逻辑应用程序模板。
1. 添加**Azure 资源组部署**任务。
1. 配置与[服务主体](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)，并引用模板和模板参数文件。
1. 继续扩建中任何其他环境、 自动化的测试或审批者根据需要发布过程的步骤。

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
