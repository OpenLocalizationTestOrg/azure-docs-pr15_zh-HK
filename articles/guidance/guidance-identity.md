<properties
   pageTitle="管理 Azure 中的标识 |Microsoft Azure"
   description="解释并将跨越 Azure 上的部署/云边界的混合系统中管理标识可用的不同方法进行比较。"
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>管理 Azure 中的标识

在大多数基于 Windows 的企业系统，您将使用活动目录 (AD) 向您的应用程序提供标识管理服务。 广告适用于内部的环境中，但当您将扩展到云网络基础结构必须进行有关如何管理标识一些重要决策。 您应该展开内部域合并在云环境中的虚拟机？ 在云中中, 创建新域，如果是，如何？ 在云环境中实现您自己的目录林中还是应该准备使用的 Azure 活动目录 (AAD)？

本文介绍了一些常见的选项来满足这种情况下，所带来的挑战，可帮助您确定哪种解决方案最符合您需要根据您的要求。

## <a name="using-azure-active-directory"></a>使用 Azure 的活动目录

您可以使用 AAD 在云中创建 AD 域并将其链接到内部部署 AD 域。 AAD 使您能够配置单一登录 (SSO) 的用户运行的应用程序通过云访问。

[![0]][0]

AAD 是一个简单的方法来在云环境中实现一个安全域。 它被利用许多 Microsoft 应用程序，如 Microsoft Office 365。 

使用 AAD 的好处︰

- 没有必要维护云中的 Active Directory 基础结构。 完全管理和 Microsoft 维护的 AAD。

- AAD 提供相同的标识信息是可用的内部部署。

- 身份验证可以发生在 Azure，减少以联系内部域的外部应用程序和用户的需要。

使用 AAD 时要考虑的事项︰

- 标识服务仅限于用户和组。 它不能进行身份验证服务和计算机帐户。

- 您必须与您的内部域保持同步的 AAD 目录配置连接。 

- 您有责任发布应用程序的用户可以访问通过 AAD 群中。

详细信息，请参阅[实施 Azure Active Directory][implementing-aad]。

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>在云中使用 Active Directory 加入到本地林

可以承载广告目录服务 (AD DS) 内部，但它可以更有效地将这一功能和 AD 存储库复制到云混合方案中应用程序的元素在 Azure 中的位置。 这种方法有助于减少由发送身份验证滞后时间，云从本地授权请求回到 AD DS 运行内部。 

[![1]][1]

此方法要求您在云中创建您自己的域并将它加入到本地林。 创建虚拟机提供 AD DS 服务。

在云环境中使用一个单独的域的好处︰

- 提供验证用户、 服务和计算机帐户内部的能力并在云中。

- 提供访问权限是可用的内部部署相同的标识信息。

- 没有必要来管理不同的 AD 林;在云环境中的域可以属于内部林。

- 您可以应用组策略定义的内部部署到云环境中的域的 GPO 对象。

在云环境中使用一个单独的域的注意事项︰

- 要求您创建和管理自己的 AD DS 服务器和云环境中的域。

- 可能在云环境中的域服务器和服务器运行内部之间某些同步延迟。

有关如何配置此体系结构的信息，请参阅[扩展活动目录目录服务 （添加） 到 Azure][extending-adds]。

## <a name="using-active-directory-with-a-separate-forest"></a>使用单独的目录林中的活动目录

运行部署活动目录 (AD) 一个组织可能具有包含多个不同的域目录林。 您可以使用域必须是独立的可能是出于安全考虑，保持的功能区域之间提供隔离但可以共享之间建立信任关系的域的信息。

[![2]][2]

使用单独的域组织可以利用 Azure 通过将一个或多个这些域重新定位到云中的单独的目录林中。 另外，组织可能希望保留所有的云资源逻辑上不同于那些保持内部，并作为林还持有群中的一部分在其自己的目录，存储云资源的信息。

在云环境中使用单独的目录林中的好处︰

- 可以实现内部身份和单独的 Azure 专用标识。

- 没有必要从内部复制 AD 林到 Azure，减少网络延迟的影响。

注意事项︰

- 对于在云中的内部标识身份验证执行额外的网络*跃点*上部署 AD 服务器。

- 要求您在云中，实现 AD DS 服务器和目录林和目录林之间的适当的信任关系的建立。

[活动目录目录服务 (ADDS) 资源林在 Azure 中]的文档[adds-forest-in-azure]介绍了如何实现这种方法更详细。

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>使用 Azure 使用 Active Directory 联合身份验证服务 (ADF)

ADFS 可以运行内部，但在混合方案中应用程序都位于 Azure 可以更高效地实现此功能，在云中，如下所示。

[![3]][3]

这种体系结构是非常有用的︰

- 利用联合的授权公开向伙伴组织的 web 应用程序的解决方案。

- 支持从防火墙外的组织运行的 web 浏览器访问的系统。

- 使用户能够通过从经授权的外部设备，例如远程计算机、 笔记本电脑和其他移动设备连接来访问 web 应用程序的系统。 

ADFS 中使用 Azure 的优点︰

- 您可以利用声明感知应用程序。

- 它提供了信任进行身份验证的外部合作伙伴的能力。

- 它提供了与大组的身份验证协议的兼容性。

ADFS 中使用 Azure 的注意事项︰

- 它要求您在云中实现您自己的添加，ADFS 和 ADFS Web 应用程序代理服务器。

- 这种体系结构可以复杂配置。

详细信息，请参阅[实施活动目录联合身份验证服务 (ADF) 中 Azure][adfs-in-azure]。

## <a name="next-steps"></a>下一步行动

下面的资源解释如何实现本文中描述的体系结构。

- [实施 Azure 的活动目录][implementing-aad]
- [扩展到 Azure 的 Active Directory 目录服务 （添加）][extending-adds]
- [在 Azure 创建活动目录目录服务 (ADDS) 资源目录林][adds-forest-in-azure]
- [在 Azure 中实施 Active Directory 联合身份验证服务 (ADF)][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "使用 Active Directory 的 Azure 的云标识体系结构"
[1]: ./media/guidance-identity/figure2.png "安全与 Active Directory 的混合网络体系结构"
[2]: ./media/guidance-identity/figure3.png "安全与独立 AD 域和林的混合网络体系结构"
[3]: ./media/guidance-identity/figure4.png "安全使用 ADFS 混合网络体系结构"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md