<properties
    pageTitle="Azure AD 连接︰ 下一步和如何管理 Azure AD 连接 |Microsoft Azure"
    description="了解如何扩展的 Azure AD 连接的默认配置和操作任务。"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>下一步和如何管理 Azure AD 连接
以下被高级操作允许您自定义 Azure 活动目录连接以满足组织的需要和要求的主题。  

## <a name="add-additional-sync-administrators"></a>添加其他同步管理员
默认情况下，只有未安装和本地管理员的用户将能够管理安装的同步引擎。 其他人可以访问和管理同步引擎，找到名为 ADSyncAdmins，在本地服务器上的组并将它们添加到此组。

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>将许可证分配给 Azure AD 特优和企业移动用户

现在，您的用户已同步到云中，需要使他们可以获得将与 Office 365 的云应用程序分派许可证。

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>将 Azure AD 津贴或企业移动套件许可证分配
--------------------------------------------------------------------------------
1. 登录到 Azure 门户管理员身份。
2. 在左边，选择**活动目录**。
3. 在活动的目录页上，双击包含您要启用的用户目录。
4. 在目录页的顶部，选择**许可证**。
5. 在许可证页上，选择活动目录津贴或企业移动套件，然后单击**分配**。
6. 在对话框中，选择您要分配许可证的用户，然后单击复选标记图标以保存更改。


## <a name="verifying-the-scheduled-synchronization-task"></a>验证计划好的同步任务
如果您想要查看的同步的状态你可以通过检查在 Azure 的门户。

### <a name="to-verify-the-scheduled-synchronization-task"></a>若要验证计划好的同步任务
--------------------------------------------------------------------------------
1. 登录到 Azure 门户管理员身份。
2. 在左边，选择**活动目录**。
3. 在活动的目录页上，双击包含您要启用的用户目录。
4. 在目录页的顶部，选择**目录集成**。
5. 在集成本地活动目录中注意上次同步时间。

<center>![云](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>启动计划的同步任务
如果您需要运行同步任务你可以通过再次运行通过 Azure AD 连接向导。  您需要提供您 Azure AD 的凭据。  在向导中，选择**自定义同步选项**任务，然后单击向导的下一步。 在结束时，请确保已**启动同步进程一旦完成初始配置**框。

<center>![云](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

在 Azure AD 连接同步的详细信息︰ 计划程序，那么请参阅[Azure AD 连接计划](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>在 Azure AD 连接中可用的其他任务
在 Azure AD 连接的初始安装之后, 您可以总是再次启动该向导从 Azure AD 连接起始页或桌面快捷方式。  您会注意到，再次运行该向导提供了一些新的选项，在窗体中的其他任务。  

下表提供有关每个这些任务和简要说明的摘要。

![加入规则](./media/active-directory-aadconnect-whats-next/addtasks.png)


其他任务 | 说明
------------- | ------------- |
查看所选的方案  |允许您查看您当前的 Azure AD 连接解决方案。  这包括常规设置、 同步的目录、 同步设置等。
自定义同步选项 | 允许您更改当前配置包括添加其他活动目录林的配置或启用同步选项，如用户、 组、 设备或密码写回。
启用调试模式下 |  这允许您将稍后同步的阶段信息，但不是会导出到 Azure 的广告或活动目录。  这使您可以预览同步之前发生。

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
