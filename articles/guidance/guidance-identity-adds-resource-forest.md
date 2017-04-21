<properties
   pageTitle="Azure 架构引用 — IaaS︰ 在 Azure 中创建一个 Active Directory 资源目录林 |Microsoft Azure"
   description="在 Azure 创建受信任的 Active Directory 域的方式。"
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>在 Azure 创建活动目录目录服务 (ADDS) 资源目录林

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍如何创建在 Azure 分开，但信任，Active Directory 域内部林中的域。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 此参考体系结构使用资源管理器，Microsoft 建议为新的部署。

运行部署活动目录 (AD) 一个组织可能具有包含多个不同的域目录林。 例如，您可以创建单个域的不同部门或 suborganizations，或者新的域可能已收购或其他组织合并的结果添加。 您可以使用域必须是独立的可能是出于安全考虑，保持的功能区域之间提供隔离但可以共享之间建立信任关系的域的信息。

使用单独的域组织可以利用 Azure 通过将一个或多个这些域重新定位到云。 此外，一个组织可能希望保持逻辑上不同于那些保持内部，所有的云资源并将云资源的信息存储在其自己的目录还保存在云环境的域中。

可以在 Active Directory 在 Azure 多种不同的方法，文章[扩展到 Azure 的 Active Directory]中所述[extending-ad-to-azure]和[实施 Azure Active Directory][implementing-aad]。 本文侧重于一个特定的方案︰ 在云举行内部部署的任何域的不同但都可以与内部域的信任关系中创建一个域。 

这种体系结构的典型使用情况包括︰

- 维护对象和标识保存在云的安全分离。

- 内部的各个域迁移到云环境中。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示 （*单击以放大*） 此体系结构中的重要组成部分。 关于灰元素的详细信息，阅读[实施安全混合网络体系结构在 Azure] [implementing-a-secure-hybrid-network-architecture]和[实施与 Azure 中的 Internet 访问的安全的混合网络体系结构][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[![0]][0]

- **内部网络。** 内部网络包含其自己的广告目录林和域。

- **AD 服务器。** 这些都是实施目录服务 (AD DS) 作为虚拟机在云中运行的域控制器。 这些服务器承载包含一个或多个域，独立于那些位于内部林。

- **单向信任关系。** 在图中的示例显示中云到内部域单向信任域中。 这种关系使内部用户访问资源域中云，但不是其他的方法解决。 这是一种常见情况。 但是，可以创建双向信任，如果云用户还需要对内部资源的访问。

- **活动目录的子网。** AD DS 服务器承载在单独的子网中。 NSG 规则保护 AD DS 的服务器，可以提供防火墙，以阻止来自意外源的通讯。

- **Web 层子网**、**业务层子网**，和**数据层的子网**。 这些子网主机的服务器和运行在云中的应用程序的组件。 有关详细信息，请参阅[的 N 层体系结构在 Azure 上运行虚拟机][running-VMs-for-an-N-tier-architecture-on-Azure]。 云的域中包含的资源和在此子网中的虚拟机。

- **Azure 的网关**。 Azure 的网关提供的内部网络和 Azure VNet 之间的连接。 这可以是一个[VPN 连接][ azure-vpn-gateway] [Azure ExpressRoute]或[azure-expressroute]。 有关详细信息，请参见[实现 Azure 中的安全混合网络体系结构][implementing-a-secure-hybrid-network-architecture]。

## <a name="recommendations"></a>建议

本部分提供建议的基础实现的基本体系结构所需的基本组件的列表。 这些建议包括︰

- 添加设置和

- 信任关系配置。

您可能需要附加的或不同于此处所述的要求。 可用于项目在此部分中作为第一步考虑如何自定义您自己的系统的体系结构。

### <a name="adds-recommendations"></a>将建议

在云环境中实现 Active Directory 的具体建议，请参阅文档[扩展到 Azure 的 Active Directory][extending-ad-to-azure]。 文章[部署 Windows 服务器 Active Directory 在 Azure 的虚拟机的准则][ad-azure-guidelines]包含附加的详细的信息。

### <a name="trust-recommendations"></a>信任的建议

在云环境中的域的其他目录林中包含的内部域。 若要启用对云的内部用户进行身份验证，在云环境中的域必须信任内部林中的登录域。 同样，如果云为外部用户提供登录域，则可能有必要内部目录林信任云域。

您可以通过[创建林信任]建立信任目录林级别的[creating-forest-trusts]，或通过[创建外部信任]域级的[creating-external-trusts]。 目录林级别信任创建两个目录林的所有域之间的关系。 外部域级别信任仅创建指定的两个域之间的关系。 您应该只创建不同目录林中的域之间的外部域级别信任。

信任可以是单向 （单向） 或双向 （双向）︰

- 单向信任使一个域或目录林 （即所谓的*传入*域或目录林） 中的用户能够访问保存在另一个的资源 （*传出*域或目录林中）。 

- 双向信任关系允许用户在域或树林中访问保存在其他的资源。

下表总结了一些简单的方案的信任配置︰

| 方案 | 内部信任 | 云的信任 |
|----------|-------------------|-------------|
| 内部用户需要访问的资源在云中，但反过来行不通 | 单向的内向 | 单向的传出 |
| 在云环境中的用户需要访问位于资源内部，但反过来行不通 | 单向的传出 | 单向的内向 |
| 群与内部部署中的用户需要访问保存在云以及内部的资源 | 双向、 传入和传出 | 双向、 传入和传出 |

## <a name="security-considerations"></a>安全注意事项

目录林级别信任关系是可传递的。 如果建立林级别之间的内部林和云中的林信任，该信任扩展到其他创建任意一个林中的新域。 如果您使用域以提供分离为安全起见，请考虑在域级别创建信任关系。 域级别信任都是非传递信任。

广告特有的安全事项，请参阅[扩展到 Azure 的 Active Directory]中的*安全注意事项*一节[extending-ad-to-azure]。

## <a name="availability-considerations"></a>可用性考虑事项

实现为每个域的至少两个域控制器。 这使服务器之间自动复制。 创建设置 AD 服务器处理的每个域作为虚拟机的可用性。 确保组中至少两个服务器。 

另外，请考虑将每个域中的一个或多个服务器指定为[备用操作主机][standby-operations-masters]作为 FSMO 角色的服务器的连接失败的情况下。

## <a name="scalability-considerations"></a>可伸缩性注意事项

广告是属于同一个域的域控制器的自动扩展。 请求被分发到域中的所有控制器。 您可以添加另一个域控制器，并在它与域自动同步。 不要配置单独的负载平衡器将流量引导至控制器的域中。 确保所有域控制器都具有足够的内存和存储资源，来处理域数据库。 使所有的域控制器虚拟机具有相同的大小。

## <a name="management-and-monitoring-considerations"></a>管理和监视的注意事项

关于管理和监视注意事项的信息，请参阅[扩展到 Azure 的 Active Directory]中的相当部分[extending-ad-to-azure]。 

有关其他信息，请参阅[监视 Active Directory][monitoring_ad]。 您可以安装工具，例如[微软系统中心][microsoft_systems_center]在管理子网以帮助执行这些任务的监视服务器上。 

## <a name="solution-components"></a>解决方案组件

一个示例解决方案脚本[部署 ReferenceArchitecture.ps1][solution-script]，可用于实现遵循本文中介绍的建议的体系结构。 此脚本使用 Azure 资源管理器的模板。 作为一套基本构建块，其中每个执行特定的操作，例如创建 VNet 或配置 NSG 提供了模板。 该脚本的目的是协调模板部署。

将模板保存在单独的 JSON 文件中的参数与参数化。 您可以修改配置以满足您自己的要求部署这些文件中的参数。 不需要修改模板本身。 您不得更改参数文件中的对象的架构。

编辑模板时，创建对象，请按照[推荐的 Azure 资源命名约定]中所述的命名约定[naming-conventions]。

该示例解决方案创建，并在云中实现名为*treyresearch.com*的域配置环境。 此环境包含所添加的子网和服务器、 DMZ，web 层、 业务层和数据访问层组件，VPN 网关和管理层。 此示例解决方案还包括一个可选配置为使用其自己的域*contoso.com*创建模拟的内部部署环境。 该解决方案包括脚本，这些使内部用户可以访问云中的*treyresearch.com*域中的对象的域之间建立信任关系。

以下各节描述各个元素的内部和云配置。

### <a name="on-premises-components"></a>内部组件

>[AZURE.NOTE] 这些组件不是本文档中描述的体系结构的主要重点，并且提供了只是为了让您有机会测试云环境安全，而不是使用实际的生产环境。 出于此原因，本部分只总结了关键参数文件。 您可以修改设置，例如 IP 地址或大小的 Vm，但最好让许多其他参数不变。

此环境包括 AD 林中*contoso.com*域。 域包含 IP 地址 192.168.0.4 和 192.168.0.5 两个添加服务器。 这两台服务器运行 DNS 服务。 这两个虚拟机上的本地管理员帐户称为`testuser`密码`AweS0me@PW`。 此外，配置将用于连接到云环境中 VNet 的 VPN 网关设置。 您可以通过编辑以下位于[**参数/onpremise**]的 JSON 文件修改配置[on-premises-folder]文件夹︰

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**。 此文件定义了内部环境的网络地址空间。

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**。 此文件包含宿主添加服务内部部署虚拟机的配置。 默认情况下，创建了两个*标准的 DS3 v2*虚拟机。

- ** [virtualNetworkGateway.parameters.json] [on-premises-virtualnetworkgateway-parameters]**和**[connection.parameters.json][on-premises-connection-parameters]**。 这些文件保存在云中，包括共享的密钥，以用于保护遍历该网关的通信到 Azure VPN 网关的 VPN 连接的设置。

该文件夹中的其余文件包含用于创建使用此基础结构内部域 (*contoso.com*) 的配置信息。 使用它们来安装添加安装 DNS，然后创建内部林。

该解决方案还使用以下脚本中，名为[传入 trust.ps1][incoming-trust]，该内部域中的计算机上运行︰

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

此脚本将 AD DS 服务器的 IP 地址添加在云中 （请参见下一节） 到本地 DNS 服务，然后使用[netdom] [netdom]命令以从云 (*treyresearch.com*) 中的域创建传入的单向信任。

### <a name="cloud-components"></a>云的组件

这些组件构成了此体系结构的核心。 他们安装的*treyresearch.com*域的基础结构并使用内部*contoso.com*域中创建信任关系。 [**参数/azure**] [azure-folder]文件夹中包含以下参数文件来配置这些组件︰

- ** [virtualNetwork.parameters.json][vnet-parameters]**。 此文件定义的虚拟机和云环境中的其他组件的 VNet 结构。 它包括的设置，例如名称、 地址空间、 子网和所需的任何 DNS 服务器的地址。 此示例引用中所示的内部 DNS 服务器，以及默认的 Azure DNS 服务器的 IP 地址的 DNS 地址。 修改引用自己的 DNS 设置，如果您不使用示例内部环境这些地址︰
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **。 此文件配置虚拟机运行在云中的添加。 配置包括两个虚拟机。 更改管理员的用户名和密码在`virtualMachineSettings`节，并且您可以有选择地修改 VM 大小以匹配域的要求︰

    有关详细信息，请参阅[扩展到 Azure 的 Active Directory][extending-ad-to-azure]。
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- **[中添加-添加的域-controller.parameters.json][add-adds-domain-controller-parameters]**。 此文件包含用于创建跨越添加服务器的*treyresearch.com*域的设置。 它使用自定义扩展，建立域并添加服务器添加到它。 除非您创建其他添加服务器 (在这种情况下您应该将它们添加到`vms`数组)、 更改它们的名称从默认值，或者想要使用一个不同的名称，您不需要修改此文件中创建一个域。

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**。 此文件包含用于创建在云中使用以连接到内部网络的 Azure VPN 网关设置。 您应该修改`sharedKey`中的值`connectionsSettings`的内部部署 VPN 设备相匹配的部分。 有关详细信息，请参见[实现一种混合网络体系结构 Azure 和内部部署 VPN][hybrid-azure-on-prem-vpn]。

- ** [dmz private.parameters.json] [dmz-private-parameters]**和**[dmz public.parameters.json ] [ dmz-public-parameters] **。 这些文件配置入站 （公共） 和出站 （专用） 侧面构成了 DMZ，保护云中的服务器执行虚拟机。 有关这些元素和其配置的详细信息，请参见[实现 Azure 和 Internet 之间的 DMZ][implementing-a-secure-hybrid-network-architecture-with-internet-access]。

- **[负载平衡器 web.parameters.json][loadBalancer-web-parameters]**，**[负载平衡器 biz.parameters.json][loadBalancer-biz-parameters]**，和**[负载平衡器 data.parameters.json][loadBalancer-data-parameters]**。 这些参数文件包含 web、 业务和数据访问层的虚拟机规格和配置每个层的负载平衡器。 这些都是虚拟机，承载 web 应用程序和数据库，并执行组织的业务工作负载。 Web 层中的虚拟机使用中指定的设置添加到域中云**[web vm 域 join.parameters.json] [web-vm-domain-join-parameters]**文件。

    每个文件包含以下两组配置参数。 `virtualMachineSettings`部分定义 Vm 承载 ADFS 中云服务。 默认情况下，该脚本创建两个虚拟机在同一可用性组。 以下片段显示了*负载平衡器 web.parameters.json*文件的相关部分︰

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    您可以根据您的要求修改的大小和每一层中的有多少个虚拟机。

    `loadBalancerSettings`一节提供了有关这些负载平衡器的描述虚拟机。 负载平衡器将出现的通信端口 80 (HTTP) 和端口 443 (HTTPS) 传递给一个或多个虚拟机。 

    >[AZURE.NOTE] TCP 连接，而不是 HTTP，端口 80 的规则使用。 这是因为在 web 层上的安装 IIS 被配置为仅支持 Windows 身份验证。 禁用匿名身份验证。 *Ping*尝试通过 HTTP 连接的端口 80 失败有 401 （未经授权） 错误，而只使用 TCP 连接时检测到该端口是否处于活动状态︰

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**。 此文件包含跳转框/管理虚拟机的配置。 才可以从跳转框中 web、 业务和数据层的虚拟机到获得登录和管理访问权限。 默认情况下，该脚本创建一个*Standard_DS1_v2* VM，但您可以修改此文件以创建更大或更多虚拟机管理工作量很可能会很大。

配置还使用[传出 trust.ps1] [outgoing-trust]脚本如下所示使用*contoso.com*域创建一个单向传出信任︰

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

此脚本是类似于前面所述的*传入 trust.ps1*脚本。 内部部署的 IP 地址添加到本地 DNS 服务，AD DS 服务器，然后使用[netdom] [netdom]命令创建的传出信任关系。

## <a name="solution-deployment"></a>解决方案部署

本解决方案假定满足下列先决条件︰

- 具有现有 Azure 订阅，您可以在其中创建资源组。

- 您已经下载并安装最新版本的 Azure Powershell。 请参阅[此处][azure-powershell-download]的说明。

若要运行该脚本来部署解决方案︰

1. 将移动到的便捷文件夹在您的本地计算机上并创建以下子文件夹︰

    - 脚本

    - 脚本/参数

    - 脚本/参数/Onpremise

    - 脚本/参数/Azure

2. 下载[部署 ReferenceArchitecture.ps1] [solution-script]文件到脚本文件夹

3. 下载内容的[参数/onpremise] [ on-premises-folder] Onpremise/参数/脚本文件夹的文件夹︰

4. 下载内容的[参数/azure] [azure-folder]脚本/参数/Azure 文件夹的文件夹。

5. 编辑脚本文件夹中的部署 ReferenceArchitecture.ps1 文件，并更改以下行来指定应创建或用来存放由脚本创建的资源的资源组︰

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. 编辑参数文件中的脚本/参数/Onpremise 和脚本/参数/Azure 文件夹。 更新资源组文件中的引用这些资源组分配到该部署 ReferenceArchitecture.ps1 文件中的变量的名称相匹配。 下表显示了哪些参数文件引用的资源组。 *Ra adfs 负荷 rg*、 *ra adfs 安全 rg*、 *ra-adfs-添加-rg*、 *ra adfs adfs rg*和*ra adfs 代理 rg*资源组只用 PowerShell 脚本中并不会出现在参数文件中。

  	|资源组|参数文件|
  	|--------------|--------------|
  	|ra adtrust onpremise rg|parameters\onpremise\connection.parameters.json<br /> parameters\onpremise\virtualMachines-adds.parameters.json<br />parameters\onpremise\virtualNetwork-adds-dns.parameters.json<br />parameters\onpremise\virtualNetwork.parameters.json<br />parameters\onpremise\virtualNetworkGateway.parameters.json<br />parameters\azure\virtualNetworkGateway.parameters.json
  	|ra adtrust 网络 rg|parameters\onpremise\connection.parameters.json<br />parameters\azure\dmz-private.parameters.json<br />parameters\azure\dmz-public.parameters.json<br />parameters\azure\loadBalancer-biz.parameters.json<br />parameters\azure\loadBalancer-data.parameters.json<br />parameters\azure\loadBalancer-web.parameters.json<br />parameters\azure\virtualMachines-adds.parameters.json<br />parameters\azure\virtualMachines-mgmt.parameters.json<br />parameters\azure\virtualNetwork-adds-dns.parameters.json<br />parameters\azure\virtualNetwork.parameters.json<br />parameters\azure\virtualNetworkGateway.parameters.json （*两次*）

    此外，设置在内部部署的配置和[解决方案组件]中所述云组件，[solution-components]部分。

7. 打开 Azure PowerShell 窗口，将移动到脚本文件夹中，然后运行以下命令︰

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    更换`<subscription id>`Azure 的订阅 id

    为`<location>`，指定一个 Azure 的区域，如`eastus`或`westus`。

    `<mode>`参数可以具有下列值之一︰

    - `Onpremise`若要创建模拟的内部部署环境。

    - `Infrastructure`创建 VNet 基础结构并跳转在云中的框。

    - `CreateVpn`以生成 Azure 的虚拟网络网关并将其连接到内部网络。

    - `AzureADDS`以构造虚拟机用作添加服务器，请将 Active Directory 部署到这些虚拟机，并在云中创建域。

    - `WebTier`其中创建 web 层的虚拟机和负载平衡器。

    - `Prepare`其中执行上述所有任务。 **这是推荐的选项，如果您正在构建一个完整的新的部署，并且您没有现有的内部部署基础结构。**

    - `Workload`若要创建业务和数据层的虚拟机和负载平衡器。 这些虚拟机并不包含在`Prepare`选项。

    >[AZURE.NOTE] 如果您使用`Prepare`选项，则脚本将使用几个小时才能完成。

8.  如果您使用的内部配置示例︰

    1. 连接到跳转框 (*ra adtrust 管理 vm1* *ra adtrust 安全 rg*资源组中)。 作为*testuser*用密码登录*AweS0me@PW*。

    2.  在跳转中打开第一个 VM 上的 RDP 会话中*contoso.com*域 （内部部署）。 此 VM 都有 192.168.0.4 的 IP 地址。 用户名是用密码*contoso\testuser* *AweS0me@PW*。

    3. 下载[传入 trust.ps1] [incoming-trust]编写脚本和运行以创建从*treyresearch.com*域的传入信任。

9. 如果您使用的的内部部署基础结构︰

    1. 下载[传入 trust.ps1] [incoming-trust]脚本。

    2. 编辑脚本和替换的值`$TrustedDomainName`变量与您自己的域的名称。

    3. 运行该脚本。

10. 从跳转的框中，将连接到第一个虚拟*treyresearch.com*域 （在云环境中的域） 中。 此 VM 都有 10.0.4.4 的 IP 地址。 用户名是用密码*treyresearch\testuser* *AweS0me@PW*。

11. 下载[传出 trust.ps1] [outgoing-trust]编写脚本和运行以创建从*treyresearch.com*域的传入信任。 如果您使用的本地计算机，然后编辑脚本。 设置`$TrustedDomainName`变量为您的内部域的名称，并指定此域中的 AD DS 服务器的 IP 地址`$TrustedDomainDnsIpAddresses`变量。

12. 在本地计算机上执行[验证信任关系]文章中概述的步骤[verify-a-trust]以确定是否信任关系已正确配置*contoso.com*和*treyresearch.com*域之间。 您可能需要等待几分钟后完成上述步骤，然后才能验证信任关系。

剩余的可选步骤显示如何确定域信任关系是否按预期的方式工作。 下列步骤要求您具有与 Visual Studio 安装到开发计算机的访问。

1.  从 Azure PowerShell 窗口中，运行以下命令来创建 web 层，如果您有未设置其以前 (使用`Prepare`选项):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    此命令创建 web 层，并将虚拟机添加到*treyresearch.com*域。

2. 使用 Visual Studio 开发计算机上，创建一个名为*TreyResearchWebApp*的 ASP.NET Web 应用程序。 使用.NET Framework 4.5.2。

3. 选择的*MVC*模板并更改为*Windows 身份验证*的身份验证。 不要在云中创建应用程序服务。

3. 生成并运行应用程序以测试验证。 确认您的当前 Windows 用户名出现在向右页顶部的菜单栏。

4. 关闭 Internet Explorer。

5. 在*解决方案资源管理器*窗口中，右击 TreyResearchWebApp 项目，单击*发布*。

6. 在*发布网站*窗口中，单击*自定义*。 创建一个称为*TreyResearchWebApp*的自定义配置文件。

7. 在*连接*页中，为*文件系统*设置的*发布方法*，指定一个文件夹命名为*TreyResearchWebApp*，位于开发计算机上的方便位置。

8. 在*设置*页上将*配置*设置到*发行版*。

9. 在*预览*页面上，单击*发布*。

10. 连接到 web 层反过来 （通过跳转框中） 中的每个 VM，并执行以下任务。 IP 地址的 web 层的虚拟机是 10.0.1.4 和 10.0.1.5。 这两个虚拟机的用户名是用密码*treyresearch\testuser* *AweS0me@PW*:

    1. 将*TreyResearchWebApp*文件夹和其内容从开发计算机复制到*C:\inetpub*文件夹。

    2. 使用 Internet Information Services (IIS) 管理器控制台，定位到*Sites\Default 网站*的计算机上。

    3. 在*操作*窗格中，单击*基本设置*并将该 web 站点的物理路径更改为*%SystemDrive%\inetpub\TreyResearchWebApp*。

    4. 在*功能视图*窗格中，双击*身份验证*。 验证启用了*Windows 身份验证*，并且禁用了*匿名身份验证*。

11. 从本地计算机上，打开 Internet Explorer，浏览到网站，网址 http://10.0.1.254 （这是 web 层负载平衡器的地址）。

12. 在*Windows 安全*对话框中，输入内部域中的用户的凭据。 指定一个*treyresearch*域中不存在的用户名。 如果您正在使用模拟的内部部署环境首先在*contoso*域中创建一个用户，并指定此用户的凭据。

13. 主页页面出现时，验证正确的域和用户名显示在向右页顶部的菜单栏。

## <a name="next-steps"></a>下一步行动

- 了解用于[扩展您的内部添加域到 Azure]的最佳做法[adds-extend-domain]

- 了解[创建 ADF 的基础架构]的最佳做法[adfs]Azure 中。

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "安全与独立的 AD 域的混合网络体系结构"