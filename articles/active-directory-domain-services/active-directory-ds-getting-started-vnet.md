<properties
    pageTitle="Azure AD 域服务︰ 创建或选择一个虚拟的网络 |Microsoft Azure"
    description="要开始使用 Azure Active Directory 域服务"
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
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>创建或选择一个虚拟网络的 Azure AD 域服务

## <a name="guidelines-to-select-an-azure-virtual-network"></a>选择 Azure 的虚拟网络的指导原则
> [AZURE.NOTE] **在开始之前**︰ 请参阅[网络 Azure AD 域服务的考虑事项](active-directory-ds-networking.md)。


## <a name="task-2-create-an-azure-virtual-network"></a>任务 2︰ 创建 Azure 的虚拟网络
下一步的配置任务是创建一个 Azure 的虚拟网络和其内部子网。 虚拟网络内，则启用此子网中的 Azure AD 域服务。 如果您已经有您希望使用现有的虚拟网络，则可以跳过此步骤。

> [AZURE.NOTE] 确保您创建或选择的 Azure AD 域服务使用 Azure 的虚拟网络属于受 Azure AD 域服务 Azure 地区。 请参阅[Azure 服务区域的](https://azure.microsoft.com/regions/#services/)页后，可以知道 Azure AD 域服务位于 Azure 区域。

请注意下的虚拟网络名称以便您选择右虚拟网络时启用后续配置步骤在 Azure AD 域服务。

执行下面的配置步骤，以创建要在其中启用 Azure AD 域服务 Azure 的虚拟网络。

1. 导航到**Azure 的传统门户网站**([https://manage.windowsazure.com](https://manage.windowsazure.com))。

2. 选择左边窗格中的**网络**节点。

    ![网络节点](./media/active-directory-domain-services-getting-started/networks-node.png)

3. 单击任务窗格底部的页上的**新建**。

    ![虚拟网络节点](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. 在**网络服务**节点，选择**虚拟网络**。

5. 单击**快速创建**来创建虚拟网络。

    ![创建虚拟网络的快速](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. 指定的虚拟网络**名称**。 您还可以选择此网络的配置**地址空间**或**虚拟机的最大计数**。 现在，可以将**DNS 服务器**设置为无。 您可以更新 DNS 服务器服务设置在您启用 Azure AD 域之后。

7. 请确保您在**位置**下拉列表中选择一个支持的 Azure 地区。 请参阅[Azure 服务区域的](https://azure.microsoft.com/regions/#services/)页后，可以知道 Azure AD 域服务位于 Azure 区域。

8. 若要创建虚拟网络，单击**创建虚拟网络**按钮。

    ![为 Azure AD 域服务创建虚拟网络。](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. 创建虚拟网络后，选择的虚拟网络，单击**配置**选项卡。

    ![创建子网](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. 导航到**虚拟网络地址空间**部分。 单击**添加子网**，与**AaddsSubnet**的名称指定一个子网。 单击**保存**以创建子网。

    ![为 Azure AD 域服务创建一个子网。](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>任务 3-启用 Azure AD 域服务
下一步的配置任务是[启用 Azure AD 域服务](active-directory-ds-getting-started-enableaadds.md)。
