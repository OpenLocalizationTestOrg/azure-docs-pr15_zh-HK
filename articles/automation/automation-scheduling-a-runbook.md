<properties 
   pageTitle="计划在 Azure 自动化 runbook |Microsoft Azure"
   description="描述如何创建在 Azure 自动化中的计划，以便您可以自动启动 runbook 在某一特定时间或定期计划。"
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
   ms.date="08/05/2016"
   ms.author="bwren" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>计划在 Azure 自动化 runbook

若要安排在 Azure 自动化在指定时间启动 runbook，您将其链接到一个或多个计划。 时间表可以配置为运行上再次出现一次或每小时或每日计划运行手册在 Azure 的传统门户和运行手册在 Azure 的门户，您可以另外安排在其每周、 每月、 每周的特定天数或月的天或月中的某一天。  Runbook 可以被链接到多个计划，和计划可以有多个链接到它的运行手册。


## <a name="creating-a-schedule"></a>创建计划

您可以在 Azure 的门户中，在传统的门户网站，或使用 Windows PowerShell 创建运行手册的新计划。 您还可以为计划使用 Azure 的经典或 Azure 门户链接 runbook 时创建新的计划。

>[AZURE.NOTE] 当您将计划与 runbook 相关联时，自动化您的帐户中存储模块的当前版本和它们链接到该计划。  这意味着，如果创建一个计划，然后更新到版本 2.0 的模块时，您的帐户中有模块版本 1.0，该计划将继续使用 1.0。  若要使用更新的模块版本，必须创建新的计划。 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>在 Azure 的传统门户网站中创建一个新计划

1. 在 Azure 经典门户中，选择自动化，然后再选择自动化帐户的名称。
1. 选择**资源**选项卡。
1. 在窗口的底部，单击**添加设置**。
1. 单击**添加时间表**。
1. 键入一个**名称**和 （可选）**描述**新的 schedule.your 计划将运行**一次**、**每小时**、**每天**、**每周**或**每月**。
1. 指定**开始时间**和其他选项，这取决于您选择的计划类型。

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>在 Azure 的门户网站中创建新的计划

1. 在 Azure 的门户中，从自动化您的帐户，请单击**资产**平铺打开**的资产**刀片式服务器。
2. 单击**计划**图块以打开**计划**刀片式服务器。
3. 单击顶部的刀片式服务器**添加时间表**。
4. **新计划**刀片上, 键入一个**名称**和 （可选）**描述**新的日程。
5. 通过选择**一次**或**定期**选择是否计划将运行一次，或按定期计划。  如果**一次**选择指定**开始时间**，然后单击**创建**。  如果选择**定期**，指定的**开始时间**和频率的希望 runbook 重复频率-按**小时**、**天**、**周**或**每月**。  如果从下拉列表中选择**一周**或**月**，**通过重复选项**将出现在刀片式服务器时的选择，**通过重复选项**刀片式服务器将会显示和如果您选择了**一周**，您可以选择一周中的天。  如果选择了**月**，可以选择由**周日期**或特定日期的日历月份，最后，不要想要或不该月的最后一天运行，然后单击**确定**。   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>若要使用 Windows PowerShell 创建新的计划

可用于[新建 AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet 经典运行手册，Azure 自动化中创建新的计划或[新建 AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet 的 Azure 门户运行手册。 必须指定计划，则应运行的频率的开始时间。

下面的示例命令显示如何创建新运行的计划，每天下午 3:30 与 Azure 服务管理 cmdlet 于 2015 年 1 月 20 日开始。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

下面的示例命令演示如何创建计划 15 日和 30 日每月使用 Azure 资源管理器 cmdlet。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
    

## <a name="linking-a-schedule-to-a-runbook"></a>链接到 runbook 的计划

Runbook 可以被链接到多个计划，和计划可以有多个链接到它的运行手册。 如果 runbook 参数，然后可以为其提供值。 您必须提供所有必需的参数值，并可能提供的任何可选参数的值。  每次通过此计划启动 runbook 时，会使用这些值。  您可以将相同的 runbook 附加到另一个计划并指定不同的参数值。


### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>若要链接到 Azure 的传统门户网站 runbook 的计划

1. 在 Azure 经典门户中，选择**自动化**，然后再单击自动化帐户的名称。
2. 选择**运行手册**选项卡。
3. 单击 runbook 计划的名称。
4. 单击**计划**选项卡。
5. 如果 runbook 不到计划的当前链接，然后将为您提供选项为**链接到新的计划**或**链接到现有的计划**。  如果 runbook 当前链接到一个时间表，请单击**链接**位于窗口的底部来访问这些选项。
6. 如果 runbook 参数，系统将提示您输入其值。  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>若要链接到 Azure 的门户网站 runbook 的计划

1. 在 Azure 的门户中，从自动化您的帐户，单击以打开**运行手册**刀片式服务器**运行手册**平铺。
2. 单击 runbook 计划的名称。
3. 如果 runbook 不到计划的当前链接，然后将为您提供选项，以创建新的计划或链接到现有的计划。  
4. 如果 runbook 参数，您可以选择选项**修改运行设置 (默认值︰ Azure)**和显示**参数**刀片式服务器位置相应地输入信息。  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>若要链接到与 Windows PowerShell runbook 的计划

[注册 AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx)可用于链接到经典的 runbook 或运行手册在 Azure 的门户网站中的[注册 AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet 的计划。  您可以使用参数参数指定 runbook 参数的值。 指定参数值的详细信息，请参阅[启动 Runbook Azure 自动化中](automation-starting-a-runbook.md)。

下面的示例命令显示如何链接使用带参数的 Azure 服务管理 cmdlet 的计划。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

下面的示例命令显示如何链接到 Azure 资源管理器 cmdlet 使用参数 runbook 的计划。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>禁用一个时间安排

禁用计划时，链接到任何运行手册将不再运行该计划。 可以手动禁用时间表或它们在创建时设置频率的计划的到期时间。 当达到过期时间时，该计划将被禁用。

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>要禁用时间表从 Azure 的传统门户网站

您可以禁用从日程安排的计划详细信息页的 Azure 经典门户的计划。

1. 在 Azure 经典门户中，选择自动化，然后再单击自动化帐户的名称。
1. 选择资源选项卡。
1. 单击要打开其详细信息的页面的计划的名称。
2. 将**启用**更改为**否**。

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>要禁用时间表从 Azure 门户

1. 在 Azure 的门户中，从自动化您的帐户，请单击**资产**平铺打开**的资产**刀片式服务器。
2. 单击**计划**图块以打开**计划**刀片式服务器。
2. 单击以打开详细信息刀片式服务器调度的名称。
3. 将**启用**更改为**否**。

### <a name="to-disable-a-schedule-with-windows-powershell"></a>若要禁用 Windows PowerShell 的日程

[一组 AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet 可用于更改现有的经典 runbook 或[设置 AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet 的 Azure 门户运行手册的计划属性。 若要禁用时间表，指定**false** **启用**参数。

下面的示例命令演示如何禁用调度使用 Azure 服务管理 cmdlet。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

下面的示例命令演示如何禁用使用 Azure 资源管理器 cmdlet runbook 的计划。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>下一步行动

- 若要了解有关使用时间表的详细信息，请参阅[在 Azure 自动化计划资产](http://msdn.microsoft.com/library/azure/dn940016.aspx)
- 若要开始使用 Azure 自动化中的运行手册，请参阅[启动在 Azure 自动化 Runbook](automation-starting-a-runbook.md) 