<properties
  pageTitle="将一个云服务连接到自定义的域控制器 |Microsoft Azure"
  description="了解如何连接到使用 PowerShell 和 AD 域扩展 AD 域自定义的 web/工作人员角色"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>连接到 AD 域控制器在 Azure 中承载的自定义 Azure 云服务角色

我们首先将在 Azure 中设置虚拟网络 (VNet)。 然后，我们将添加活动目录域控制器 （承载 Azure 虚拟机上） 到 VNet。 接下来，我们会将现有的云服务角色添加到先前创建的 VNet，并随后将它们连接到域控制器。

我们开始之前，几件事要记住的︰

1.  本教程使用 PowerShell，因此请确保您已安装的 Azure PowerShell 和准备就绪。 若要获取有关设置 Azure PowerShell 的帮助，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

2.  AD 域控制器和 Web/辅助角色实例需要在 VNet 中。

按照本逐步式指南，如果您遇到任何问题，请留言我们下面。 有人将尽快回复您 （是的我们会阅读注释）。

3. 云由引用网络<mark>必须是</mark>一个**经典的虚拟网络**服务。

## <a name="create-a-virtual-network"></a>创建虚拟网络

您可以在 Azure 使用 Azure 的传统门户网站或 PowerShell 创建虚拟网络。 对于本教程，我们将使用 PowerShell。 若要创建虚拟网络使用 Azure 的传统门户网站，请参阅[创建虚拟网络](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)。

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>创建虚拟机

完成设置虚拟网络后，您将需要创建 AD 域控制器。 对于本教程，我们将设置一个 AD 域控制器 Azure 虚拟机上。

若要执行此操作，请创建虚拟机通过 PowerShell 使用下面的命令︰

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>升级到域控制器虚拟机
若要将虚拟机配置为 AD 域控制器，您需要登录到虚拟机并将其配置。

登录到虚拟机，可以熬 PowerShell 的 RDP 文件，使用下面的命令。

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

一旦您登录到虚拟机，按照有关[如何设置客户 AD 域控制器](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx)的分步指南为 AD 域控制器设置您的虚拟机。

## <a name="add-your-cloud-service-to-the-virtual-network"></a>将云服务添加到虚拟网络

接下来，您需要添加到刚创建的 VNet 的云服务部署。 若要做到这一点，修改您的云服务 cscfg，加上使用 Visual Studio 或您选择的编辑器您 cscfg 中的相关章节。

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

下一步生成云服务项目，并将其部署到 Azure。 若要获取帮助部署到 Azure 的云服务包，请参阅[如何创建和部署云服务](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>与此域连接 web/工作人员角色

在 Azure 上部署云服务项目，一旦连接使用 AD 域扩展的自定义 AD 域角色实例。 将 AD 域扩展添加到现有的云服务部署，并将加入自定义的域，请在 PowerShell 中执行以下命令︰

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

就是这样。

云服务应现在加入到您的自定义的域控制器。 如果您想要详细了解有关如何配置 AD 域扩展可用的不同选项，使用 PowerShell 帮助，如下所示。

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
