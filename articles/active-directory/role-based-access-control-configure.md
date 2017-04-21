<properties
    pageTitle="在 Azure 的门户中使用基于角色的访问控件 |Microsoft Azure"
    description="入门中访问管理 Azure 门户中基于角色的访问控制。 使用角色分配给您的资源分配权限。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>角色分配用于管理对 Azure 订阅资源的访问

> [AZURE.SELECTOR]
- [管理访问权限的用户或组](role-based-access-control-manage-assignments.md)
- [管理的资源的访问权限](role-based-access-control-configure.md)

Azure 的基于角色的访问控制 (RBAC) 使 Azure 的细粒度访问管理。 使用 RBAC，则可以授予访问量用户执行其任务所需。 这篇文章可以帮助您收到 RBAC Azure 门户中启动并运行。 如果您希望有关 RBAC 如何帮助您管理访问的更多详细信息，请参阅[什么是基于角色的访问控制](role-based-access-control-what-is.md)。

## <a name="view-access"></a>查看访问权限
您可以看到谁有权访问资源、 资源组或订阅从其主刀片在[Azure 的门户](https://portal.azure.com)。 例如，我们希望看到谁有权访问其中一个我们资源组︰

1. 在左侧导航栏中选择**资源组**。  
    ![资源组的图标](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. 从**资源组**刀片式服务器选择资源组的名称。
3. 从左侧的菜单中选择**(IAM) 的访问控制**。  
4. 访问控制刀片列出所有用户、 组和应用程序已被授权访问该资源组。  

    ![用户刀片式服务器中的继承的与分配访问屏幕抓图](./media/role-based-access-control-configure/view-access.png)

请注意，有些用户是**已分配**访问时其他人**继承**它。 访问权限分配给资源组专门或继承从分配到父订阅。

> [AZURE.NOTE] 经典的预订管理员和 co 管理员被视为新的 RBAC 模型中的订阅的所有者。


## <a name="add-access"></a>添加访问权限
授予访问权限的资源、 资源组或订阅，角色分配的范围内。

1. 访问控制刀片在选择**添加**。  
2. 选择您要**选择一个角色**刀片式服务器从指定的角色。
3. 选择您想要授予访问权限的目录中的用户、 组或应用程序。 您可以搜索与显示名称、 电子邮件地址和对象标识符的目录。  

    ![添加用户刀片式服务器-搜索屏幕抓图](./media/role-based-access-control-configure/grant-access2.png)

4. 请选择**确定**以创建分配。 **添加用户**弹出窗口跟踪进度。  
    ![添加用户进度栏的屏幕抓图](./media/role-based-access-control-configure/addinguser_popup.png)

已成功添加角色分配之后, 它将出现在**用户**刀片式服务器。

## <a name="remove-access"></a>删除访问权限

1. 选择访问控制刀片式服务器上的角色分配。
2. 工作分配详细信息刀片式服务器中选择**删除**。  
3. 选择**是**以确认删除。  
    ![用户刀片式服务器-删除角色屏幕抓图](./media/role-based-access-control-configure/remove-access1.png)

不能删除继承的分配。 注意，在下面的图像中删除按钮为灰色。 相反，看**分配在**细节。 请转到列那里去角色分配的资源。

![用户刀片式服务器-继承的访问禁用删除按钮的屏幕快照](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>其他工具来管理访问
可以将角色分配和管理使用 Azure RBAC 命令中非 Azure 门户工具的访问。  请了解更多有关这些系统必备组件并开始使用 Azure RBAC 命令的链接。

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure 的命令行界面](role-based-access-control-manage-access-azure-cli.md)
- [REST API，](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>下一步行动
- [创建访问变更历史记录报告](role-based-access-control-access-change-history-report.md)
- 请参阅[RBAC 内置角色](role-based-access-built-in-roles.md)
- 定义您自己的[自定义 Azure RBAC 角色](role-based-access-control-custom-roles.md)
