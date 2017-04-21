<properties
    pageTitle="Azure AD 域服务︰ 启用 Azure AD 域服务 |Microsoft Azure"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>启用 Azure AD 域服务

## <a name="task-3-enable-azure-ad-domain-services"></a>任务 3︰ 启用 Azure AD 域服务
在此任务中，您可以启用您的目录的 Azure AD 域服务。 执行以下配置步骤以启用您的目录的 Azure AD 域服务。

1. 导航到**Azure 的传统门户网站**([https://manage.windowsazure.com](https://manage.windowsazure.com))。

2. 选择左边窗格中的**活动目录**节点。

3. 选择要为其启用 Azure AD 域服务 Azure AD 租户 （目录）。

    ![选择 Azure 的广告目录](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. 单击**配置**选项卡。

    ![配置目录选项的卡](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. 向下滚动到**域服务**一节。

    ![域服务配置节](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. 切换标题为**是****启用对此目录的域服务**的选项。 您注意到几个更多服务的配置选项 Azure AD 域页面上显示。

    ![启用域服务](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] 当您启用 Azure AD 域服务为您的租户时，Azure 广告将生成并存储 Kerberos 和 NTLM 凭据哈希值所需的用户进行身份验证。

7. 指定**DNS 域名的域服务**。

   - 该目录的默认域名 (即结束**。 onmicrosoft.com**域后缀) 默认为选中状态。

   - 该列表包含所有已配置为 Azure 的广告目录 – 包括验证的域以及您在域选项卡中配置的验证的域。

   - 此外，您还可以键入自定义域名。 在此示例中，我们键入自定义域名 contoso100.com 中。

     > [AZURE.WARNING] 请确保您指定 （例如，contoso100 中的 contoso100.com 的域名） 的域名的域前缀是少于 15 个字符。 不能创建 Azure AD 域服务域与域前缀长度超过 15 个字符。

8. 请确保您已选择托管域的 DNS 域名已不在虚拟的网络中。 具体而言，如果检查︰

   - 您已经具有虚拟的网络上的同一个 DNS 域名的域。

   - 所选的虚拟网络具有与内部网络的 VPN 连接和内部网络上有一个具有相同的 DNS 域名的域。

   - 虚拟的网络上有该名称与现有的云服务。

9. 下一步是选择您想 Azure AD 域服务可用的虚拟网络。 选择虚拟网络和您在下拉列表标题为**连接到此虚拟网络的域服务**的专用子网。

   - 请确保您已指定的虚拟网络属于受 Azure AD 域服务 Azure 地区。 请参阅[Azure 服务区域的](https://azure.microsoft.com/regions/#services/)页后，可以知道 Azure AD 域服务位于 Azure 区域。

   - 属于在 Azure AD 域服务不支持区域的虚拟网络不会显示在下拉列表中。
   
   - Azure AD 域服务用于在虚拟网络中的专用子网。 请确保不选择网关的子网。 请参阅[网络注意事项](active-directory-ds-networking.md)。 

   - 同样，在下拉列表中不显示使用 Azure 资源管理器创建的虚拟网络。 Azure AD 域服务目前不支持基于资源管理器的虚拟网络。

10. 要启用 Azure AD 域服务，请单击页面底部的任务窗格中的**保存**。

11. 页面显示待定... 状态，启用 Azure AD 域服务时您的目录。

    ![启用域服务-挂起状态](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure AD 域服务提供了高可用性对于您的托管域。 启用 Azure AD 域服务后，发现域服务是上一个一个地虚拟网络显示可用的 IP 地址。 不久该服务使您的域的高可用性是不久，显示第二个 IP 地址。 高可用性配置并为您的域处于活动状态时，您会看到**配置**选项卡的**域服务**部分中的两个 IP 地址。

12. 大约 20-30 分钟后，您将看到域服务是可用的虚拟网络**配置**页上的**IP 地址**字段中的第一个 IP 地址。

    ![域服务启用的第一个 IP 配置](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. 高可用性操作为您的域时，您会看到页面上显示的两个 IP 地址。 托管的域出现在您所选的虚拟网络，在这两个 IP 地址上。 记下 IP 地址以便可以更新虚拟网络的 DNS 设置。 这一步使虚拟网络连接到用于操作，如域加入域上的虚拟机。

    ![启用域服务-这两个 IPs 设置](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Azure AD 租户的大小 (编号的用户，组等)，向您的托管域同步需要一段时间。 此同步过程是在后台。 为数以万计的对象具有的大租户，可能需要一两天的时间对所有用户、 组成员身份和凭据以进行同步。

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>任务 4-更新 Azure 的虚拟网络的 DNS 设置
下一步的配置任务是[更新 Azure 的虚拟网络的 DNS 设置](active-directory-ds-getting-started-dns.md)。
