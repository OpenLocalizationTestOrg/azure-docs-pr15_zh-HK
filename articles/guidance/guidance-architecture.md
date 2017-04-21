
<properties
   pageTitle="Azure 指南 |模式和实践 |Microsoft Azure"
   description="Azure 的参考体系结构"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure 的参考体系结构

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_此内容是活动的开发。它是有用今天，因此我们将使它可供预览。我们重视您的反馈。_

我们的参考体系结构方案，通过排列组合在一起的多个相关的体系结构。
每个单独的体系结构提供了建议的做法和规定的步骤，以及体现建议的可执行组件。
在体系结构的许多都是渐进式;构建在前面的体系结构，具有更少的要求。

## <a name="designing-your-infrastructure-for-resiliency"></a>设计您的基础架构的可恢复性

本系列开头推荐的做法来优化 VM 配置和进行故障切换的多区域部署中达到最佳的效果。

- [在 Azure 上运行的 Windows 虚拟机](guidance-compute-single-vm.md)
- [在 Azure 上运行 Linux 虚拟机](guidance-compute-single-vm-linux.md)
- [运行多个虚拟机的可扩展性和可用性](guidance-compute-multi-vm.md)
- [运行 Windows 的 N 层体系结构中的 Vm](guidance-compute-n-tier-vm.md)
- [运行 Linux 的 N 层体系结构中的 Vm](guidance-compute-n-tier-vm-linux.md)
- [在实现高可用性的多个区域中运行 Windows 虚拟机](guidance-compute-multiple-datacenters.md)
- [在实现高可用性的多个区域中运行 Linux 虚拟机](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>您的内部网络连接到 Azure

这一系列启动演示的方法将现有的网络连接到 Azure。 然后它会扩展以包括可用性和安全性要求。

- [实现使用 Azure 的混合网络体系结构和部署 VPN](guidance-hybrid-network-vpn.md)
- [实施与 Azure ExpressRoute 混合网络体系结构](guidance-hybrid-network-expressroute.md)
- [实现高可用性的混合网络体系结构](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>保护您的混合网络

该系列涉及到来自内部数据中心和互联网的安全连接在 Azure 创建 DMZ 行之有效的做法。

- [DMZ Azure 和本地数据中心之间实现](guidance-iaas-ra-secure-vnet-hybrid.md)
- [实现一个 DMZ Azure 和 Internet 之间](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>提供标识服务

这一系列开始通过示范如何使用 Azure Active Directory 提供了 Azure 中的用户身份验证。 然后它会扩展，以涵盖到 Azure，扩展添加基础结构和使用 ADFS 委派的复杂方案。

- [实施 Azure 的活动目录](./guidance-identity-aad.md)
- [扩展到 Azure 的 Active Directory 目录服务 （添加）](./guidance-identity-adds-extend-domain.md)
- [在 Azure 创建活动目录目录服务 (ADDS) 资源目录林](./guidance-identity-adds-resource-forest.md)
- [在 Azure 中实施 Active Directory 联合身份验证服务 (ADF)](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>构建可伸缩的 web 应用程序使用 Azure PaaS

该系列涉及用于构造可扩展性和高可用性的 web 应用程序的建议。 

- [基本的 web 应用程序](guidance-web-apps-basic.md)
- [提高 web 应用程序中的可扩展性](guidance-web-apps-scalability.md)
- [Web 应用程序提供高可用性](guidance-web-apps-multi-region.md)
