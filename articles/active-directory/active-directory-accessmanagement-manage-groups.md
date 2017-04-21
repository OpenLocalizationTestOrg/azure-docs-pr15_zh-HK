<properties
    pageTitle="管理在 Azure Active Directory 组 |Microsoft Azure"
    description="如何创建和管理组来管理 Azure 使用 Azure 活动目录的用户。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>在 Azure Active Directory 中的管理组

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-groups-create-azure-portal.md)
- [Azure 的传统门户网站](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Azure 活动目录 (AD Azure) 用户管理的功能之一是能够创建的用户组。 一组用于执行管理任务，如每次将许可证或权限分配给多个用户。 您还可以使用组来分配访问权限

- 例如，在目录中的对象的资源
- SaaS 应用程序、 Azure 服务、 SharePoint 站点目录的外部资源或本地资源

此外，资源所有者可以分配到 Azure AD 组由其他人所拥有的资源访问。 此工作分配授予访问资源的组的成员。 然后，该组的所有者管理组的成员身份。 实际上，资源所有者委托给组的所有者的权限，若要将用户分配给其的资源。

## <a name="how-do-i-create-a-group"></a>如何创建一组？

根据订阅您的组织的服务，您可以创建一组使用下列方法之一︰
- Azure 的传统门户网站
- Office 365 客户门户
- Windows Intune 帐户门户

我们将描述在 Azure 的经典门户中执行任务。 有关使用非 Azure 门户管理 Azure 的广告目录的详细信息，请参阅[管理 Azure 的广告目录](active-directory-administer.md)。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，选择**活动目录**，然后选择为您的组织的目录的名称。

2. 选择**组**选项卡。

3. 选择**添加组**。

4. 在**添加组**窗口中指定的名称和组的说明。


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>如何添加或删除单个用户的安全组中？

**若要将单个用户添加到组**

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，选择**活动目录**，然后选择为您的组织的目录的名称。

2. 选择**组**选项卡。

3. 打开要向其添加成员的组。 打开所选组的**成员**选项卡上，如果它没有被显示。

4. 选择**添加成员**。

5. 在**添加成员**页中，选择用户或组，您想要添加作为此组的成员的名称。 请确保该名称将添加到**所选**窗格。


**若要从组中删除单个用户**

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，选择**活动目录**，然后选择为您的组织的目录的名称。

2. 选择**组**选项卡。

3. 打开要从中移除成员的组。

4. 选择**成员**选项卡，选择您想要删除此组，成员的名称，然后单击**删除**。

6. 在提示确认要从组中删除该成员。


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>如何动态地管理组的成员身份？

在 Azure 广告，您可以很容易设置一个简单的规则来确定哪些用户是组的成员。 一个简单的规则是一种进行一个比较。 例如，如果一组分配给一个 SaaS 应用程序时，您可以设置规则添加用户拥有职称为"销售代表"。 此规则然后授予访问此 SaaS 应用程序的所有用户与该目录中的职务。

当用户更改任何属性，系统计算所有动态组规则目录以查看用户的属性更改将触发了任何组中添加或删除。 如果用户满足一组中的规则，它们是作为成员添加到该组。 如果他们不能再满足它们是成员的一组规则，它们作为成员组中被删除的。

> [AZURE.NOTE] 您可以设置安全组或 Office 365 组动态成员身份的规则。 基于组的分配给应用程序当前不支持嵌套的组成员身份。
>
> 动态成员身份的组需要 Azure AD 特优的许可分配给
>
> - 管理员负责管理组中的规则
> - 组的所有成员

**若要启用动态组的成员身份**

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，选择**活动目录**，然后选择为您的组织的目录的名称。

2. 选择**组**选项卡，然后打开要编辑的组。

3. 选择**配置**选项卡，然后再将**启用动态成员身份**设置为**是**。

4. 设置简单单一规则来控制此组功能如何动态成员身份的组。 请确保**中添加用户，**选项被选中，然后选择从列表 （例如，部门、 职务等） 的用户属性

5. 接下来，选择一个条件 （不等于、 等于、 不启动与、 开头，不包含、 包含、 不匹配，匹配）。

6. 指定选定的用户属性的比较值。

若要了解有关如何为动态组成员身份创建*高级*规则 （规则可以包含多个比较），请参阅[使用属性来创建高级的规则](active-directory-accessmanagement-groups-with-advanced-rules.md)。

## <a name="additional-information"></a>其他信息

这些文章在 Azure Active Directory 提供附加信息。

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [配置组设置 azure Active Directory cmdlet](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
