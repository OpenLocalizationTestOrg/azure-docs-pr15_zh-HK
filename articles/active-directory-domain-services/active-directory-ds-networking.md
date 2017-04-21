<properties
    pageTitle="Azure AD 域服务︰ 网络指南 |Microsoft Azure"
    description="Azure Active Directory 域服务的网络注意事项"
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
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Azure AD 域服务的网络注意事项

## <a name="how-to-select-an-azure-virtual-network"></a>如何选择 Azure 的虚拟网络
下列准则可帮助您选择要使用 Azure AD 域服务的虚拟网络。

### <a name="type-of-azure-virtual-network"></a>Azure 的虚拟网络类型

- 您可以启用经典的 Azure 虚拟网络中的 Azure AD 域服务。

- Azure AD 域服务**不能使用 Azure 资源管理器创建虚拟网络中启用**。

- 可以基于资源管理器的虚拟网络连接到经典 Azure AD 域服务已启用的虚拟网络。 此后，您可以在虚拟网络中基于资源管理器使用 Azure AD 域服务。 有关详细信息，请参阅[网络连接](active-directory-ds-networking.md#network-connectivity)部分中。

- **区域的虚拟网络**︰ 如果您计划使用现有的虚拟网络，确保它是区域性的虚拟网络。

    - 使用旧式的好友小组机制的虚拟网络不能使用 Azure AD 域服务。

    - 若要使用 Azure AD 域服务时，[将迁移到区域的虚拟网络的传统虚拟网络](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)。


### <a name="azure-region-for-the-virtual-network"></a>Azure 区域虚拟网络

- 选择要启用的服务 Azure AD 域服务托管的域部署在同一 Azure 作为虚拟的网络区域。

- 虚拟网络中选择 Azure 所在地区 Azure AD 域服务支持。

- 请参阅[Azure 服务区域的](https://azure.microsoft.com/regions/#services/)页后，可以知道 Azure AD 域服务位于 Azure 区域。


### <a name="requirements-for-the-virtual-network"></a>虚拟的网络要求

- **接近于 Azure 的工作负荷**︰ 选择当前承载/将主机需要访问 Azure AD 域服务的虚拟机的虚拟网络。

- **自定义/让-您的自己的 DNS 服务器**︰ 确保有没有自定义的虚拟网络配置的 DNS 服务器。

- **现有域同名的域**︰ 确保您没有现有域与域同名的虚拟网络上可用。 例如，假定您有域在所选的虚拟网络称为 'contoso.com' 已经可用。 稍后，您尝试启用 Azure AD 域服务管理的域使用相同的域名 （即 'contoso.com'），虚拟的网络上。 当尝试启用 Azure AD 域服务时遇到故障。 此故障是由于该虚拟网络上的域名称的名称冲突。 在此情况下，必须使用一个不同的名称来设置 Azure AD 域服务托管域。 或者，可以消除资源调配现有的域，然后继续启用 Azure AD 域服务。

> [AZURE.WARNING] 在启用该服务后，无法转移到不同的虚拟网络的域服务。


## <a name="network-security-groups-and-subnet-design"></a>网络安全组和子网的设计
[网络安全组 (NSG)](../virtual-network/virtual-networks-nsg.md)包含一个列表的访问控制列表 (ACL) 规则，允许或拒绝对您在虚拟网络中的虚拟机实例的网络流量。 NSGs 可以使用子网或在该子网内的各个虚拟机实例相关联。 当 NSG 与子网关联，ACL 规则将应用于该子网中的所有 VM 实例。 另外，可以限制到单个虚拟机的通信进一步通过将直接向该 VM NSG 相关联。

![建议使用的子网的设计](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>选择一个子网的最佳做法
- 将 Azure AD 域服务部署到 Azure 虚拟网络中**独立的专用子网**。

- 不适用于您的托管域的专用子网 NSGs。 如果您必须将 NSGs 应用于专用子网，则确保**不会阻止服务和管理您的域所需的端口**。

- 不过分限制托管域的专用子网中可用的 IP 地址数。 这种限制会阻止服务使两个域控制器可用于托管域。

- 虚拟网络的**不能将 Azure AD 域服务网关网中的**。


> [AZURE.WARNING] 将关联启用 NSG Azure AD 域服务的子网，可能会破坏微软能够提供服务和管理域。 此外，中断 Azure AD 租户和托管的域之间的同步。 **SLA 不适用于的部署在 NSG 已应用 Azure AD 域服务以阻止更新和管理您的域中。**


### <a name="ports-required-for-azure-ad-domain-services"></a>Azure AD 域服务所需的端口
下列端口所需的 Azure AD 域服务到服务和维护您的托管的域。 确保这些端口未被阻止您使您的托管的域的子网。

| 端口号 | 目的 |
|---|---|
| 443 | Azure AD 租户与同步 |
| 3389 | 您的域的管理 |
| 5986 | 您的域的管理 |
| 636 | 向您的托管域安全 LDAP (LDAPS) 访问 |



## <a name="network-connectivity"></a>网络连接
仅在 Azure 中一个经典虚拟网络中，可以启用 Azure AD 域服务托管的域。 不支持使用 Azure 资源管理器创建虚拟网络。


### <a name="scenarios-for-connecting-azure-networks"></a>Azure 网络连接方案
连接 Azure 的虚拟网络使用托管的域中任何下列部署方案︰

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>使用多个 Azure 的经典虚拟网络中的托管的域
您可以连接到 Azure 经典虚拟网络已在其中启用 Azure AD 域服务的其他 Azure 的经典虚拟网络。 此 VPN 连接可以使用与您在其他虚拟网络中部署的工作负载管理的域。

![经典的虚拟网络连通性](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>使用基于资源管理器的虚拟网络中的托管的域
可以将基于资源管理器的虚拟网络连接到 Azure 经典虚拟网络已在其中启用 Azure AD 域服务。 此连接将使您能够使用托管的域与您在基于资源管理器的虚拟网络中部署的工作负载。

![与传统虚拟网络连接资源管理器](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>网络连接选项

- **使用站点到站点 VPN 连接的 VNet 到 VNet 连接**︰ 将虚拟网络连接到另一个虚拟网络 (VNet VNet) 是类似于将虚拟网络连接到内部网站的位置。 这两种连接类型使用的 VPN 网关来提供使用 IPsec/IKE 安全隧道。

    ![虚拟网络连接使用 VPN 网关](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [详细信息-使用 VPN 网关的虚拟网络连接](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **使用虚拟网络对等的 VNet 到 VNet 连接**︰ 虚拟网络对等是通过 Azure 的骨干网连接在同一个地区的两个虚拟网络的机制。 一旦对，两个虚拟网络将显示为一个用于连接的所有目的。 它们仍然管理作为单独的资源，但这些虚拟网络中的虚拟机可以使用专用 IP 地址的直接相互通信。

    ![使用对等的虚拟网络连通性](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [详细信息-虚拟网络对等](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>相关的内容

- [Azure 的虚拟网络对等](../virtual-network/virtual-network-peering-overview.md)

- [将经典的部署模型为 VNet 到 VNet 连接配置](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure 网络安全组](../virtual-network/virtual-networks-nsg.md)
