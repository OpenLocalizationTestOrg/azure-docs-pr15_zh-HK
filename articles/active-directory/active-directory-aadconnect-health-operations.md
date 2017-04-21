<properties
    pageTitle="Azure AD 连接的健康运营。"
    description="本文介绍了部署 Azure AD 连接健康后可以执行的其他操作。"
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD 连接健康操作

以下主题描述可以使用 Azure AD 连接状况执行的各种操作。

## <a name="enable-email-notifications"></a>启用电子邮件通知
您可以配置 Azure AD 连接健康服务发送电子邮件通知时生成警报，指示您身份的基础结构并不健康。 这会生成警报时，以及当它被标记为已解析。 请按照下面的说明来配置电子邮件通知。

![Azure AD 连接健康电子邮件通知发现](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] 默认情况下，禁用电子邮件通知。


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>若要启用 Azure 的 AD 连接健康电子邮件通知

1. 打开您希望接收电子邮件通知服务警报刀片式服务器。
2. 单击操作栏上的"通知设置"按钮。
3. 将电子邮件通知开关设置为 ON。
4. 选择配置所有全局管理员可收到电子邮件通知复选框。
5. 如果您想要接收电子邮件通知，在任何其他电子邮件地址，您可以在额外的电子邮件收件人框中指定。 若要从该列表中删除电子邮件地址，请右键单击该项并选择删除。
6. 若要完成所做的更改，请单击"保存"。 仅在您选择"保存"后，所有更改将都会影响。

## <a name="delete-a-server-or-service-instance"></a>删除服务器或服务实例

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>从 Azure AD 连接的运行状况服务中删除服务器
在某些情况下，您可能希望删除从被监视的服务器。 请按照下面的说明从 Azure AD 连接的运行状况服务中删除服务器。

在删除服务器时，请注意以下︰

- 此操作将停止从该服务器中收集任何进一步的数据。 此服务器将从监视服务。 后此操作，您将不能查看新警报，此服务器的监视或使用率分析数据。
- 此操作不会卸载或从服务器上删除健康代理。 如果您不具有执行此步骤之前卸载健康代理，您可能会看到错误事件与健康代理服务器上。
- 此操作不会删除已从该服务器中收集的数据。 根据 Microsoft Azure 数据保留策略，将删除该数据。
- 执行此操作后，如果您想要开始监视同一台服务器，将需要卸载并重新安装在此服务器上的运行状况代理。


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>从 Azure AD 连接的健康服务中删除服务器

Azure AD AD FS 和 Azure AD 连接 （同步） 的连接运行状况︰

1. 打开刀片式服务器刀片式服务器列表中选择要删除的服务器名称。
2. 在刀片式服务器，单击操作栏上的"删除"按钮。
3. 确认在确认框中键入服务器的名称中删除服务器的操作。
4. 请单击"删除"按钮。

Azure AD 连接为 AD DS 运行状况︰

1. 打开域控制器操控板。
2. 选择要删除的域控制器。
3. 单击操作栏上的"删除所选"按钮。
4. 确认要删除服务器的操作。
5. 请单击"删除"按钮。

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>从 Azure AD 连接的运行状况服务中删除某个服务实例

在某些情况下，您可能希望删除某个服务实例。 请按照下面的说明从 Azure AD 连接的运行状况服务中删除某个服务实例。

在删除某个服务实例时，请注意以下︰

- 此操作将删除当前服务实例监视服务。
- 此操作不会卸载或删除此服务实例的一部分作为被监视的任何的服务器健康代理。 如果您不具有执行此步骤之前卸载健康代理，您可能看到错误事件相关的健康代理的服务器上。
- 根据 Microsoft Azure 数据保留策略，将删除此服务实例中的所有数据。
- 之后执行此操作，如果您想要启动监视服务，请卸载并重新安装在将监视的所有服务器上运行状况代理。 执行此操作后，如果您想要开始监视同一台服务器，将需要卸载并重新安装在此服务器上的运行状况代理。


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>若要删除某个服务实例从 Azure AD 连接的健康服务

1. 选择您想要删除的服务标识符 （场名称） 从服务列表刀片式服务器打开服务刀片式服务器。
2. 在刀片式服务器，单击操作栏上的"删除"按钮。
3. 在确认框中键入以确认该服务名称。 (例如︰ sts.contoso.com)
4. 请单击"删除"按钮。
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>管理与基于角色的访问权限的访问控制
### <a name="overview"></a>概述
Azure AD 连接状况的[基于角色的访问控制](role-based-access-control-configure.md)提供给用户和/或外部全局管理员组访问 Azure AD 连接的健康服务。 这通过向目标的用户和/或组分配角色，并提供一种机制来限制在您的目录中的全局管理员。

#### <a name="roles"></a>角色
Azure AD 连接健康支持下列内置角色。

| 角色 | 权限 |
| ----------- | ---------- |
| 所有者 | 所有者可以***管理访问权限***（如分配角色给用户/组） 中，***查看所有信息***（如都查看预警） 从门户和***更改设置***（例如电子邮件通知） 在 Azure AD 连接的健康。 <br>默认情况下，Azure AD 全局管理员分配此角色，不能对此进行更改。  |
|参与者|  参与者可以***查看所有信息***（如都查看预警） 从门户和***更改设置***（例如电子邮件通知） 在 Azure AD 连接的健康。|
|读取器| 读者可以***查看所有信息***（如都查看预警） 从在 Azure AD 连接健康门户。|

所有其他角色 （例如管理员用户访问或 DevTest 实验室用户），即使使用门户的经验，具有访问在 Azure AD 连接的健康没有影响。

#### <a name="access-scope"></a>访问范围

Azure AD 连接支持在两个级别的管理访问权限︰

- ***所有服务实例***︰ 这是大多数客户的建议的路径，在所有的角色类型正在由 Azure AD 连接运行状况监视控制所有服务实例 （例如 ADFS 场） 的访问。

- ***服务实例***︰ 在某些情况下，您可能需要分别根据角色类型或服务实例的访问。 在这种情况下，您可以管理服务实例级别的访问。  

如果最终用户具有访问任何一个目录或服务实例级别授予权限。


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>如何允许用户或组访问 Azure AD 连接运行状况
#### <a name="steps-1-select-the-appropriate-access-scope"></a>步骤 1︰ 选择适当的访问权限范围
若要允许用户访问*所有服务实例*级别在 Azure AD 连接的健康，请在 Azure AD 连接健康打开主刀片式服务器。<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>步骤 2︰ 添加用户、 组和角色分配
1. 单击"用户"部分中的配置部分。<br>
![Azure AD 连接健康 RBAC 主刀片式服务器](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. 选择"添加"
3. 选择如"所有者"的"角色"<br>
![Azure AD 连接健康 RBAC 中添加用户](./media/active-directory-aadconnect-health/RBAC_add.png)
4. 键入的名称或目标的用户或组的标识符。 您可以一次选择一个或多个用户或组。 单击"选择"。
![Azure AD 连接健康 RBAC 选择用户](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. 选择"确定"。<br>

6. 角色分配完成后，用户和/或组将显示在列表中。<br>
![Azure AD 连接健康 RBAC 用户列表](./media/active-directory-aadconnect-health/RBAC_user_list.png)

这些步骤允许列出的用户和组访问权限根据及其分配角色。
>[AZURE.NOTE]
- 全局管理员始终具有完全访问权限的所有操作，但全局管理员帐户将不会显示在上面的列表中。
- 在 Azure AD 连接状况不支持"邀请用户"功能。

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>第 3 步︰ 与用户或组共享的刀片位置
1. 指派权限之后, 用户可以转到[http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth)访问 Azure AD 连接运行状况。
2. 一次在刀片式服务器，用户可固定刀片或到仪表板不同的部分只需单击"附到仪表板"<br>
![Azure AD 连接健康 RBAC 针刀片式服务器](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] 具有"读者"角色分配权限的用户不能执行"创建"的操作来获取从 Azure 市场 Azure AD 连接健康扩展。 此用户仍可以访问刀片式服务器转到上面的链接。 后续的使用情况，用户可以将固定刀片式服务器到仪表板。

### <a name="remove-users-andor-groups"></a>删除用户和/或组
您可以删除用户或组添加到 Azure AD 连接健康角色基于访问控制部件，通过右键单击并选择删除。<br>
![Azure AD 连接健康 RBAC 移除用户](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接健康代理安装](active-directory-aadconnect-health-agent-install.md)
* [与 AD FS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adfs.md)
* [对于同步使用 Azure AD 连接运行状况](active-directory-aadconnect-health-sync.md)
* [与 AD DS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adds.md)
* [Azure AD 连接健康常见问题解答](active-directory-aadconnect-health-faq.md)
* [Azure AD 连接健康版本历史记录](active-directory-aadconnect-health-version-history.md)
