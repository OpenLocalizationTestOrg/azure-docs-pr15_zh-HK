<properties
   pageTitle="开始使用 Azure 批 CLI |Microsoft Azure"
   description="在 Azure CLI 中获得管理 Azure 批服务资源的批处理命令简介"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>开始使用 Azure 批 CLI

跨平台 Azure 命令行界面 (Azure CLI) 使您能够管理您的批处理帐户和资源，如池、 作业和 Linux、 Mac 和 Windows 命令外壳中的任务。 使用 Azure CLI 时，可以执行和许多批处理 Api、 Azure 门户，和批 PowerShell cmdlet 执行相同任务的脚本。

本文基于 Azure CLI 版本 0.10.5。

## <a name="prerequisites"></a>系统必备组件

* [安装 Azure CLI](../xplat-cli-install.md)

* [将 Azure CLI 连接到 Azure 订阅](../xplat-cli-connect.md)

* 切换到**资源管理器模式**︰`azure config mode arm`

>[AZURE.TIP] 我们建议您更新您的 Azure CLI 安装经常以充分利用服务更新和增强功能。

## <a name="command-help"></a>命令帮助

可以通过附加在 Azure CLI 中显示的每个命令的帮助文本`-h`用作在该命令后的唯一选项。 例如︰

* 若要获得帮助的`azure`命令时，请输入︰`azure -h`
* 若要在 CLI 中获取所有批处理命令的列表，请使用︰`azure batch -h`
* 若要获取有关创建批处理帐户的帮助，请输入︰`azure batch account create -h`

当有疑问时，使用`-h`命令行选项来获得关于任何 Azure CLI 命令的帮助。

## <a name="create-a-batch-account"></a>创建一个批处理帐户

用法︰

    azure batch account create [options] <name>

示例︰

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

使用指定的参数创建新的批处理帐户。 您必须指定至少一个位置、 资源组和帐户名。 如果您没有一个资源组，创建一个运行`azure group create`，并指定某个 Azure 区域 （例如"西 US")`--location`选项。 例如︰

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] 批次帐户名称必须是唯一 Azure 创建帐户的区域内。 它可能包含小写字母数字字符，并且必须 3-24 个字符的长度。 您不能使用特殊字符，如`-`或`_`批次帐户名称中。

### <a name="linked-storage-account-autostorage"></a>链接的存储帐户 (autostorage)

（可选） 可以将**通用**存储帐户链接到批帐户，创建它时。 批次的[应用程序包](batch-application-packages.md)功能使用 blob 存储链接的通用存储帐户，[批处理文件约定.NET](batch-task-output.md)库也一样。 这些可选的功能帮助您部署应用程序运行批处理任务，并保持它们产生的数据。

若要链接到新批帐户现有 Azure 存储帐户，创建它时，请指定`--autostorage-account-id`选项。 此选项需要存储帐户的完全限定的资源 ID。

首先，显示您的存储帐户的详细信息︰

    azure storage account show --resource-group "resgroup001" "storageaccount001"

然后，使用**Url**值为`--autostorage-account-id`选项。 Url 值与"/ 订阅 /"开头，并包含存储帐户您的订阅 ID 和资源路径︰

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>删除批次帐户

用法︰

    azure batch account delete [options] <name>

示例︰

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

删除指定的批处理帐户。 出现提示时，确认您要删除的帐户 （帐户删除可能需要一些时间才能完成）。

## <a name="manage-account-access-keys"></a>管理帐户访问键

批次帐户中，您需要[创建和修改资源](#create-and-modify-batch-resources)访问键。

### <a name="list-access-keys"></a>列表中的访问键

用法︰

    azure batch account keys list [options] <name>

示例︰

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

列出给定批帐户的帐户项。

### <a name="generate-a-new-access-key"></a>生成一个新的访问键

用法︰

    azure batch account keys renew [options] --<primary|secondary> <name>

示例︰

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

将给定批帐户指定的帐户密钥重新生成。

## <a name="create-and-modify-batch-resources"></a>创建和修改批资源

您可以使用 Azure CLI 创建、 读取、 更新和删除 (CRUD) 批资源，如池、 计算节点、 作业和任务。 这些 CRUD 操作要求批帐户名、 访问键和终结点。 还可以指定这些与`-a`， `-k`，和`-u`选项或设置[环境变量](#credential-environment-variables)，CLI 使用自动 （如果填充）。

### <a name="credential-environment-variables"></a>凭据的环境变量

您可以设置`AZURE_BATCH_ACCOUNT`， `AZURE_BATCH_ACCESS_KEY`，和`AZURE_BATCH_ENDPOINT`而不是指定的环境变量`-a`， `-k`，和`-u`在执行每个命令的命令行选项。 批次 CLI 使用这些变量 (如果设置)，以便您可以省略`-a`， `-k`，和`-u`选项。 本文的其余部分假定使用这些环境变量。

>[AZURE.TIP] 列出您的密钥与`azure batch account keys list`，并显示该帐户的端点`azure batch account show`。

### <a name="json-files"></a>JSON 文件

当您创建批处理资源池和作业等时，可以指定包含新资源的配置，而不是传递任意命令行选项及其参数的 JSON 文件。 例如︰

`azure batch pool create my_batch_pool.json`

您还可以使用命令行选项只有许多资源创建操作，而有些功能需要包含资源详细信息的 JSON 格式的文件。 例如，如果您想要指定开始任务的资源文件必须使用 JSON 文件。

若要查找创建资源所必需的 JSON，指[批 REST API 参考][rest_api]在 MSDN 上的文档。 "添加*资源类型"*的每个主题创建资源，则可以使用 JSON 文件作为模板包含示例 JSON。 例如，创建池的 JSON 可以找到在[添加到帐户池][rest_add_pool]。

>[AZURE.NOTE] 如果在创建资源时指定一个 JSON 文件，您对该资源在命令行指定的所有其它参数将被忽略。

## <a name="create-a-pool"></a>创建池

用法︰

    azure batch pool create [options] [json-file]

示例 （虚拟机配置）︰

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

示例 （云服务配置）︰

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

在批处理服务创建池的计算节点。

[批处理功能概述](batch-api-basics.md#pool)中已经提到，有两个选项在您池中选择节点操作系统︰**虚拟机配置**和**云服务配置**。 使用`--image-*`选项来创建虚拟机配置池和`--os-family`创建云服务配置池。 不能同时指定`--os-family`和`--image-*`选项。

您可以指定池[应用程序包](batch-application-packages.md)和[启动任务](batch-api-basics.md#start-task)的命令行。 若要指定资源文件的开始任务，但是，您必须使用[JSON 文件](#json-files)。

删除的池︰

    azure batch pool delete [pool-id]

>[AZURE.TIP] 检查[虚拟机映像的列表](batch-linux-nodes.md#list-of-virtual-machine-images)值适用于`--image-*`选项。

## <a name="create-a-job"></a>创建作业

用法︰

    azure batch job create [options] [json-file]

示例︰

    azure batch job create --id "job001" --pool-id "pool001"

将作业添加到批处理帐户，并指定在其执行其任务的池。

删除作业︰

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>列出池、 作业、 任务和其他资源

每个批处理资源类型支持`list`查询批处理帐户，并列出了该类型的资源的命令。 例如，您可以在您的帐户，并在作业中的任务列出池︰

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>高效地列出资源

进行快速查询，您可以**选择**、**筛选器**和**扩展**子句为指定选项`list`操作。 使用这些选项来限制批服务返回的数据量。 因为所有筛选发生服务器端，只有您所感兴趣的数据交叉网线。 使用这些子句来节省带宽 （和因此时间） 当您执行的操作列表。

例如，这会返回只有其 id 以"renderTask"开头的池︰

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

批次 CLI 支持批处理服务支持的所有三个子句︰

* `--select-clause [select-clause]`返回每个实体的属性的子集
* `--filter-clause [filter-clause]`返回与指定的 OData 表达式匹配的实体
* `--expand-clause [expand-clause]`获得一个基础的其余部分调用中的实体信息。 在展开子句仅支持`stats`这次的属性。

有关的三个子句和与它们的列表查询的详细信息，请参阅[高效地查询 Azure 批服务](batch-efficient-list-queries.md)。

## <a name="application-package-management"></a>应用程序数据包管理

应用程序包提供简化的方法来部署应用程序池中的计算节点。 使用 Azure CLI 时，可以上载应用程序的包、 管理包版本和删除包。

若要创建新的应用程序并添加软件包版本︰

**创建**应用程序︰

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**添加**一个应用程序包︰

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**激活**此程序包︰

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

设置应用程序的**默认版本**︰

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>部署一个应用程序包

当您创建一个新池时，您可以指定用于部署的一个或多个应用程序包。 在池中创建时指定一个包时，它被部署到每个节点作为节点连接池。 在重新引导或重新映像的节点时，还将部署包。

指定`--app-package-ref`选项时创建池，池中加入到该池的节点部署一个应用程序包。 `--app-package-ref`选项接受以分号分隔的应用程序将部署到计算节点 id 列表。

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

通过使用命令行选项创建池时，您当前不能指定*哪些*应用程序软件包版本部署到计算节点，例如"1.10-beta3"。 因此，您必须首先指定的应用程序的默认版本`azure batch application set [options] --default-version <version-id>`在创建池之前 （请参见上一节）。 如果使用[JSON 文件](#json-files)而不是命令行选项在创建池时，但是，可以指定池的包版本。

[与 Azure 批处理应用程序包的应用程序部署](batch-application-packages.md)中，可以找到应用程序包的详细信息。

>[AZURE.IMPORTANT] 必须为要使用应用程序软件包的批处理帐户[链接 Azure 存储帐户](#linked-storage-account-autostorage)。

### <a name="update-a-pools-application-packages"></a>更新池的应用程序包

若要更新分配给现有池的应用程序，发出`azure batch pool set`命令与`--app-package-ref`选项︰

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

若要部署新的应用程序包，以计算现有库中已经存在的节点，必须重新启动或重新映射这些节点︰

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] 您可以获得与池中，以及它们的节点 id 的节点的列表`azure batch node list`。

请记住，您必须已配置应用程序在部署之前的默认版本 (`azure batch application set [options] --default-version <version-id>`)。

## <a name="troubleshooting-tips"></a>故障排除提示

本部分旨在为您提供在 Azure CLI 问题进行故障排除时要使用的资源。 它不一定可以解决所有问题，但它可以帮助您缩小原因，并指导您要帮助的资源。

* 使用`-h`以获取所有 CLI 命令的**帮助文本**

* 使用`-v`和`-vv`来显示**详细**的命令输出;`-vv`是"额外"的详细信息，并显示实际的其余请求和响应。 这些开关是以便显示完整的错误输出。

* 您可以查看**命令输出作为 JSON**与`--json`选项。 例如，`azure batch pool show "pool001" --json`以 JSON 格式显示 pool001 的属性。 然后可以复制和修改该输出中使用`--json-file`（请参阅本文前面的[JSON 文件](#json-files)）。

* [在 MSDN 上的批次论坛][batch_forum]是一个非常有用的资源，并由批工作组成员密切监控。 一定要有发布您的问题，如果您遇到问题或想要使用特定操作的帮助。

* 不是每个批次资源操作当前受 Azure CLI 支持。 例如，当前不能指定池，只包 id。 应用程序软件包*版本* 在这种情况下，您可能需要提供`--json-file`为您的命令而不是使用命令行选项。 请务必保持最新状态与最新的 CLI 版本，以挑选未来的功能增强。

## <a name="next-steps"></a>下一步行动

*  请参阅以了解如何使用此功能来管理和部署在批处理计算节点执行的应用程序[与 Azure 批处理应用程序包的应用程序部署](batch-application-packages.md)。

* 请参阅[高效地查询批处理服务](batch-efficient-list-queries.md)更减少项目数和查询的退回批次的信息的类型。

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx