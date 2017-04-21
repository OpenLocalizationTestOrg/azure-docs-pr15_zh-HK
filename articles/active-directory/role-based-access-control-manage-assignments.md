<properties
    pageTitle="Azure 的资源访问权限分配 |Microsoft Azure"
    description="查看和管理所有基于角色的访问控制工作分配的任何用户或组在 Azure 门户"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>查看 Azure 门户-公共预览中的用户和组的访问权限分配

> [AZURE.SELECTOR]
- [管理访问权限的用户或组](role-based-access-control-manage-assignments.md)
- [管理的资源的访问权限](role-based-access-control-configure.md)

与基于角色的访问控制 (RBAC) 在 Azure Active Directory 预览中，您可以管理访问 Azure 的资源。 [在预览中是什么？](active-directory-preview-explainer.md)

分配与 RBAC 的访问是细粒度的因为有两种方法可以限制的权限︰

- **范围︰**RBAC 角色分配的应用范围限定为特定预订、 资源组或资源。 为单个资源授予访问权限的用户无法访问相同的订阅中的任何其他资源。
- **角色︰**在分配的范围内，访问范围将会缩小甚至进一步通过分配一个角色。 角色可以是高级别的，像所有者，或特定的情况下，虚拟机读卡器等。

角色只可以从订购、 资源组或资源分配的范围内分配。 但是，您可以查看给定的用户或组的所有访问权限分配在同一个地方。

获取有关如何[使用角色分配管理 Azure 订阅资源访问权限](role-based-access-control-configure.md)的详细信息。

##  <a name="view-access-assignments"></a>查看访问权限分配

若要查找指定给单个用户或组的访问，开始在 Azure [Azure 门户](http://portal.azure.com)中的 Active Directory 中。

1. 选择**Azure 的活动目录**。 如果在导航列表中看不到此选项，选择**更多的服务**，然后向下滚动到**Azure Active Directory**。
2. 选择**用户和组**和然后**所有用户**或**所有组**。 对于此示例，我们将专注于单个用户。
    ![管理用户和组在 Azure Active Directory 的屏幕抓图](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. 按名称或用户名搜索的用户。
4. 用户刀片式服务器上选择**Azure 的资源**。 显示分派给该用户的所有访问权限。

### <a name="read-permissions-to-view-assignments"></a>若要查看工作分配的读取权限

此页仅显示具有读取权限的访问权限分配。 例如，您有读取访问权限订阅的转到 Azure 资源刀片式服务器来检查用户的工作分配。 可以看到她访问分配的订阅，但不会看到她还订阅 B.有访问权限分配

## <a name="delete-access-assignments"></a>删除访问权限分配

从该刀片式服务器，您可以删除已直接指派给用户或组的访问权限分配。 如果访问分配继承父组，您需要转到预订的资源和管理分配中存在。

1. 分派给用户或组的所有访问权限的列表中，选择要从中删除。
2. **删除**选择，然后选择**是**确认。
    ![删除访问分配的屏幕抓图](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>相关的主题

- 开始[使用角色分配管理 Azure 订阅资源访问权限](role-based-access-control-configure.md)的基于角色的访问控制
- 请参阅[RBAC 内置角色](role-based-access-built-in-roles.md)
