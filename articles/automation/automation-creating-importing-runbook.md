<properties
    pageTitle="创建或导入了 Azure 自动化 runbook"
    description="本文介绍如何在 Azure 自动化中创建新的 runbook 或导入一个文件中。"
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>创建或导入了 Azure 自动化 runbook

您可以添加 runbook Azure 自动化，通过[创建一个新](#creating-a-new-runbook)或通过从文件或[Runbook 库](automation-runbook-gallery.md)中导入现有的 runbook。 这篇文章提供了有关创建和从文件中导入运行手册的信息。  您可以获取所有访问社区运行手册和[Azure 自动化 Runbook 和模块库](automation-runbook-gallery.md)中的模块的详细信息。

## <a name="creating-a-new-runbook"></a>创建新的 runbook

在使用 Windows PowerShell 的 Azure 的门户网站之一的 Azure 自动化中，您可以创建新的 runbook。 一旦创建了 runbook，您可以编辑使用[学习 PowerShell 流](automation-powershell-workflow.md)和[图形创作在 Azure 自动化](automation-graphical-authoring-intro.md)中的信息。

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>使用来创建新的 Azure 自动化 runbook Azure 经典门户

可以仅使用[PowerShell 工作流运行手册](automation-runbook-types.md#powershell-workflow-runbooks)在 Azure 的门户。

1. 在 Azure 经典的门户中，单击，**新****的应用程序服务**、**自动化**、 **Runbook**、**快速创建**。
2. 输入所需的信息，然后单击**创建**。 Runbook 名称必须以字母开头，并且可以有字母、 数字、 下划线和破折号。
3. 如果您想要现在编辑 runbook，然后单击**编辑 Runbook**。 否则，请单击**确定**。
4. **运行手册**选项卡将显示您新的 runbook。


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>使用来创建新的 Azure 自动化 runbook Azure 门户

1. 在 Azure 的门户中，打开您自动化的帐户。
2. 单击打开列表运行手册，**运行手册**方块。
3. 单击**添加 runbook**按钮，然后**创建新的 runbook**。
2. 键入 runbook 的**名称**并选择其[类型](automation-runbook-types.md)。 Runbook 名称必须以字母开头，并且可以有字母、 数字、 下划线和破折号。
3. 单击**创建**创建 runbook 和打开编辑器。


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>若要使用 Windows PowerShell 创建新的 Azure 自动化 runbook

[新 AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) cmdlet 可用于创建空的[工作流 PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks)。 您可以指定**Name**参数来创建空的 runbook，您可以以后编辑，或者您可以指定要导入的 runbook 文件的**路径**参数。 此外应包括**类型**参数来指定四种 runbook 类型之一。

下面的示例命令显示如何创建新的空 runbook。

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>将 runbook 从文件导入到 Azure 自动化

通过导入 PowerShell 脚本或 PowerShell 流 （.ps1 扩展） 或导出图形 runbook (.graphrunbook)，可以在 Azure 自动化中创建新的 runbook。  必须指定[类型的 runbook](automation-runbook-types.md)将创建的导入下列事项考虑在内。

- .Graphrunbook 文件可能仅导入新[图形 runbook](automation-runbook-types.md#graphical-runbooks)，和.graphrunbook 文件中只能创建图形化运行手册。
- 包含 PowerShell 流的.ps1 文件可以只导入[PowerShell 流 runbook](automation-runbook-types.md#powershell-workflow-runbooks)。  如果该文件包含多个 PowerShell 工作流，则导入将失败。 必须将每个工作流保存到其自己的文件，并分别导入每个。
- 不包含工作流的.ps1 文件可以导入[PowerShell runbook](automation-runbook-types.md#powershell-runbooks)或[runbook PowerShell 工作流](automation-runbook-types.md#powershell-workflow-runbooks)中。  如果导入 PowerShell 流 runbook，然后它将被转换为工作流程，和注释将包含在 runbook 指定所做的更改。

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>从 Azure 传统门户网站文件导入 runbook
下面的过程可用于将脚本文件导入到 Azure 自动化。  请注意，仅导入使用此门户 PowerShell 流 runbook 的.ps1 文件。  对于其他类型，必须使用 Azure 的门户。

1. 在 Azure 管理门户中，选择**自动化**，然后选择自动化帐户。
2. 单击**导入**。
3. 单击**浏览文件**，然后找到要导入的脚本文件。
4. 如果您想要现在编辑 runbook，然后单击**编辑 Runbook**。 否则，请单击确定。
5. 新的 runbook 将**运行手册**选项卡上显示自动化帐户。
6. 您可以运行它之前，必须[将发布 runbook](#publishing-a-runbook) 。


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>从 Azure 门户网站文件导入 runbook
下面的过程可用于将脚本文件导入到 Azure 自动化。  

>[AZURE.NOTE] 请注意，仅导入使用门户 PowerShell 流 runbook 的.ps1 文件。

1. 在 Azure 的门户中，打开您自动化的帐户。
2. 单击打开列表运行手册，**运行手册**方块。
3. 单击**添加 runbook**按钮，然后**导入**。
4. 单击**Runbook 文件**来选择要导入的文件
2. 如果启用了**名称**字段，则必须使用选项来更改它。  Runbook 名称必须以字母开头，并且可以有字母、 数字、 下划线和破折号。
3. 将自动选择的[runbook 类型](automation-runbook-types.md)，但可以采取考虑适用限制之后更改的类型。 
3. 新 runbook 自动化帐户将出现在列表中运行手册。
4. 您可以运行它之前，必须[将发布 runbook](#publishing-a-runbook) 。

>[AZURE.NOTE] 导入图形的 runbook 或图形的 PowerShell 流 runbook 之后，您可以选择要转换为其他类型，如果想要。 您不能将转换为文本。

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>要从 Windows PowerShell 的脚本文件中导入 runbook

可以使用[导入 AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) cmdlet 草稿 PowerShell 流 runbook 作为导入脚本文件。 如果 runbook 已经存在，导入将失败，除非您使用*-强制*参数。 

下面的示例命令显示如何将脚本文件导入 runbook。

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>发布 runbook

当您创建或导入新的 runbook 时，必须将其发布，然后才能运行它。  每个 runbook 自动化中的有草稿和已发布的版本。 只有已发布版本可以运行和可编辑的草稿版本。 已发布版本不会受到影响的草稿版本所做的任何更改。 时应将草稿版本可用，则将发布它将覆盖已发布版本的草稿版本。

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>若要发布使用 Azure 传统门户网站 runbook

1. 在 Azure 经典门户打开 runbook。
1. 在屏幕的顶部，单击**创作**。
1. 在屏幕的底部，单击**发布**，然后**是**为验证消息。

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>若要发布使用 Azure 门户 runbook

1. 在 Azure 的门户打开 runbook。
1. 单击**编辑**按钮。
1. **发布**按钮单击，然后单击**是**确认邮件到。


## <a name="to-publish-a-runbook-using-windows-powershell"></a>若要发布使用 Windows PowerShell runbook

可以使用[发布 AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) cmdlet 发布 Windows PowerShell 与 runbook。 下面的示例命令显示如何发布示例 runbook。

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>下一步行动
- 了解您可以怎样利用 Runbook 和 PowerShell 模块库，请参阅[Runbook 和模块库的 Azure 自动化](automation-runbook-gallery.md)
- 若要了解有关使用文本编辑器编辑 PowerShell 并且 PowerShell 工作流运行手册的详细信息，请参阅[编辑在 Azure 自动化的文本运行手册](automation-edit-textual-runbook.md)
- 要了解更多有关图形 runbook 创作，请参阅[图形创作在 Azure 自动化](automation-graphical-authoring-intro.md)
