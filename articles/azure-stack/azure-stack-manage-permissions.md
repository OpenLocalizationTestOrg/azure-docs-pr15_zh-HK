<properties
    pageTitle="管理资源 （服务管理员和租户） Azure 堆栈中每个用户的权限 |Microsoft Azure"
    description="作为服务管理员或租户，学习如何管理对每个用户的资源的权限。"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>管理用户权限

Azure 堆栈中的用户可以读取、 所有者或订购、 资源组或服务的每个实例的参与者。 例如，用户 A 可能有读者订阅 1 权但有虚拟机 7 的所有者权限。

-   读者︰ 用户可以查看的所有内容，但无法进行任何更改。

-   撰稿者︰ 用户可以管理对资源的访问之外的全部内容。

-   负责人︰ 用户可以管理所有内容，包括对资源的访问。


## <a name="set-access-permissions-for-a-user"></a>设置用户访问权限

1.  使用具有所需的管理资源的所有者权限的帐户登录。

2.  在刀片式服务器中的资源，请单击**访问权限**图标![](media/azure-stack-manage-permissions/image1.png)。

3.  在**用户**刀片式服务器，单击**角色**。

4.  在**角色**刀片式服务器，单击**添加**以添加该用户的权限。

## <a name="next-steps"></a>下一步行动

[添加 Azure 堆栈租户](azure-stack-add-new-user-aad.md)
