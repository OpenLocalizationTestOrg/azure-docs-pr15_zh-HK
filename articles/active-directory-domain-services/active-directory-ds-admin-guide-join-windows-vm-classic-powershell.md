<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 管理指南 |Microsoft Azure"
    description="将 Windows 虚拟机加入到管理域使用 Azure PowerShell 和经典的部署模型。"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>将 Windows Server 虚拟机加入到管理域使用 PowerShell

> [AZURE.SELECTOR]
- [Azure 经典门户-Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell 的窗口](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure 具有用于创建和使用资源的两种不同的部署模型︰[资源管理器和经典](../resource-manager-deployment-model.md)。 本文介绍如何使用经典的部署模型。 Azure AD 域服务目前不支持的资源管理器模型。

下列步骤显示了如何自定义一套 Azure PowerShell 命令创建和预配置的基于 Windows Azure 虚拟机使用构建基块的方法。 这些步骤将帮助您构建一个基于 Windows Azure 的虚拟机并将它加入到 Azure AD 域服务托管域。

这些步骤创建 Azure PowerShell 命令集的一种填写空白的方法。 如果您是新手 PowerShell 或想要知道要成功配置为指定的值，此方法非常有用。 高级的 PowerShell 用户可以执行命令并替换自己的变量的值 （以"$"开头的行）。

如果您没有这样做，使用中[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)说明在您的本地计算机上安装 Azure PowerShell。 然后，打开 Windows PowerShell 命令提示符。

## <a name="step-1-add-your-account"></a>步骤 1︰ 添加您的帐户

1. PowerShell 提示时，键入**添加 AzureAccount** ，并单击**输入**。
2. 与 Azure 订阅相关的电子邮件地址中键入，然后单击**继续**。
3. 键入您的帐户的密码。
4. 单击**登录**。

## <a name="step-2-set-your-subscription-and-storage-account"></a>步骤 2︰ 设置您的订购和存储帐户

通过 Windows PowerShell 命令提示符处运行以下命令来设置 Azure 订购、 存储帐户。 引号，包括的所有内容替换 < 和 > 字符，使用正确的名称。

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

从 SubscriptionName 属性的**Get AzureSubscription**命令的输出可以正确订阅名称。 运行**选择 AzureSubscription**命令之后，可以从标签属性的**Get AzureStorageAccount**命令的输出中获取正确的存储帐户名。


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>第 3 步︰ 分步演练的调配虚拟机，并将它加入到托管域
下面是相应的 Azure PowerShell 命令设置为创建该虚拟机，以便于阅读每个块之间的空行。

指定 Windows 虚拟机设置的信息。

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

D、 DS 或 G 系列的虚拟机的 InstanceSize 值，请参阅[虚拟机和 Azure 的云服务规模](https://msdn.microsoft.com/library/azure/dn197896.aspx)。

提供有关管理域的信息。

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

指定在云服务的名称。

    $svcname="Contoso100-test"

指定应加入虚拟机的虚拟网络名称。 确保在此虚拟网络中可用 AAD DS 托管的域。

    $vnetname="MyPreviewVnet"

选择要用于调配 VM 虚拟机映像。

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

配置虚拟机的一组虚拟机名称、 实例的大小和要使用的图像。

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

获得用于 VM 的本地管理员凭据。 选择一个强的本地管理员密码。

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

获取属于 AAD DC 管理员组，以将虚拟机加入到托管域的用户帐户的凭据。 未指定域名-例如，在我们的示例中，我们指定的用户名为 bob。

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

配置虚拟机-指定域联接要求和必需的凭据。

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

为虚拟机设置一个子网。

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

可选︰ 指向域的 IP 地址。 如果您设置 Azure AD 域服务托管域是虚拟网络的 DNS 服务器的 IP 地址，此步骤不是必需的。

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

现在，调配加入域的 Windows 虚拟机。

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>脚本来配置 Windows 虚拟机时，会自动将它加入到 AAD 域服务管理域
此 PowerShell 命令集创建的行的业务服务器的虚拟机︰

- 使用 Windows Server 2012 R2 数据中心图像。
- 是特小的虚拟机。
- 具有该名称 contoso 测试。
- 是自动加入域到 contoso100 托管域。
- 将添加到托管域所在的虚拟网络。

以下是完整的示例脚本创建 Windows 虚拟机并自动将它加入到 Azure AD 域服务托管域。

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>相关的内容
- [Azure AD 域服务-快速入门指南](./active-directory-ds-getting-started.md)

- [管理 Azure AD 域服务管理的域](./active-directory-ds-admin-guide-administer-domain.md)
