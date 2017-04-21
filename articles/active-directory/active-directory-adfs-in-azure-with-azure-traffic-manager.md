<properties
    pageTitle="在 Azure 使用 Azure 流量管理器中的高可用性跨地区 AD FS 部署 |Microsoft Azure"
    description="在本文中，您将学习如何对高可用性部署在 Azure 中的 AD FS。"
    keywords="Ad fs 使用 Azure 的流量管理器中，adfs Azure 流量管理器中，地理位置、 多数据中心、 地理数据中心、 多个地理位置的数据中心，部署在 azure 中的 AD FS、 部署 azure adfs，azure adfs，azure ad fs、 部署 adfs、 部署 ad fs 在 azure，adfs 部署 adfs azure 中的，部署 AD FS 在 azure，adfs azure，AD FS，Azure，AD FS 在 Azure，iaas 简介ADFS，移到 azure 的 adf"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>高可用性跨地区 AD FS Azure 使用 Azure 流量管理器中的部署

[AD FS Azure 中的部署](active-directory-aadconnect-azure-adfs.md)提供了如何将简单的 AD FS 基础结构部署为您的组织在 Azure 中的逐步指导。 本文提供了在 Azure 创建 AD FS 跨地区部署下一步使用[Azure 流量管理器](../traffic-manager/traffic-manager-overview.md)。 Azure 的流量管理器有助于通过使创建地理位置跨高可用性和高性能 AD FS 基础结构为您的组织使用的路由方法可用以满足不同的需求，从基础结构的范围。

启用高可用性跨地区 AD FS 基础结构︰

* **消除了单点故障︰**具有故障转移功能的 Azure 流量管理器中，您可以实现高度可用的 AD FS 基础架构，即使在发生故障后在全球范围内的部分数据中心之一
* **提高了性能︰**您可以使用本文中的建议的部署提供高性能的 AD FS 基础架构，它能够帮助用户更快地进行身份验证。 

##<a name="design-principles"></a>设计原则

![总体设计](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

将同一文章在 Azure 中的 AD FS 部署中的设计原则中所列的基本设计原则。 上图显示了一个简单的扩展到其他地理区域的基本部署。 下面是一些点，扩展到新的地理区域部署时要考虑

* **虚拟网络︰**应在部署额外的 AD FS 基础架构所需的地理区域中创建新的虚拟网络。 在上图中看 Geo1 VNET 和 Geo2 VNET 为每个地理区域中的两个虚拟网络。

* **域控制器和 AD FS 服务器新地理 VNET:**建议部署的域控制器的新的地理区域中，以便在新区域的 AD FS 服务器不必与在远离另一个域控制器联系网络来完成身份验证，从而提高性能。

* **存储帐户︰**存储帐户都有区域相关联。 因为您将部署新的地理区域中的计算机，必须创建新的存储帐户，以区域中使用。  

* **网络安全组︰**如在其他地理区域中不能使用存储帐户，网络在区域中创建的安全组。 因此，您将需要创建新的网络安全组类似于 INT 和 DMZ 子网的第一个地理区域中的新的地理区域。

* **的公用 IP 地址的 DNS 标签︰**终结点，可以引用 azure 的流量管理器仅通过 DNS 标签。 因此，您需要创建外部负载平衡器的公用 IP 地址的 DNS 标签。

* **Azure 的流量管理器︰**Microsoft Azure 流量管理器允许您控制与您在世界各地的不同数据中心中运行的服务终结点的用户流量的分布。 在 DNS 级别工作 azure 的流量管理器。 它使用 DNS 响应最终用户将流量引导至全球分布的终结点。 客户端连接到这些终结点直接。 具有不同的性能、 加权和优先级的路由选择选项，可以轻松地选择最适合您的组织需要的路由选择选项。 

* **两个区域之间的 V 网络到 V 网络连接︰**您不需要本身具有的虚拟网络之间的连接。 由于每个虚拟网络有权访问的域控制器，而且本身具有 AD FS 和 WAP 服务器，可以没有任何不同区域中的虚拟网络之间的连接。 

##<a name="steps-to-integrate-azure-traffic-manager"></a>将集成 Azure 流量管理器的步骤

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>部署在新的地理区域的 AD FS
按照步骤和[Azure 中的 AD FS 部署](active-directory-aadconnect-azure-adfs.md)来部署新的地理区域中的同一个拓扑中的准则。

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>互联网面临 （公共） 负载均衡器的公用 IP 地址的 DNS 标签
如上所述，Azure 流量管理器只能引用 DNS 标签作为终结点，因此很重要的若要创建外部负载平衡器的公用 IP 地址的 DNS 标签。 下面的屏幕快照显示了如何配置公用 IP 地址的 DNS 标签。 

![DNS 标签](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>部署 Azure 的流量管理器

请按照下面的步骤来创建一个流量管理器配置文件。 有关详细信息，您也可以指[管理 Azure 流量管理器配置文件](../traffic-manager/traffic-manager-manage-profiles.md)。

1. **创建一个流量管理器配置文件︰**为通讯管理器配置文件提供一个唯一的名称。 此配置文件的名称是 DNS 名称的一部分，并充当通讯管理器域名称标签的前缀。 名称 / 前缀添加到。 trafficmanager.net 创建 DNS 标签为通讯管理器。 下面的屏幕截图显示通讯管理器 DNS 前缀设置为 mysts，结果 DNS 标签将 mysts.trafficmanager.net。 

    ![通信管理器配置文件创建](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **的流量路由方法︰**有流量管理器中可用的三种路由选择选项︰

    * 优先级 
    * 性能
    * 加权
    
    **性能**是推荐的选项，以实现响应度很高的 AD FS 基础结构。 但是，您可以选择最适合您的部署需要任何路由方法。 AD FS 功能不受所选的路由选择选项。 详细信息，请参阅[通信量管理器通信路由方法](../traffic-manager/traffic-manager-routing-methods.md)。 在该示例中上面的屏幕快照可以看到所选的**性能**方法。
   
3.  **配置终结点︰**在通讯管理器页中，在终结点上单击并选择添加。 这将打开添加终结点页面类似于下面的屏幕截图
 
    ![配置终结点](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    对于不同的输入，请遵循下面的准则︰

    **类型︰**随着我们将指向 Azure 的公用 IP 地址，请选择 Azure 的终结点。

    **名称︰**创建一个要与该终结点关联的名称。 这不是 DNS 名称和无关的 DNS 记录。

    **的目标资源类型︰**选择公用 IP 地址作为此属性的值。 

    **的目标资源︰**这将使您可以选择从不同的 DNS 标签必须用下您的订购。 选择对的 DNS 标签。

    添加希望 Azure 流量管理器通信路由到每个地理区域的终结点。
    有关详细信息以及如何添加 / 流量管理器中配置终结点的详细的步骤，请参阅[添加、 禁用、 启用或删除终结点](../traffic-manager/traffic-manager-endpoints.md)
    
4. **配置探测器︰**在通讯管理器页中，单击配置。 在配置页中，您需要更改监视器设置，以在 HTTP 端口 80 和相对路径 /adfs/probe 探测器

    ![配置探测器](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **确保这些终结点的状态是联机配置完成后**。 降级状态的所有终结点时，Azure 流量管理器将执行路由通信流假设，诊断程序不正确，并且所有终结点是可到达的最佳尝试。

5. **修改 DNS 记录为 Azure 流量管理器︰**联合身份验证服务应该是一个 CNAME 到 Azure 流量管理器的 DNS 名称。 创建 CNAME 在公用 DNS 记录，以便无论谁正试图达到联合身份验证服务真正到达 Azure 流量管理器。

    例如，要点联合身份验证服务 fs.fabidentity.com 到流量管理器中，您需要更新您的 DNS 资源记录，为下面︰

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>测试路由和 AD FS 登录   

###<a name="routing-test"></a>测试路由

工艺路线的最基本测试是尝试使用 ping 从一台计算机在每个地理区域中的联合身份验证服务 DNS 名称。 根据选择的路由选择方法，它实际执行 ping 操作的端点将反映在 ping 显示。 例如，如果您选择性能路由，然后级别的客户端区域的终结点将到达。 下面是两个从两个不同地区的客户端计算机 ping 该快照，一个在东亚地区，一个中西部美国。 

![测试路由](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS 签入测试

测试 AD FS 的最简单方法是使用 IdpInitiatedSignon.aspx 页。 为了能够执行操作，则需要启用 AD FS 的属性 IdpInitiatedSignOn。 请按照下面的步骤来验证您的 AD FS 设置
 
1. 运行以下 cmdlet 在 AD FS 服务器上，使用 PowerShell，将其设置为启用。 一组 AdfsProperties EnableIdPInitiatedSignonPage $true
2. 从任何外部计算机访问 https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. 您应该看到 AD FS 页下方如下所示︰

    ![ADFS 测试-身份验证质询](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    并成功登录，它将为您提供一条成功消息如下所示︰

    ![ADFS 测试-身份验证成功](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>相关的链接
* [在 Azure 中的基本 AD FS 部署](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure 通信管理器](../traffic-manager/traffic-manager-overview.md)
* [通信管理器通信路由方法](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>下一步行动
* [管理 Azure 流量管理器的配置文件](../traffic-manager/traffic-manager-manage-profiles.md)
* [添加、 禁用、 启用或删除终结点](../traffic-manager/traffic-manager-endpoints.md) 

