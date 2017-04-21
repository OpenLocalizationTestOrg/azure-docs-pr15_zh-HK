<properties
    pageTitle="在 Azure 中的 active Directory 联合身份验证服务 |Microsoft Azure"
    description="在本文中，您将学习如何对高可用性部署在 Azure 中的 AD FS。"
    keywords="部署在 azure 中的 AD FS、 部署 azure adfs，azure adfs，azure ad fs、 部署 adfs、 部署 ad fs 在 azure，adfs 部署 adfs azure 中的，部署在 azure，adfs azure，简介 AD FS，Azure，AD FS 在 Azure，iaas，ADFS，AD FS 将移到 azure adf"
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
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>在 Azure 中的 AD FS 部署 

AD FS 提供简化、 安全标识联合身份验证和 Web 单一登录 (SSO) 功能。 与 Azure 广告联盟或 O365，用户可以使用内部部署的凭据进行身份验证和访问云中的所有资源。 因此，它变得非常重要具有高可用性的 AD FS 基础结构以确保对资源的访问内部部署和在云中。 部署在 Azure 中的 AD FS 可以帮助实现用最少的工作所需的高可用性。
部署在 Azure 中的 AD FS 的多种优点，下面列出了其中的一些︰

* **高可用性**-电源的 Azure 可用性设置，您确保高可用性的基础结构。
* **易于扩展**— 需要更好的性能吗？ 轻松地迁移到更强大的计算机通过 Azure 中只需几次单击
* **跨地理冗余**-使用 Azure 地理冗余可以确保您的基础结构在全球范围内具有高可用性
* **易于管理**--使用高度简化的管理选项在 Azure 门户中，管理您的基础架构是非常轻松和顺利 

## <a name="design-principles"></a>设计原则

![部署设计](./media/active-directory-aadconnect-azure-adfs/deployment.png)

上图显示了建议的基本拓扑结构，开始部署在 Azure 中的 AD FS 基础结构。 背后的各种拓扑结构组成部分的原则如下所示︰

* **直流 / ADFS 服务器**︰ 如果您有少于 1000 用户只需安装在域控制器上的 AD FS 角色。 如果您不要在域控制器上的任何性能影响或有 1000 多家用户，然后将部署在不同的服务器的 AD FS。
* **WAP 服务器**— — 有必要部署 Web 应用程序代理服务器，以便用户可以访问的 AD FS 时它们也不在公司网络。
* **DMZ**: Web 应用程序代理服务器将被置于 DMZ 和 DMZ 和内部子网之间允许仅 TCP/443 访问。
* **负载平衡器**︰ 为了确保 AD FS 和 Web 应用程序代理服务器的高可用性，建议对 Web 应用程序代理服务器的 AD FS 服务器和 Azure 负载平衡器使用内部负载平衡器。
* **可用性设置**︰ 以提供到 AD FS 部署冗余，建议您组合两个或多个虚拟机在类似工作负载的可用性设置。 此配置可以确保，在计划内或计划外维护事件中，至少一个虚拟机将可用
* **存储帐户**︰ 建议有两个存储帐户。 有一个单一的存储帐户可能会导致单点故障的创建，并且可能导致部署变得不可用存储帐户的停机可能性不大的方案中。 两个存储帐户有助于将相关联的每个断层个存储帐户。
* **网络划分**︰ 应在一个单独的 DMZ 网络中部署 Web 应用程序代理服务器。 可以将一个虚拟网络划分为两个子网，然后将部署在独立的子网中的 Web 应用程序代理服务器。 只是，您可以配置每个子网的网络安全组设置，并允许两个子网之间只有必需的通信。 每个部署方案下提供更多详细信息

##<a name="steps-to-deploy-ad-fs-in-azure"></a>部署在 Azure 中的 AD FS 的步骤

这一节中提到的步骤概述了部署指南下面 Azure 中所显示的 AD FS 基础结构。

### <a name="1-deploying-the-network"></a>1.部署网络

如上所述，您可以要么在单个虚拟网络中创建两个子网也可创建两个完全不同的虚拟网络 (VNet)。 本文将重点放在部署一个虚拟网络，并将其划分为两个子网。 这是当前比较简单的方法，因为两个单独的 VNets 要求 VNet 到 VNet 网关进行通信。

**1.1 创建虚拟网络**

![创建虚拟网络](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
在 Azure 的门户中，选择虚拟网络，并且您可以部署的虚拟网络，并立即与只需单击一个子网。 INT 子网也是，现在准备要添加的虚拟机。
下一步是添加到网络，即 DMZ 子网的另一个子网。 只需创建 DMZ 子网，

* 选择新创建的网络
* 在属性中选择子网
* 子网中单击添加按钮面板
* 提供创建子网的子网的名称和地址空间信息

![子网](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![DMZ 的子网](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2.安全组创建网络**

网络安全组 (NSG) 包含一个列表的访问控制列表 (ACL) 规则，允许或拒绝对您在虚拟网络中的虚拟机实例的网络流量。 NSGs 可以使用子网或在该子网内的各个虚拟机实例相关联。 当 NSG 与子网关联，ACL 规则将应用于该子网中的所有 VM 实例。
用于本指南中，我们将创建两个 NSGs︰ 两个分别用于内部网络和 DMZ。 他们将被标记为 NSG_INT 和 NSG_DMZ 分别。

![创建 NSG](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

NSG 创建后，将有 0 站和 0 的出站规则。 在各自的服务器上的角色已安装并正常工作后，可以根据所需的安全级别进行入站和出站规则。

![初始化 NSG](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

NSGs 创建后，将与子网关联 NSG_INT INT 和 NSG_DMZ 与网 DMZ。 下面给出了示例屏幕快照︰

![NSG 配置](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* 单击要打开的面板的子网的子网
* 选择要与 NSG 关联的子网 

完成配置后，面板的子网应类似于如下︰

![NSG 之后的子网](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3.为内部创建连接**

我们需要连接到内部为了部署在 azure 中的域控制器 (DC)。 Azure 提供各种连接选项连接到 Azure 基础结构的内部部署基础结构。

* 点到站点
* 虚拟网络站点到站点
* ExpressRoute

建议使用 ExpressRoute。 ExpressRoute 允许您创建之间 Azure 数据中心和基础架构，以在您的场所或在主机托管环境中的专用连接。 通过公共互联网不会 ExpressRoute 连接。 他们通过互联网提供更多可靠性、 更快的速度、 更低的延迟和更高的安全性，比典型的连接。
虽然建议使用 ExpressRoute，您可以选择最适合您的组织的任何连接方法。 若要了解有关 ExpressRoute 的详细信息，并使用 ExpressRoute 的各种连接选项，阅读[技术概要︰ ExpressRoute](https://aka.ms/Azure/ExpressRoute)。

### <a name="2-create-storage-accounts"></a>2.创建存储帐户

为维护高可用性并避免依赖于一个单一的存储帐户，您可以创建两个存储帐户。 在每个可用性组中的计算机将分成两组，然后将每个组分配一个单独的存储帐户。

![创建存储帐户](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3.创建可用性设置

对于每个角色 （DC/AD FS 和 WAP），创建将包含 2 机每个的最小值的可用性设置。 这将有助于实现更高的可用性，为每个角色。 创建可用性设置，而至关重要的以下决定︰
* **容错域**︰ 同一故障域中的虚拟机共享相同的电源插座和物理网络交换机。 建议 2 故障域的最小值。 默认值为 3，可以保留该名称为此部署
* **更新域**︰ 机属于同一更新域将重启在一起更新的过程中。 您想要有 2 个更新域的最小值。 默认值为 5，而可以保留它的目的这一部署是因为

![可用性设置](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

创建下面的可用性设置

| 可用性设置 | 角色 | 容错域 | 更新域 |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | DC/ADF | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4.部署虚拟机
下一步是部署将承载基础结构中的不同角色的虚拟机。 至少两台计算机的可用性集中每个建议。 创建基本部署的六个虚拟机。

| 机器 | 角色 | 子网 | 可用性设置 | 存储帐户 | IP 地址 |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|DC/ADF|INT|contosodcset|contososac1|静态|
|contosodc2|DC/ADF|INT|contosodcset|contososac2|静态|
|contosowap1|WAP|DMZ|contosowapset|contososac1|静态|
|contosowap2|WAP|DMZ|contosowapset|contososac2|静态|

您可能已经注意到，尚未指定任何 NSG。 这是因为 azure 使您可以在子网级别使用 NSG。 然后，您可以使用单独的 NSG 与无论是子网，否则 NIC 对象相关联的控制计算机网络通信。 阅读更多有关[什么是网络安全组 (NSG)](https://aka.ms/Azure/NSG)。
如果您正在管理 DNS，建议静态 IP 地址。 您可以使用 Azure DNS 并在您的域的 DNS 记录，请参阅通过其 Azure Fqdn 的新机。
虚拟机窗格应类似于下面部署完成后︰

![部署虚拟机](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5.配置域控制器 / AD FS 服务器
 为了对任何传入的请求进行身份验证，AD FS 将需要与域控制器联系。 要将成本高昂的旅行从 Azure 保存到本地 DC 进行身份验证，建议部署 Azure 中的域控制器的复制副本。 为了获得高可用性，建议创建最小 2 个域控制器的可用性。

|域控制器|角色|存储帐户|
|:-----:|:-----:|:-----:|
|contosodc1|复制副本|contososac1|
|contosodc2|复制副本|contososac2|

* 将两个服务器提升为副本与 DNS 的域控制器
* 通过安装 AD FS 角色使用服务器管理器来配置 AD FS 服务器。

###<a name="6---deploying-internal-load-balancer-ilb"></a>6.部署内部负载平衡器 (ILB)

**6.1.创建 ILB**

若要部署 ILB，选择的负载平衡器在 Azure 门户上的单击添加 （+）。
>[AZURE.NOTE] 如果看不到**负载平衡器**菜单中，，单击左下角的门户和滚动**浏览**直到看到**负载平衡器**。  然后单击以将其添加到菜单的黄星。 现在，选择新的负载平衡器图标，以打开要开始配置负载平衡器的面板。

![浏览负载平衡器](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **名称**︰ 任何合适的名称为负载平衡器
* **方案**︰ 由于该负载平衡器将放置在 AD FS 服务器前面，并且应为内部网络连接，请仅选择"内部"
* **虚拟网络**︰ 选择要在其中部署 AD FS 的虚拟网络
* **子网**︰ 选择内部网
* **IP 地址分配**︰ 动态

![内部负载平衡器](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
单击后创建和部署 ILB，它会出现在列表中的负载平衡器︰

![ILB 后的负载平衡器](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
下一步是配置池后端和后端探测器。

**6.2.配置 ILB 后端池**

在负载平衡器面板中，选择新创建的 ILB。 将打开设置面板。 
1.  从设置面板中选择池后端
2.  在添加后端库面板中，单击添加虚拟机上
3.  您将看到一个面板，您可以在其中选择可用性设置
4.  选择 AD FS 可用性组

![配置 ILB 后端池](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3。 配置的探测器**

在 ILB 设置面板中，选择探测器。
1.  单击添加
2.  探测器提供的详细信息。 **名称**︰ 探测器名称 b。 **协议**︰ TCP c。 **端口**︰ 443 (HTTPS) d。 **间隔**︰ 5 （默认值）--这是的 ILB 将探测中后端池 e 机时间间隔。 **不正常的阈值限制**︰ 2 （默认值 val ue） – 这是阈值的连续探测失败之后，ILB 将声明池中后端机无响应状态，并停止向它发送通信流。

![配置 ILB 探测器](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4.创建负载平衡规则**

为有效地平衡通信量，ILB 应该配置负载平衡规则。 若要创建负载平衡规则， 
1.  选择负载平衡规则从 ILB 的设置面板
2.  在负载平衡规则面板中添加上单击
3.  在添加负载平衡规则面板。 **名称**︰ 提供规则 b 的名称。 **协议**︰ 选择 TCP c。 **端口**︰ 443 d。 **后端端口**︰ 443 e。 **后端池**︰ 选择为 AD FS 群集早期 f 创建的池。 **探测器**︰ 选择前面创建的 AD FS 服务器探测

![配置 ILB 平衡规则](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5.与 ILB 更新 DNS**

转到您的 DNS 服务器并创建 CNAME ILB。 CNAME 应指向 ILB 的 IP 地址的 IP 地址与联合身份验证服务。 例如，如果 ILB DIP 地址是 10.3.0.8，并安装联合身份验证服务为 fs.contoso.com，然后创建指向 10.3.0.8 fs.contoso.com 的 CNAME。
这将确保有关 fs.contoso.com 结尾的所有通信在 ILB 上并相应地路由。

###<a name="7---configuring-the-web-application-proxy-server"></a>7.配置 Web 应用程序代理服务器

**7.1.配置访问 AD FS 服务器的 Web 应用程序代理服务器**

为了确保 Web 应用程序代理服务器就能够到达 ILB 背后的 AD FS 服务器，创建的记录在 %systemroot%\system32\drivers\etc\hosts ILB。 请注意，可分辨的名称 (DN) 应联合身份验证服务名称，例如 fs.contoso.com。 然后 IP 输入应该是 ILB 的 IP 地址 (如示例中所示的 10.3.0.8)。

**7.2.安装 Web 应用程序代理服务器角色**

确保 Web 应用程序代理服务器就能够到达 ILB 背后的 AD FS 服务器之后下, 一步可以安装的 Web 应用程序代理服务器。 Web 应用程序代理服务器未加入到域中。 通过选择远程访问角色，在两个 Web 应用程序代理服务器上安装 Web 应用程序的代理角色。 服务器管理器会指导您完成 WAP 安装。
有关如何部署 WAP 的详细信息，请阅读[安装和配置 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383662.aspx)。

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8.部署互联网面临 （公共） 负载平衡器

**8.1.创建互联网面临 （公共） 负载平衡器**
 
在 Azure 的门户中，选择负载平衡器，然后单击添加。 在创建负载平衡器面板中，输入以下信息
1. **名称**︰ 负载平衡器的名称
2. **方案**︰ 公共--此选项告知 Azure 该负载平衡器将需要一个公用地址。
3. **IP 地址**︰ 创建一个新的 IP 地址 （动态）

![互联网对称负载平衡器](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

在部署之后，负载平衡器将出现在负载平衡器列表中。

![负载平衡器列表](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2.公用 IP 为指定 DNS 标签**

新创建的负载平衡器条目配置的面板显示负载平衡器面板中单击。 请按照以下步骤为公用 IP 配置 DNS 标签︰
1.  单击公用 IP 地址。 此操作将打开公用 IP 和其设置面板
2.  单击配置
3.  提供 DNS 标签。 这将成为您可以从任何地方，例如 contosofs.westus.cloudapp.azure.com 访问公用 DNS 标签。 外部 DNS 解析为 DNS 标签外部负载平衡器 (contosofs.westus.cloudapp.azure.com) 的联合身份验证服务 （如 fs.contoso.com) 中，您可以添加一个条目。

![配置负载平衡器面向 internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![配置负载平衡器 (DNS) 面向 internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3.互联网面临 （公共） 负载平衡器配置池后端** 

按照相同的步骤如下所示创建内部负载平衡器，将后端池为互联网面临 （公共） 负载平衡器配置为设置 WAP 服务器的可用性。 例如，contosowapset。

![配置互联网面临负载平衡器池后端](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8.4.配置探测器**

按照相同的步骤如下所示配置内部负载平衡器配置为 WAP 服务器的后端池的探测器。

![配置互联网面临负载平衡器的探测器](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5.创建负载平衡规则**

按照相同的步骤，如在 ILB 配置负载平衡规则为 TCP 443。

![配置互联网面临负载平衡器的平衡法则](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9.确保网络的安全

**9.1.保护内部子网**

总之，您需要以下规则有效地保护您 （在下面列出的顺序） 的内部子网

|规则|说明|流量|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| 允许从 DMZ 的 HTTPS 通信 | 入站 |
|DenyInternetOutbound| 不能访问互联网 | 出站 |

![INT 访问规则 （入站）](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[注释]: <> (![INT 访问规则 （入站）](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [注释]: <> (![INT 访问规则 （出站）](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2。 保护 DMZ 子网**

|规则|说明|流量|
|:----|:----|:------:|
|AllowHTTPSFromInternet| 允许从 internet 到 DMZ 的 HTTPS | 入站|
|DenyInternetOutbound|  除 HTTPS 到互联网的任何内容被阻止 | 出站 |

![外部访问规则 （入站）](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[注释]: <> (![扩展访问规则 （入站）](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [注释]: <> (![扩展访问规则 （出站）](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] 如果客户端用户证书身份验证 (使用 X509 clientTLS 身份验证用户证书) 是必需的则 AD FS 需要 TCP 端口 49443 启用入站访问。

###<a name="10--test-the-ad-fs-sign-in"></a>10.AD FS 签入测试

最简单的方法是测试 AD FS 是通过使用 IdpInitiatedSignon.aspx 页。 为了能够执行操作，则需要启用 AD FS 的属性 IdpInitiatedSignOn。 请按照下面的步骤来验证您的 AD FS 设置
1.  运行以下 cmdlet 在 AD FS 服务器上，使用 PowerShell，将其设置为启用。
    一组 AdfsProperties EnableIdPInitiatedSignonPage $true 
2.  从任何外部计算机访问 https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3.  您应该看到 AD FS 页下方如下所示︰

![测试登录页](./media/active-directory-aadconnect-azure-adfs/test1.png)

成功登录，它将为您提供一条成功消息如下所示︰

![测试成功](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>部署在 Azure 中的 AD FS 的模板

该模板将部署一个 6 机安装中，2 个域控制器，AD FS 和 WAP 的。

[AD FS 在 Azure 部署模板](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

您可以使用现有虚拟网络或同时部署此模板创建新的 VNET。 在部署过程中参数的用法说明下面列出了可用于自定义部署的各种参数。 

| 参数 | 说明 |
|:--------|:-----|
|位置| 要我们到如东部部署资源的区域。 |
|StorageAccountType| 创建存储帐户的类型|
|VirtualNetworkUsage| 如果将创建新虚拟网络连接或使用现有的指示|
|VirtualNetworkName| 创建，必需在现有或新建的虚拟网络使用的虚拟网络名称|
|VirtualNetworkResourceGroupName| 指定现有的虚拟网络所在的资源组的名称。 当使用一个现有的虚拟网络，这就个必选参数，以便部署可以查找现有的虚拟网络的 ID|
|VirtualNetworkAddressRange| 新的 VNET，则为强制参数创建新的虚拟网络地址范围|
|InternalSubnetName| 内部子网的名称，必须在这两个虚拟网络使用选项 （新的或现有的）|
|InternalSubnetAddressRange| 如果创建新的虚拟网络包含服务器的域控制器和 ADF，强制性的内部子网的地址范围。|
|DMZSubnetAddressRange| Dmz 子网，其中包含 Windows 应用程序代理服务器，必须创建新的虚拟网络地址范围。|
|DMZSubnetName| 内部子网的名称，必需的对于这两个虚拟网络使用选项 （新的或现有的）。 |
|ADDC01NICIPAddress| 第一个域控制器的内部 IP 地址，该 IP 地址将静态分配到 DC 和必须是内部子网内的有效 ip 地址|
|ADDC02NICIPAddress| 第二个域控制器的内部 IP 地址，该 IP 地址将静态分配到 DC 和必须是内部子网内的有效 ip 地址|
|ADFS01NICIPAddress| ADFS 第一个服务器的内部 IP 地址，该 IP 地址将静态分配给 ADFS 的服务器和必须是内部子网内的有效 ip 地址|
|ADFS02NICIPAddress| 第二个 ADFS 服务器的内部 IP 地址，该 IP 地址将静态分配给 ADFS 服务器和必须是内部子网内的有效 ip 地址|
|WAP01NICIPAddress| 第一台 WAP 服务器的内部 IP 地址，该 IP 地址将静态分配到 WAP 服务器并且必须是有效的 ip 地址在 DMZ 子网|
|WAP02NICIPAddress| 第二个 WAP 服务器的内部 IP 地址，该 IP 地址将静态分配到 WAP 服务器和必须是有效的 ip 地址在 DMZ 子网|
|ADFSLoadBalancerPrivateIPAddress| ADFS 负载平衡器的内部 IP 地址，该 IP 地址将静态分配到负载平衡器和必须是内部子网内的有效 ip 地址|
|ADDCVMNamePrefix| 域控制器虚拟机名称前缀|
|ADFSVMNamePrefix| ADFS 服务器虚拟机名称前缀|
|WAPVMNamePrefix| WAP 服务器虚拟机名称前缀|
|ADDCVMSize| 域控制器虚拟机大小|
|ADFSVMSize| ADFS 服务器的虚拟机大小|
|WAPVMSize| WAP 服务器的虚拟机大小|
|AdminUserName| 虚拟机的本地管理员名称|
|AdminPassword| 虚拟机的本地管理员帐户的密码|

## <a name="additional-resources"></a>其他资源
* [可用性设置](https://aka.ms/Azure/Availability ) 
* [Azure 负载平衡器](https://aka.ms/Azure/ILB)
* [内部负载平衡器](https://aka.ms/Azure/ILB/Internal)
* [互联网面向负载平衡器](https://aka.ms/Azure/ILB/Internet)
* [存储帐户](https://aka.ms/Azure/Storage )
* [Azure 的虚拟网络](https://aka.ms/Azure/VNet)
* [AD FS 和 Web 应用程序代理服务器链接](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>下一步行动

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
* [配置和管理您使用 Azure AD 连接的 AD FS](active-directory-aadconnectfed-whatis.md)
* [高可用性跨地区 AD FS Azure 使用 Azure 流量管理器中的部署](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




