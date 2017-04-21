<properties
    pageTitle="将用户分配到一个自定义在 Azure Active Directory 域 |Microsoft Azure"
    description="如何填充自定义域 Azure Active Directory 中的用户帐户。"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>将用户分配到一个自定义的域

您自定义的域添加到 Azure Active Directory 后，您必须添加此域的用户帐户，以便您可以开始对其进行身份验证。

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>用户从您的企业网络上的某个目录同步

如果您已经安装了一个连接之间您内部部署 Active Directory 和 Azure Active Directory，同步可以使用这些帐户填充。 有关如何与您的内部部署 Active Directory 同步 Azure Active Directory 的详细信息，请参阅[集成内部标识使用 Azure 活动目录](active-directory-aadconnect.md)。

## <a name="users-added-and-managed-in-the-cloud"></a>添加并在云环境中管理用户

若要更改现有用户帐户的域︰

1.  打开 Azure 经典门户使用的帐户是全局管理员或用户管理员。

2.  打开您的目录。

3.  选择**用户**选项卡。

4.  从列表中选择用户。

5.  更改用户所属的域，然后选择**保存**。

这也可以使用[Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)或[图形 API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)。

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>创建新用户时选择一个自定义的域

1.  打开 Azure 经典门户使用的帐户是全局管理员或用户管理员。

2.  打开您的目录。

3.  选择**用户**选项卡。

4.  在命令栏中，选择**添加**。

5.  当您添加的用户名时，从域列表中选择自定义的域。

6.  选择**保存**。

## <a name="next-steps"></a>下一步行动

-   [使用自定义域名来简化您的用户的登录体验](active-directory-add-domain.md)

-   [管理自定义域名](active-directory-add-manage-domain-names.md)

-   [在 Azure 广告了解有关管理域的概念](active-directory-add-domain-concepts.md)
