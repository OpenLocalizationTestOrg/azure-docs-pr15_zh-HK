<properties
    pageTitle="Microsoft Azure 堆栈诊断 |Microsoft Azure"
    description="Azure 堆栈故障排除。"
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
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure 堆栈进行故障排除

本文档提供了 Azure 堆栈的常见故障排除信息。

对于最佳的体验，请确保您部署的环境符合所有的[要求](azure-stack-deploy.md)和[准备](azure-stack-run-powershell-script.md)安装之前。 

本部分中描述的问题的故障排除的建议从多个源和可能或可能无法解决您的特定问题。 如果遇到了未记录的问题，请务必检查[Azure 堆栈 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)进一步支持和其他信息。

按原样提供的代码示例并不能保证预期的结果。 本部分是频繁的编辑和更新实现对产品的改进。

  

## <a name="known-issues"></a>已知的问题

 - 您可能会看到以下非终止错误期间部署，这会影响成功部署︰
     - "不是 C:\WinRM\Start-Logging.ps1 一词是"
     - "调用 EceAction︰ 不能为 null 数组索引" 
     - "InvokeEceAction︰ 不能将参数绑定到消息参数，因为它是一个空字符串。"
 - 您可能会看到"应用程序具有标识符 URI 未找到"错误的步骤 60.61.93 在失败的部署 这种现象是由于在 Azure Active Directory 中注册应用程序的方式。  如果您收到此错误，继续[重新运行安装脚本](azure-stack-rerun-deploy.md)步骤 60.61.93 中之前部署完毕。
 - 您将看到，在市场上的**可用性设置**资源**机 ARM**类别下显示 — 此外观是一个浅显的问题。
 - 当在门户中，在**基本操作**步骤中，创建新的虚拟机的存储选项默认 SSD。  必须更改此设置，到硬盘或有关 VM 部署的**大小**一步，您将看不到可供选择，并继续部署的 VM 大小。 
 - 您将看到不再默认情况下，MA CON01 虚拟机上安装 AzureRM PowerShell 模块 （TP1 在这被称为 ClientVM）。 这种现象是设计使然，因为没有[安装这些模块并将连接](azure-stack-connect-powershell.md)到另一种方法。  
 - 您将看到**Microsoft.Insights**资源提供程序不会自动注册为租户订阅。 如果您想要查看 vm 部署为一个租户监视数据，下面的命令从 PowerShell （后运行您[安装并连接](azure-stack-connect-powershell.md)作为承租人）︰ 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - 您将看到为资源组导出门户中的功能，但是任何文本，则不属性显示并可供出口。      
 - 您可以启动部署的存储资源大于可用的配额。  这一部署将失败，并且帐户资源将被挂起。  有两个修正选项可用︰
     - 服务管理员可以增加配额，但更改不会立即生效，通常要花一个小时的时间来传播。
     - 服务管理员可以创建具有其他配额，租户可以向订阅中添加加载项计划。
 - 当使用门户网站上具有 Azure 的中国中标识的 Azure 堆栈环境创建虚拟机，您不会看到 VM 大小可以选择在虚拟机部署的**大小**步骤并将不能继续部署。
 - VM 实际上已成功部署时，您可能看到在门户中，部署失败。
 - 删除计划、 优惠或订阅时，不能删除虚拟机。
 - 您将看到在市场上的 VM 扩展。
 - 无法部署虚拟机从已保存的 VM 映像。
 - 承租人可以看到它不包括在其订阅的服务。  当承租人尝试部署这些资源时，他们会收到一个错误。  示例︰ 租户订阅仅包括存储资源。  租户将看到选项可创建虚拟机等其他资源。  在这种情况下，当承租人尝试部署虚拟机，他们将收到一条消息，指示无法创建虚拟机。 
 - 在安装时 TP2，不要激活 VHD 提供位置运行 Azure 堆栈设置脚本，或者您可能会收到错误消息说明 Windows 操作系统将很快过期的主机。


## <a name="deployment"></a>部署

### <a name="deployment-failure"></a>部署失败
如果您在安装过程中出现故障，Azure 堆栈安装程序允许继续安装失败后[重新运行的部署步骤](azure-stack-rerun-deploy.md)。

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>在部署结束时，PowerShell 会话仍处于打开状态，并且不会显示任何输出

选择后，这种现象可能是刚刚 PowerShell 命令窗口中的默认行为的结果。 POC 部署实际上已成功，但脚本暂停时选择窗口。 您可以验证这种情况通过查找单词命令窗口的标题栏中的"选择"。  按 ESC 键来取消选择它，并应在其后显示完成消息。

## <a name="templates"></a>模板

### <a name="azure-template-wont-deploy-to-azure-stack"></a>碧空模板将不会部署到 Azure 堆栈

请确保︰

- 模板必须使用 Microsoft Azure 服务已经可用，或在 Azure 堆栈中的预览。
- 用于特定资源的 Api 支持的本地堆栈 Azure 实例，和您的目标 （"本地"在 Azure 堆栈技术预览 (TP) 2、"东美国"或"南印度"Azure 中的 vs） 的有效位置。
- 查看[这篇文章](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md)的测试 AzureRmResourceGroupDeployment cmdlet，这样掉在 Azure 资源管理器语法中的差异较小。

在[GitHub 资料库](http://aka.ms/AzureStackGitHub/)中已经提供的 Azure 堆栈模板也可用于帮助您入门。

## <a name="virtual-machines"></a>虚拟机

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>启动后我 Microsoft Azure 堆栈 POC 主机，所有我承租人 Vm Hyper-V 管理器中，从和自动回来后等一小会吗？

当系统重新启动后，存储子系统和 RPs 需要确定一致性。 所需的时间取决于硬件和规范使用，但它可能为租户要回来，并且能识别的虚拟机的主机重新启动之后的某个时间。

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>我已删除了一些虚拟机，但仍然看到的 VHD 文件在磁盘上。 预计这种现象？

是的这是预期的行为。 因为，它旨在通过这种方式︰

- 当您删除虚拟机时，则不会删除 Vhd。 磁盘的单独资源的资源组中。
- 当存储帐户被删除时，删除可见立即通过 Azure 资源管理器 （门户，PowerShell） 但运行垃圾回收之前，它可能包含的磁盘仍保留在存储中。

如果您看到"孤立"Vhd，务必知道它们是否属于已删除存储帐户的文件夹。 如果存储帐户未被删除，则正常仍然保留。

您可以阅读更多有关在中[管理存储帐户](azure-stack-manage-storage-accounts.md)配置固定阈值。

## <a name="installation-steps"></a>安装步骤
Azure 堆栈安装步骤的以下信息可能是非常有用的故障排除︰  

| 索引 | 名称 | 说明|
| ----- | ----- | -----|
|0.11 | (DEP)验证物理机 | 验证硬件和操作系统的物理节点上的配置。 |
| 0.12 | (DEP)配置 POC 的物理计算机网络 | 正在配置虚拟网络交换机和接口。 |
| 0.14 | (DEP)部署域 | 部署虚拟机上的活动目录。 |
| 0.15 | (DEP)配置域服务器 | 域服务器配置安全组等。 |
| 0.16 | (DEP)物理计算机配置 | 配置网络，加入到域，以及安装的本地管理员。 |
| 0.18 | （存储）将存储群集配置 | 请创建存储群集创建存储池和文件服务器。 |
| 0.19 | (CPI)安装结构基础结构 | 设置结构部署的先决条件。 |
| 0.21 | （净额）BGP 和 NAT 安装 | BGP 和 NAT 的只针对一个节点需要安装。 |
| 0.22 | （净额）配置 NAT 和时间服务器 | 同步的时间服务器，并配置 NAT 条目。 |
| 40.41 | (CPI)创建来宾虚拟机 | 创建虚拟机的管理。 |
| 40.42 | (FBI)设置 PowerShell JEA | 为所有角色设置 PowerShell JEA。 |
| 40.43 | (FBI)设置 Azure 堆栈证书颁发机构 | 安装堆栈 Azure 证书颁发机构。 |
| 40.44 | (FBI)配置 Azure 堆栈证书颁发机构 | 配置 Azure 堆栈证书颁发机构。 |
| 40.45 | （净额）设置在 Vm 上的 NC | 在来宾虚拟机上安装 NC |
| 40.46 | （净额）在虚拟机上配置 NC | 在来宾虚拟机上配置 NC |
| 40.47 | （净额）配置来宾虚拟机 | 使用 NC Acl 配置虚拟机的管理。 |
| 60.61.81 | (FBI)部署 Azure 的堆栈结构震铃服务-FabricRing 系统必备组件 | 创建 FabricRing 的 Vip |
| 60.61.82 | (FBI)部署 Azure 的堆栈结构震铃服务-部署结构环群集 | 安装并配置 Azure 堆栈结构环群集。 |
| 60.61.83 | (FBI)资源提供程序部署管理扩展 | 资源提供程序安装管理扩展 |
| 60.61.84 | (ACS)在节点级别设置 Azure 一致的存储。 | 安装并配置在节点级别一致 Azure 存储。 |
| 60.61.85 | (ACS)在群集级别设置 Azure 一致的存储。 | 安装并配置群集级别在 Azure 一致的存储。 |
| 60.61.86 | (FBI)部署系统必备组件的 Azure 的堆栈结构环控制器服务- | InfraServiceController 的先决条件 |
| 60.61.87 | (FBI)部署系统必备组件的 Azure 的堆栈结构环控制器服务- | CPI 的先决条件 |
| 60.61.88 | (FBI)部署系统必备组件的 Azure 的堆栈结构环控制器服务- | ASAppGateway 的先决条件 |
| 60.61.89 | (FBI)部署系统必备组件的 Azure 的堆栈结构环控制器服务- | 存储控制器的先决条件 |
| 60.61.90 | (FBI)部署系统必备组件的 Azure 的堆栈结构环控制器服务- | HealthMonitoring 的先决条件 |
| 60.61.91 | (FBI)部署系统必备组件的 Azure 的堆栈结构环控制器服务- | ECE 的先决条件 |
| 60.61.92 | (FBI)部署系统必备组件的 Azure 的堆栈结构环控制器服务- | PMM 的先决条件 |
| 60.61.93 | (Katal)创建 AzureStack 服务主体 | 创建 AAD Azure 的图形应用程序和服务主体。 |
| 60.61.94 | （净额）虚拟机设置网关 | 在来宾虚拟机上安装网关。 |
| 60.61.95 | （净额）配置网关的虚拟机 | 在来宾虚拟机上配置网关。 |
| 60.61.96 | （净额）部署 idn 主机上的则 | 部署 idn 基础结构主机上的则 |
| 60.61.97 | （净额）配置 idn 则 | 配置 idn，则角色 |
| 60.61.98 | (FBI)安装 WSUS Vm | 在来宾虚拟机上安装 WSUS 服务器。 |
| 60.61.99 | (FBI)配置 WSUS 的虚拟机 | 在来宾虚拟机上配置 WSUS 服务器。 |
| 60.61.100 | (FBI)安装 SQL Azure 的虚拟机 | 在来宾虚拟机上安装 SQL Azure 服务器 |
| 60.61.101 | (Katal)安装了 Vm 的先决条件。 | 设置来宾虚拟机上的 Microsoft Azure 栈的先决条件。 |
| 60.61.102 | (Katal)安装程序的虚拟机 | 在来宾虚拟机上安装 Microsoft Azure 堆栈。 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.121 | (FBI)部署资源提供程序和控制器 | 安装资源提供程序和控制器 |
| 60.120.122 | (FBI)控制器配置 | 配置控制器 |
| 60.120.123 | (Katal)WAS 配置虚拟机 | 在来宾虚拟机上配置 Microsoft Azure 堆栈。 |
| 60.120.124 | (Katal)Azure 堆栈 AAD 配置。 | 对于 Azure AD 配置 Azure 的堆栈。 |
| 60.120.125 | (Katal)安装了 ADF | 安装 Active Directory 联合身份验证服务 (ADF) |
| 60.120.126 | (Katal)安装 ADFS/图 | 安装 Azure 堆栈图 |
| 60.120.127 | (Katal)配置 ADF | 配置 Active Directory 联合身份验证服务 (ADF) |
| 60.140.141 | (FBI)配置 SRP | 配置存储资源提供程序 |
| 60.140.142 | (ACS)配置一致 Azure 存储。 | 配置一致 Azure 存储。 |
| 60.140.143 | (FBI)创建存储帐户 | 创建使用不同的提供程序的所有存储帐户。 |
| 60.140.144 | (FBI)SRP 的寄存器使用 | 注册存储提供程序使用。 |
| 60.140.145 | (CPI)将创建的虚拟机、 主机和群集迁移到 CPI | 将对象的创建虚拟机、 主机和群集迁移到 CPI |
| 60.140.146 | (FBI)配置 Windows Defender | 配置 Windows Defender |
| 60.160.161 | （周一）配置监视代理 | 配置监视代理 |
| 60.160.162 | (FBI)NRP 系统必备组件 | NRP 系统必备组件安装 |
| 60.160.163 | (FBI)NRP 部署 | 安装 NRP  |
| 60.160.164 | (FBI)NRP 配置 | 配置 NRP |
| 60.160.165 | (FBI)CRP 系统必备组件 | CRP 系统必备组件安装 |
| 60.160.166 | (FBI)CRP 部署 | 安装 CRP |
| 60.160.167 | (FBI)CRP 配置 | 配置 CRP |
| 60.160.168 | (FBI)FRP 系统必备组件 | FRP 系统必备组件安装 |
| 60.160.169 | (FBI)FRP 部署 | 安装 FRP  |
| 60.160.170 | (FBI)FRP 配置 | 配置 FRP |
| 60.160.174 | (FBI)URP 系统必备 | URP 系统必备组件安装 |
| 60.160.175 | (FBI)URP 部署 | 安装 URP  |
| 60.160.176 | (FBI)URP 配置 | 配置 URP |
| 60.160.171 | (FBI)HRP 系统必备组件 | 将 HRP 系统必备组件安装 |
| 60.160.172 | (FBI)HRP 部署 | 将 HRP 的安装  |
| 60.160.173 | (FBI)HRP 配置 | 将 HRP 的配置 |
| 60.160.177 | (KV)KeyVault 系统必备组件 | 安装 KeyVault 系统必备组件 |
| 60.160.178 | (KV)KeyVault 部署 | 安装 KeyVault  |
| 60.160.179 | (KV)KeyVault 配置 | 配置 KeyVault | 
| 60.190.191 | (FBI)配置库 | 配置库 |
| 60.190.192 | (FBI)配置结构震铃服务 | 配置结构震铃服务 |
| 60.221 | (FBI)设置控制台的虚拟机 | 在来宾虚拟机上安装控制台服务器。 |
| 60.222 | (FBI)设置控制台的虚拟机 | 移动到虚拟机控制台 DVM 内容。 |
| 251 | 准备用于将来的主机重新启动 | 设置重新启动策略 |


## <a name="next-steps"></a>下一步行动

[常见问题及的解答](azure-stack-FAQ.md)
