<properties 
   pageTitle="Runbook 设置"
   description="介绍了 runbook Azure 自动化以及如何更改它们使用 Azure 管理门户和 Windows PowerShell 的配置设置。"
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

# <a name="runbook-settings"></a>Runbook 设置

每个 runbook Azure 自动化中的有多个设置，帮助它进行识别，并更改其日志记录行为。 上述每个设置被下述跟过程如何对其进行修改。

## <a name="settings"></a>设置

### <a name="name-and-description"></a>名称和说明

已创建后，您无法更改 runbook 的名称。 说明是可选的可达 512 个字符。

### <a name="tags"></a>标记

标记使您可以指定不同的单词和短语，可帮助识别 runbook。 例如，当您提交到[Runbook 库](https://msdn.microsoft.com/library/dn781422.aspx)runbook，您指定特定标签以标识 runbook 应列入哪些类别。 通过用逗号分隔，可以指定多个标记为 runbook。

### <a name="logging"></a>日志记录

默认情况下，详细和进度记录不会写入到作业历史记录。 您可以更改特定的 runbook 记录这些记录的设置。 这些记录的详细信息，请参阅[Runbook 输出和消息](https://msdn.microsoft.com/library/dn879148.aspx)。

## <a name="changing-runbook-settings"></a>更改 runbook 设置

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>更改 runbook Azure 管理门户设置

您可以从 runbook 的**配置**页更改设置 Azure 管理门户中 runbook。

1. 在 Azure 管理门户中，选择**自动化**，然后单击自动化帐户的名称。
1. 选择**运行手册**选项卡。
1. 单击 runbook 的名称。
1. 选择**配置**选项卡。

### <a name="changing-runbook-settings-with-windows-powershell"></a>更改 Windows PowerShell 的 runbook 设置

[一组 AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) cmdlet 可以用于更改 runbook 的设置。 如果您想要指定多个标记，您可以要么提供数组或一个字符串标记参数的逗号分隔值。 您可以获得与[获取 AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx)的当前标记。

下面的示例命令显示如何设置 runbook 属性。 此示例将三个标记添加到现有的标记，指定应该记录的详细记录。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>相关的文章
- [Runbook 的输出和消息](../automation-runbook-output-and-messages) 
- [创建或导入 Runbook](https://msdn.microsoft.com/library/dn643637.aspx) 