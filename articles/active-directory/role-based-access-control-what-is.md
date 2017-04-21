<properties
    pageTitle="基于角色的访问控件 |Microsoft Azure"
    description="入门中访问管理 Azure 门户中的 Azure 的基于角色的访问控制。 使用角色分配给您的目录中的权限。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>开始使用 Azure 门户中访问管理

安全中心的公司应重点放在为员工提供他们所需的确切权限。 过多的权限公开给攻击者的帐户。 太少的权限意味着员工不能获得有效地完成其工作。 Azure 的基于角色的访问控制 (RBAC) 可帮助解决这一问题通过提供 Azure 的细粒度访问管理。

使用 RBAC，可以隔离在团队内的职责，并授予执行各自作业所需的用户只能访问量。 不需要为每个人提供无限制的权限在 Azure 订阅或资源，您可以允许特定的操作。 例如，使用 RBAC 让一个雇员管理虚拟机在订阅中，而另一个可以管理 SQL 数据库中相同的订阅。

## <a name="basics-of-access-management-in-azure"></a>在 Azure 中访问管理基础知识
每个 Azure 订阅程序与一个 Azure 活动目录 (AD) 目录。 用户、 组和应用程序从该目录可以管理 Azure 订阅中的资源。 分配这些使用 Azure 门户、 Azure 的命令行工具和 Azure 管理 Api 的访问权限。

通过将适当的 RBAC 角色分配给用户、 组和应用程序在特定作用域授予访问权限。 角色分配的范围可以是订阅、 资源组或单个资源。 在某个父作用域分配的角色也对它所包含的子授予访问权限。 例如，对某一资源组具有访问权限的用户可以管理包含，如网站、 虚拟机和子网的所有资源。

![关系之间 Azure Active Directory 元素的关系图](./media/role-based-access-control-what-is/rbac_aad.png)

您分配的 RBAC 角色决定了哪些用户、 组或应用程序可以管理在该范围内的资源。

## <a name="built-in-roles"></a>内置角色
Azure RBAC 具有三个基本的角色应用于所有资源类型︰

- **所有者**拥有完全访问所有资源包括委托给其他用户访问的权限。
- **参与者**可以创建和管理所有类型的 Azure 的资源，但不能授予其他人访问。
- **读取器**可以查看现有的 Azure 资源。

其余的 Azure 中的 RBAC 角色允许管理 Azure 的特定资源。 例如，虚拟机参与者角色允许用户创建和管理虚拟机。 它并不赋予其访问虚拟网络或虚拟机连接到的子网。

[RBAC 内置角色](role-based-access-built-in-roles.md)列出可用在 Azure 中的角色。 它指定的操作和每个内置角色向用户授予权限的范围。 如果您希望定义自己的角色进行更多控制，请参阅如何构建[自定义 Azure RBAC 角色](role-based-access-control-custom-roles.md)。

## <a name="resource-hierarchy-and-access-inheritance"></a>资源层次结构和访问权限继承
- 只能将一个目录所属 Azure 中每个**订阅**。
- 每个**资源组**所属只有一个订阅。
- 每个**资源**都属于只有一个资源组。

在子作用域继承在父作用域授予访问权限。 例如︰

- 您到 Azure AD 组订阅范围内分配读者角色。 该组的成员可以查看每个资源组和资源在预订中。
- 参与者角色分配应用程序在资源组作用域。 它可以管理该资源组，但不是能在订阅其他资源组中的所有类型的资源。

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC 与经典预订管理员
经典的预订管理员和 co 管理员具有完全访问权限的 Azure 的订阅。 他们可以管理资源[Azure 门户](https://portal.azure.com)使用 Azure 资源管理器 Api 或[Azure 的传统门户网站](https://manage.windowsazure.com)和 Azure 经典的部署模型。 在 RBAC 模型中，经典的管理员分配在订阅范围所有者角色。

只有 Azure 的入口和新的 Azure 资源管理器 Api 支持 Azure RBAC。 用户和 RBAC 角色分配的应用程序不能使用传统管理门户和 Azure 经典的部署模型。

## <a name="authorization-for-management-vs-data-operations"></a>管理与数据操作的授权
Azure RBAC 只在 Azure 门户和 Azure 资源管理器 Api 支持管理操作的 Azure 的资源。 它不能授权 Azure 的资源的所有数据级操作。 例如，可以授权他人管理存储帐户，但不是到 blob 或表中存储帐户不能。 同样，SQL 数据库是可以管理的但不是在它的表。

## <a name="next-steps"></a>下一步行动
- 开始使用[Azure 门户中基于角色的访问控制](role-based-access-control-configure.md)。
- 请参阅[RBAC 内置角色](role-based-access-built-in-roles.md)
- 定义您自己的[自定义 Azure RBAC 角色](role-based-access-control-custom-roles.md)
