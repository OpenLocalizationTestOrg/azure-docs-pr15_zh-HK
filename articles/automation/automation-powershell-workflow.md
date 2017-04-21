<properties 
   pageTitle="学习 PowerShell 工作流"
   description="这篇文章旨在作为快速课的作者熟悉 PowerShell 了解 PowerShell 并且 PowerShell 工作流之间的具体差异。"
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
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>学习 Windows PowerShell 工作流

在 Azure 自动化运行手册作为 Windows PowerShell 工作流实现。  Windows PowerShell 流类似于 Windows PowerShell 脚本，但具有某些显著的差异，可能会造成混淆，给新用户。  本文适用于用户已经熟悉 PowerShell 并简要介绍了您要求如果要 PowerShell 脚本转换为在 runbook 中使用 PowerShell 工作流的概念。  

工作流是一系列程控、 连接步骤执行长时间运行的任务或需要跨多个设备或受控的节点的协调的多个步骤。 通过常规脚本流的好处包括︰ 能够同时执行多个设备与操作，并自动从故障中恢复的能力。 Windows PowerShell 工作流是一个 Windows PowerShell 脚本利用窗口基础工作流。 工作流是使用 Windows PowerShell 语法编写的并由 Windows PowerShell 启动，而窗口基础工作流来处理它。

这篇文章中的主题的完整详细信息，请参阅[开始使用 Windows PowerShell 工作流](http://technet.microsoft.com/library/jj134242.aspx)。

## <a name="types-of-runbook"></a>Runbook 的类型

有三种类型的 runbook Azure 自动化、 *PowerShell 流*， *PowerShell*和*图形*中。  当您创建 runbook，并无法将 runbook 转换为另一种类型中，它被创建后，您可以定义 runbook 类型。

PowerShell 工作流运行手册，运行手册 PowerShell 的用户更喜欢使用直接 PowerShell 代码或者使用 Azure 自动化中的文本编辑器或脱机编辑器如 PowerShell ISE。 如果您正在创建 PowerShell 流 runbook，您应该了解这篇文章中的信息。 

图形化运行手册允许您创建 runbook 相同的活动和 cmdlet 使用，但使用一个图形界面，隐藏基础 PowerShell 工作流的复杂性。  如检查点和并行执行本文中的概念仍然适用于图形化运行手册，但您不必担心如何详细语法。 

## <a name="basic-structure-of-a-workflow"></a>工作流的基本结构

将 PowerShell 脚本转换为 PowerShell 工作流的第一步封闭它与**工作流**关键字。  工作流开始**工作流**关键字后跟括在大括号中的脚本的正文。 工作流的名称遵循**工作流**关键字，如下面的语法所示。 

    Workflow Test-Workflow
    {
       <Commands>
    }

工作流的名称必须与自动化 runbook 的名称相匹配。 如果正在导入 runbook，文件名必须匹配的工作流名称，并且必须以.ps1 结束。

若要将参数添加到该工作流，使用**参数**关键字，就像对的脚本。 

## <a name="code-changes"></a>代码更改

PowerShell 工作流代码看起来与 PowerShell 除少数重大更改的脚本代码几乎完全相同。  以下各节描述了您将需要对它 PowerShell 脚本运行工作流中进行的更改。

### <a name="activities"></a>活动

活动是工作流中的特定任务。 就像一个脚本由一个或多个命令，工作流由按顺序执行的一个或多个活动组成。 Windows PowerShell 工作流自动转换 Windows PowerShell cmdlet 的许多活动在工作流运行时。 当您 runbook 中指定这些 cmdlet 之一时，实际上由窗口基础工作流运行相应的活动。 对于那些没有相应活动的 cmdlet，Windows PowerShell 工作流自动运行该 cmdlet [InlineScript](#inlinescript)活动中。 没有一套 cmdlet 的排除，不能使用工作流中，除非显式 InlineScript 块中包含了它们。 有关这些概念的详细信息，请参见[使用脚本工作流中的活动](http://technet.microsoft.com/library/jj574194.aspx)。

工作流活动共享一组公共参数来配置它们的操作。 关于工作流常见参数的详细信息，请参阅[about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx)。

### <a name="positional-parameters"></a>定位参数

不能使用与活动和工作流中的 cmdlet 的位置参数。  所有此种方法是，您必须使用的参数名称。

例如，请考虑下面的代码用于获取所有正在运行的服务。

     Get-Service | Where-Object {$_.Status -eq "Running"}

如果您尝试在工作流运行相同的代码，您将获得一条消息，如"参数集不能被解决使用指定的命名参数。  若要解决此问题，只需提供如下的参数名称。

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>反序列化的对象

在工作流中的对象被反序列化。  这意味着它们的属性仍然可用，但不是它们的方法。  例如，考虑下面的 PowerShell 代码停止使用 Stop 方法服务对象的服务。

    $Service = Get-Service -Name MyService
    $Service.Stop()

如果您尝试运行此工作流中，您将收到指出"方法调用不支持 Windows PowerShell 流中"错误。  

在这种情况下 $Service [InlineScript](#InlineScript)块环绕下列两行代码将是块内的服务对象是一个选项。 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

另一种方法是使用另一个 cmdlet 执行相同的功能的方法，如果有的话。  在我们的示例中，停止服务 cmdlet 提供相同的功能作为停止方法，并且您可以使用以下工作流。

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

当您需要运行一个或多个命令作为传统 PowerShell 脚本，而不是 PowerShell 流， **InlineScript**活动非常有用。  处理工作流中的命令发送到窗口基础工作流，而由 Windows PowerShell 处理 InlineScript 块中的命令。 

InlineScript 使用的语法如下所示。

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

通过将输出分配给一个变量，可以从 InlineScript 返回输出。 下面的示例停止服务，然后输出的服务名称。

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


您可以将值传递到 InlineScript 块，而是必须使用**$Using**范围修饰符。  下面的示例是与前面的示例相同，只不过由变量提供的服务名称。 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


InlineScript 活动可能会在某些工作流程的关键，他们不支持工作流构造，并只应在必要的时候出于以下原因︰

- 您不能使用 InlineScript 块内部的[检查点](#Checkpoints)。 如果在块中出现故障，它必须继续从块的开始。
- 不能使用在 InlineScriptBlock 的[并行执行](#parallel-execution)。
- InlineScript 会影响工作流的可扩展性，因为它包含 Windows PowerShell 会话的整个长度的 InlineScript 块。

在使用 InlineScript 上的更多细节，请参阅[在工作流运行 Windows PowerShell 命令](http://technet.microsoft.com/library/jj574197.aspx)和[about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx)。


## <a name="parallel-processing"></a>并行处理

Windows PowerShell 工作流的一个优点是能够与典型脚本一样按顺序而不是并行执行一组命令。 

可以使用**并行**关键字将同时运行的多个命令创建的脚本块。 这将使用如下所示的语法。 在这种情况下，Activity1 和 Activity2 将在同一时间开始。 仅在 Activity1 和 Activity2 均已完成后，Activity3 将启动。

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


例如，请考虑以下 PowerShell 命令，将多个文件复制到网络的目标。  因此复制启动下一步之前，必须完成文件按顺序运行这些命令。     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

以便他们都开始复制一次，以下工作流程并行运行这些命令。  只是所有后完全复制将完成消息显示。

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


您可以使用**ForEach 的并行**构造进程并发集合中的每一项的命令。 脚本块中的命令按顺序运行的同时并行处理集合中的项。 这将使用如下所示的语法。 在这种情况下，Activity1 将在集合中的所有项目同时启动。 对于每个项目中，Activity2 将启动 Activity1 完成后。 仅在 Activity1 和 Activity2 具有所有项目都完成后，Activity3 将启动。

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

下面的示例是类似于前面的示例中并行复制文件。  在这种情况下，会为每个文件显示一条消息，复制后多。  只是所有后完全复制将最终完成消息显示。

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  我们建议不要并行运行子运行手册，因为这已显示为不可靠的结果。  有时，子 runbook 的输出将不会显示，并且一个子 runbook 中的设置可能会影响其他并行子运行手册 


## <a name="checkpoints"></a>检查点

*检查点*是工作流变量的当前值，并对该点生成任何输出包含的当前状态的快照。 如果工作流错误地终止或暂停，然后下一次运行它时就会启动从其最后一个检查点，而不是 worfklow 的开始。  在**检查点工作流**活动与工作流中，您可以设置检查点。

下面的代码示例中，在发生异常后 Activity2 导致工作流结束。 再次运行该工作流时，它开始通过运行 Activity2，因为这是紧跟在最后一个检查点设置。

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

活动，可能容易出现异常，不应重复如果继续工作流后，您应该在工作流中设置检查点。 例如，您的工作流可能会创建一个虚拟机。 之前和之后的命令来创建虚拟机，您可以设置检查点。 如果创建失败，然后命令时都会重复如果再次启动工作流。 如果 worfklow 失败后创建成功，那么继续该工作流时，虚拟机将不重新创建。

下面的示例会将多个文件复制到网络位置之后的每个文件, 设置检查点。  丢失的网络位置时，该工作流将在错误中。  当再次启动时，将意味着被复制的文件将被跳过的最后一个检查点继续。

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

因为调用[挂起工作流](https://technet.microsoft.com/library/jj733586.aspx)活动后或最后一个检查点之后，则不保留用户名凭据，您需要设置为 null，然后他们再次检索从资产存储库调用**挂起工作流**或检查点之后的凭据。  否则，您可能会收到以下错误消息︰ 无法恢复*工作流作业，要么由于无法完全，保存或保存持久性数据持久性数据已损坏。您必须重新启动该工作流。*

相同的代码演示如何处理此 PowerShell 工作流运行手册中。

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


如果身份验证使用运行方式帐户配置服务主体，这是不需要。  

有关检查点的详细信息，请参阅[添加到脚本流的检查点](http://technet.microsoft.com/library/jj574114.aspx)。


## <a name="next-steps"></a>下一步行动

- 若要开始使用 PowerShell 工作流运行手册，请参阅[我第一个 PowerShell 流 runbook](automation-first-runbook-textual.md) 
