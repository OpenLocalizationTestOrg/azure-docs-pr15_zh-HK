<properties
   pageTitle="从控制器迁移到 Azure 自动化 |Microsoft Azure"
   description="描述如何将系统中心控制器运行手册和集成包迁移到 Azure 自动化。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>从控制器迁移到 Azure 自动化 （测试版）

在[系统中心控制器](http://technet.microsoft.com/library/hh237242.aspx)运行手册根据活动虽然是在 Azure 自动化运行手册基于 Windows PowerShell 编写专门为控制器的集成包。  Azure 自动化中的[图形化运行手册](automation-runbook-types.md#graphical-runbooks)有控制器运行手册到类似的外观与他们表示 PowerShell cmdlet、 子运行手册和资产的活动。

[系统中心控制器迁移 Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all)包括工具来帮助您从通讯到 Azure 自动化转换运行手册。  转换本身运行手册，除了必须转换为与 Windows PowerShell cmdlet 的集成模块的集成包运行手册使用的活动。  

下面是转换到 Azure 自动化控制器运行手册的基本过程。  以下各节将详细介绍上述每个步骤。

1.  下载该[系统中心控制器迁移 Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all)包含的工具和本文中讨论的模块。
2.  将[标准活动模块](#standard-activities-module)导入到 Azure 的自动化。  这包括可能由转换运行手册的标准通讯活动的转换的版本。
3.  这些集成包使用的运行手册，访问系统中心[系统中心控制器集成模块](#system-center-orchestrator-integration-modules)导入 Azure 自动化中。
4.  将自定义以及第三方集成包使用[集成包转换器](#integration-pack-converter)转换并导入到 Azure 自动化。
5.  转换使用[Runbook 转换器](#runbook-converter)的控制器运行手册并在 Azure 自动化安装。
6.  手动在 Azure 自动化中创建所需的通讯资产由于 Runbook 转换器不转换这些资源。
7.  在您的本地数据中心运行转换的运行手册，它将访问本地资源配置[混合 Runbook 工作人员](#hybrid-runbook-worker)。

## <a name="service-management-automation"></a>服务管理自动化

[服务管理自动化](http://technet.microsoft.com/library/dn469260.aspx)(SMA) 存储和运行控制器，如您的本地数据中心中运行手册，它将 Azure 自动化使用相同的集成模块。 [Runbook 转换器](#runbook-converter)转换控制器运行手册为图形化运行手册虽然在 SMA 中不受支持的。  您仍可以安装的[标准活动模块](#standard-activities-module)和[系统中心控制器集成模块](#system-center-orchestrator-integration-modules)到 SMA，但是您必须手动[重新编写运行手册](http://technet.microsoft.com/library/dn469262.aspx)。

## <a name="hybrid-runbook-worker"></a>混合的 Runbook 工作人员

在控制器中运行手册都存储在数据库服务器上并运行 runbook 在服务器上，同时在您的本地数据中心中。  在 Azure 自动化运行手册在 Azure 云存储中，可以使用[混合 Runbook 辅助](automation-hybrid-runbook-worker.md)本地数据中心中运行。  这是您通常的运行方式转换控制器中，因为它们被设计为运行在本地服务器上运行手册。

## <a name="integration-pack-converter"></a>集成包转换器

集成包转换器转换创建使用[控制器集成 Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx)到基于 Windows PowerShell 可以导入到 Azure 自动化或服务管理自动化的集成模块的集成包。  

集成包转换器运行时，系统将显示一个向导，它将允许您选择一个集成软件包 (.oip) 文件中。  然后，向导列出了该集成包中包含的活动，并允许您选择将迁移。  在完成向导时，它创建为每个原始的集成包中的活动包括相应的 cmdlet 集成模块。


### <a name="parameters"></a>参数

集成包中的活动的任何属性都转换为相应的 cmdlet 集成模块中的参数。  Windows PowerShell cmdlet 都有一套可用于所有的 cmdlet 的[通用参数](http://technet.microsoft.com/library/hh847884.aspx)。  例如，-详细参数会导致用于输出它的操作的详细的信息。  没有 cmdlet 可能必须具有公共的参数相同的名称的参数。  如果一个活动有带有公共参数相同的名称的属性，向导将提示您提供该参数的另一个名称。

### <a name="monitor-activities"></a>监视活动

控制器中的监视器运行手册与[监视活动](http://technet.microsoft.com/library/hh403827.aspx)启动和运行持续等待被调用的特定事件。  Azure 自动化不支持监视器运行手册，因此将不会转换任何集成包中的监视活动。  相反，监视活动的集成模块中创建占位符 cmdlet。  此 cmdlet 都有任何功能，但它允许使用它来安装任何转换后的 runbook。  此 runbook 将不能运行在 Azure 自动化，但它可以安装，以便您可以修改它。

### <a name="integration-packs-that-cannot-be-converted"></a>不能转换的集成包

与集成包转换器无法转换集成包通过 OIT 不创建的。 也有一些集成包，使用该工具由 Microsoft 当前不能转换。  这样的集成包转换后的版本已经[可供下载](#system-center-orchestrator-integration-modules)，以便它们可以在 Azure 自动化或服务管理自动化安装。


## <a name="standard-activities-module"></a>标准活动模块

控制器包含一组[标准的活动](http://technet.microsoft.com/library/hh403832.aspx)，不包括在集成包，而由许多运行手册。  标准活动模块是为每种活动包括等效 cmdlet 集成模块。  在导入任何转换的运行手册，使用标准的活动之前，必须在 Azure 自动化安装此集成模块。

除了支持转换的运行手册，标准活动模块中的 cmdlet 可以用于被人熟悉控制器生成运行在 Azure 自动化的新手册。  使用 cmdlet 可以执行所有标准的活动的功能，而他们可能会以不同的方式运行。  在已转换的标准活动模块的 cmdlet 将工作及其相应的活动相同并使用相同的参数。  这可以帮助他们过渡到 Azure 自动化运行手册中现有的控制器 runbook 作者。

## <a name="system-center-orchestrator-integration-modules"></a>系统中心控制器集成模块

Microsoft 提供了用于生成运行手册来自动化系统中心组件和其他产品的[集成包](http://technet.microsoft.com/library/hh295851.aspx)。  这些集成包的一些当前基于 OIT，但目前不能转换由于已知问题为集成模块。  [系统中心控制器集成模块](https://www.microsoft.com/download/details.aspx?id=49555)包括以下这些可以导入到 Azure 自动化和服务管理自动化的集成包的转换的版本。  

通过此工具的 RTM 版本，将发布基于与集成包转换器 OIT 是可转换的集成包的更新的版本。  此外将提供指导以帮助您转换运行手册使用不基于 OIT 集成包中的活动。

## <a name="runbook-converter"></a>Runbook 转换器

Runbook 转换器转换为[图形的运行手册](automation-runbook-types.md#graph-runbooks)，可以导入到 Azure 自动化控制器运行手册。  

Runbook 转换器是用调用中执行的转换的**ConvertFrom SCORunbook** cmdlet 作为 PowerShell 模块实现的。  安装该工具时，它将创建一个快捷方式加载该 cmdlet PowerShell 会话。   

下面是转换控制器 runbook，并将其导入到 Azure 自动化的基本过程。  以下各节提供使用的工具和处理转换运行手册的进一步的详细信息。

1. 从控制器中导出一个或多个运行手册。
2. 获得 runbook 中的所有活动的集成模块。
3. 将转换控制器运行手册中导出的文件。
4. 查看日志，以验证转换中的信息并确定任何所需的人工任务。
4. 导入转换到 Azure 自动化的运行手册。
5. 在 Azure 自动化中创建任何必需的资产。
6. 编辑 runbook 在 Azure 自动化修改任何所需的活动。

### <a name="using-runbook-converter"></a>使用 Runbook 转换器

**ConvertFrom SCORunbook**的语法如下所示︰

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath-包含运行手册要转换的导出文件的路径。
- 模块的以逗号分隔的集成模块包含运行手册中的活动的列表。
- OutputFolder-要创建的文件夹路径转换图形运行手册。 


下面的示例命令将运行手册中名为**MyRunbooks.ois_export**的导出文件。  这些运行手册使用 Active Directory 和 Data Protection Manager 集成包。

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>日志文件

Runbook 转换器将在转换后的 runbook 相同的位置创建下列日志文件。  如果文件已经存在，然后他们将被覆盖上一次转换中的信息。

| 文件 | 目录 |
|:---|:---|
| Runbook 转换器-Progress.log | 转换包括成功转换的每个活动的信息和警告的未转换的每个活动的详细的步骤。 |
| Runbook 转换器-Summary.log  | 最后一个转换包括所有警告的摘要，然后按照需要创建所需的转换后的 runbook 变量如执行的任务。  |

### <a name="exporting-runbooks-from-orchestrator"></a>从控制器中导出运行手册

Runbook 转换器适用于导出文件中包含一个或多个运行手册的控制器中。  它将创建导出文件中的每个控制器 runbook 对应 Azure 自动化 runbook。  

要导出 runbook 控制器，用鼠标右键单击 runbook Runbook 设计器中的名称并选择**导出**。  要导出的文件夹中的所有运行手册，请用鼠标右键单击该文件夹的名称并选择**导出**。


### <a name="runbook-activities"></a>Runbook 活动

Runbook 转换器转换为相应的活动在 Azure 自动化控制器 runbook 中的每个活动。  对于不能转换这些活动，警告文本与 runbook 中创建一个占位符活动。  将转换后的 runbook 导入到 Azure 自动化后，您必须替换任何这些活动有效执行所需的功能的活动。

将转换[活动的标准模块](#standard-activities-module)中的任何通讯活动。  有一些标准的通讯活动，尽管不是本模块并不会转换。  例如，**发送平台事件**有没有等效的 Azure 自动化，因为事件是特定于控制器。

[监视活动](https://technet.microsoft.com/library/hh403827.aspx)不会转换，因为它们在 Azure 自动化中没有等效项。  该异常是当[转换集成包](#integration-pack-converter)将被转换为占位符活动监视活动。

如果使用**模块**参数提供集成模块的路径将转换从[转换集成包](#integration-pack-converter)的任何活动。  对于系统中心集成包，您可以使用[系统中心控制器集成模块](#system-center-orchestrator-integration-modules)。


### <a name="orchestrator-resources"></a>控制器的资源

Runbook 转换器只能转换运行手册，不是如计数器、 变量或连接其他通讯资源。  在 Azure 自动化中不支持的计数器。  支持变量和连接，但您必须手动创建它们。  日志文件将通知您，如果 runbook 需要这种资源并指定需要创建在转换后的 runbook，都能正常运行的 Azure 自动化中的相应资源。

例如，runbook 可以使用变量来填充在活动中的特定值。  转换后的 runbook 将转换活动和通讯变量同名 Azure 自动化中指定变量的资产。  这将创建在转换后的**Runbook 转换器-Summary.log**文件中所述。  您将需要手动创建此变量资产在 Azure 自动化中使用 runbook 之前。 

 
### <a name="input-parameters"></a>输入的参数

在控制器中的运行手册接受输入的参数**初始化数据**活动。  如果要转换成 runbook 包含此活动，在活动中每个参数创建 Azure 自动化 runbook 中的[输入的参数](automation-graphical-authoring-intro.md#runbook-input-and-output)。  在转换后的 runbook 检索并返回每个参数创建一个[工作流脚本控制](automation-graphical-authoring-intro.md#activities)活动。  在 runbook 中使用输入的参数的任何活动是指输出从该活动。

这种策略使用的原因是为了最好地反映控制器 runbook 中的功能。  新的图形化运行手册中的活动应参考直接输入，使用 Runbook 输入的数据源的参数。


### <a name="invoke-runbook-activity"></a>调用 Runbook 活动

在控制器中的运行手册与**调用 Runbook**活动启动其他运行手册。 如果要转换成 runbook 包含此活动，并且**等待完成**选项设置，然后 runbook 活动是为其创建在转换后的 runbook。  如果未设置**等待完成**选项，则工作流脚本活动会创建用于启动 runbook**开始 AzureAutomationRunbook** 。  将转换后的 runbook 导入到 Azure 自动化后，您必须指定如果在活动中的信息修改此活动。




## <a name="related-articles"></a>相关的文章

- [System Center 2012-控制器](http://technet.microsoft.com/library/hh237242.aspx)
- [服务管理自动化](https://technet.microsoft.com/library/dn469260.aspx)
- [混合的 Runbook 工作人员](automation-hybrid-runbook-worker.md)
- [通讯标准活动](http://technet.microsoft.com/library/hh403832.aspx)
- [下载系统中心控制器迁移 Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
