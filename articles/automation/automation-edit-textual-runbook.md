<properties 
    pageTitle="编辑文本运行手册在 Azure 自动化"
    description="本文提供了不同过程使用 PowerShell 并且 PowerShell 工作流的运行手册，Azure 自动化中使用文本编辑器。"
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>编辑文本运行手册在 Azure 自动化

可以使用 Azure 自动化中的文本编辑器编辑[PowerShell 运行手册](automation-runbook-types.md#powershell-runbooks)和[PowerShell 工作流运行手册](automation-runbook-types.md#powershell-workflow-runbooks)。 这具有典型功能的智能感知和颜色编码具有其他特殊功能等其他代码编辑器可帮助您访问运行手册的公共资源。  这篇文章提供了为执行不同的功能，使用该编辑器的详细的步骤。

文本编辑器中包括活动、 资产和子运行手册代码插入 runbook 的功能。 而不是键入代码中您自己，可以从可用资源的列表中选择，并具有相应的代码插入到 runbook。

每个 runbook Azure 自动化中的有两个版本，草稿和已发布。 编辑 runbook 的草稿版本，以便可以执行发布它。 不能编辑已发布版本。 有关详细信息，请参阅[发布 runbook](automation-creating-importing-runbook.md#publishing-a-runbook) 。

若要使用[图形化的运行手册](automation-runbook-types.md#graphical-runbooks)，请参阅[图形创作 Azure 自动化中](automation-graphical-authoring-intro.md)。

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>若要编辑使用 Azure 的门户网站 runbook

使用下列步骤打开文本编辑器中编辑 runbook。

1. 在 Azure 的门户中，选择您的自动化帐户。
2. 单击打开列表运行手册，**运行手册**方块。
3. 单击您想要编辑，然后单击**编辑**按钮 runbook 的名称。
6. 执行所需的编辑。
7. 当完成编辑后，请单击**保存**。
8. 如果您想要发布 runbook 的最新草稿版本，请单击**发布**。

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>若要插入 runbook 的 cmdlet

2. 在画布上的文本编辑器中，将光标位置您想要放置该 cmdlet 的地方。
3. 展开库控件中的**Cmdlet**节点。 
3. 展开包含您要使用的 cmdlet 的模块。
4. 右键单击该 cmdlet 来插入并选择**添加到画布**。  如果该 cmdlet 将设置多个参数，则会添加的默认设置。  此外可以展开该 cmdlet 来选择不同的参数组。
4. 该 cmdlet 的代码插入参数与其整个列表。
5. 提供适当的值来代替括在大括号的任何所需参数的 <> 数据类型。  移除不需要的任何参数。

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>若要将子 runbook 的代码插入到 runbook

2. 在画布上的文本编辑器中，将光标位置想要放置[子 runbook](automation-child-runbooks.md)的代码。
3. 展开库控件中的**运行手册**节点。 
3. 右键单击要插入并选择**添加到画布**runbook。
4. 与任何 runbook 参数的任何占位符插入子 runbook 的代码。
5. 这些占位符替换为适当的值对于每个参数。

### <a name="to-insert-an-asset-into-a-runbook"></a>若要插入 runbook 的资产

2. 在画布上的文本编辑器中，将光标位置想要放置子 runbook 的代码。
3. 展开库控件中的**资产**节点。 
4. 展开所需的资产类型的节点。
3. 右键单击要插入并选择**添加到画布**的资产。  对于[变量资产](automation-variables.md)，具体取决于您是否要获取或设置变量选择**添加"获取变量"画布**或**添加"设置变量"为画布**。
4. 资产的代码插入到 runbook。



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>若要编辑使用 Azure 的门户网站 runbook

使用下列步骤打开文本编辑器中编辑 runbook。

1. 在 Azure 的门户中，选择**自动化**，然后再单击自动化帐户的名称。
2. 选择**运行手册**选项卡。
3. 单击您想要编辑，然后选择**作者**选项卡 runbook 的名称。
5. 单击屏幕底部的**编辑**按钮。
6. 执行所需的编辑。
7. 当完成编辑后，请单击**保存**。
8. 如果您想要发布 runbook 的最新草稿版本，请单击**发布**。

### <a name="to-insert-an-activity-into-a-runbook"></a>若要插入 Runbook 的活动

1. 在画布上的文本编辑器中，将光标位置您想要放置活动的地方。
1. 在屏幕的底部，单击**插入**，然后再**活动**。
1. 在**集成模块**列中，选择包含活动的模块。
1. 在**活动**窗格中，选择活动。
1. 在**说明**列中，请注意活动的描述。 或者，您可以单击查看详细的帮助，以帮助在浏览器中的活动。
1. 单击向右箭头。  如果活动有参数，它们将列出有关您的信息。
1. 单击检查按钮。  将 runbook 中插入代码以运行该活动。
1. 如果该活动需要参数，提供一个适当的值来代替括在大括号 <> 数据类型。

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>若要将子 runbook 的代码插入到 runbook

1. 在画布上的文本编辑器中，将光标位置您想要放置[子 runbook](automation-child-runbooks.md)的地方。
2. 在屏幕的底部，单击**插入**，然后再**Runbook**。
3. 选择 runbook 从中间栏插入并单击向右箭头。
4. 如果 runbook 参数，它们将列出有关您的信息。
5. 单击检查按钮。  运行选定的 runbook 代码将插入到当前的 runbook。
7. 如果 runbook 需要参数，提供了一个适当的值来代替括在大括号 <> 数据类型。

### <a name="to-insert-an-asset-into-a-runbook"></a>若要插入 runbook 的资产

1. 在画布上的文本编辑器中，将光标位置想要检索该资产的活动。
1. 在屏幕的底部，单击**插入**，然后再**设置**。
1. 在**设置操作**列中，选择所需的操作。
1. 选择从中间栏中的可用资源。
1. 单击检查按钮。  将 runbook 中插入代码以获取或设置该资产。



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>若要编辑使用 Windows PowerShell Azure 自动化 runbook

若要编辑与 Windows PowerShell runbook，使用您选择的编辑器，并将其保存到.ps1 文件。 可以使用[Get AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) cmdlet 检索 runbook 和[集 AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition)用于替换现有的草稿 runbook 修改后的内容。

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>若要检索使用 Windows PowerShell Runbook 中的内容

下面的示例命令显示如何检索 runbook 的脚本并将其保存到一个脚本文件。 在此示例中，检索的草稿版本。 还有可能，虽然不能更改此版本检索 runbook 的已发布版本。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>若要更改使用 Windows PowerShell Runbook 中的内容

下面的示例命令显示如何使用脚本文件的内容替换现有 runbook 的内容。 请注意，这与[要导入脚本文件与 Windows PowerShell runbook](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS)相同的示例过程。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>相关的文章

- [创建或导入了 Azure 自动化 runbook](automation-creating-importing-runbook.md)
- [学习 PowerShell 工作流](automation-powershell-workflow.md)
- [图形创作在 Azure 自动化](automation-graphical-authoring-intro.md)
- [证书](automation-certificates.md)
- [连接](automation-connections.md)
- [凭据](automation-credentials.md)
- [时间安排](automation-schedules.md)
- [变量](automation-variables.md)