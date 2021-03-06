
<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定混合身份生命周期采用策略 |Microsoft Azure"
    description="可帮助定义可用于每个生命周期阶段的选项根据混合身份管理任务。"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>确定混合身份生命周期采用战略
在此任务中，您将定义混合身份解决方案以满足业务要求，[确定混合身份管理任务](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)中定义的标识管理战略。


若要定义根据以前介绍过这一步的端到端身份生命周期的混合身份管理任务，必须要考虑的选项可用于每个生命周期阶段。

## <a name="access-management-and-provisioning"></a>访问管理和资源调配
具有良好的客户访问权限管理解决方案，您的组织可以跟踪精确地跨组织有权访问哪些信息。

访问控制是集中式、 单点自动配置系统的一个重要功能。 除了保护敏感信息，访问控件公开具有现有帐户未经的授权或不再有必要。 若要控制过时帐户，拥有帐户的用户的授权信息与资源调配系统链接在一起帐户信息。 通常，授权用户的身份信息都保留在数据库和目录的人力资源。

复杂的 IT 企业中的帐户包含数百个参数，以便定义机构，并可以通过资源调配系统控制这些详细信息。 新用户可以用来自权威来源提供的数据来标识。 访问请求审批功能启动批准 （或拒绝） 资源调配他们的资源的进程。


| 生命周期管理阶段          | 内部部署                                                                                                                                                                                                                                                       | 云                                                                                                                                                                                                                                                                                                                     | 混合                                                                                   |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| 帐户管理和资源调配 | 通过使用 Active Directory® 域服务 (AD DS) 服务器角色，可以创建用户和资源管理、 可扩展、 安全而且易于管理基础结构并为启用目录的应用程序，如 Microsoft Exchange Server® 提供的支持。 <br><br> [您可以配置标识管理器通过 AD DS 中的组](https://technet.microsoft.com/library/ff686261.aspx) <br>[您可以配置在 AD DS 中的用户](https://technet.microsoft.com/library/ff686263.aspx) <br><br> 管理员可以使用访问控制来管理用户访问共享资源，出于安全考虑。 访问控制在对象级别的访问权限或权限对象，设置不同级别的管理在活动目录中，例如完全控制、 写、 读或拒绝访问。 在 Active Directory 中的访问控制定义了不同的用户可以使用 Active Directory 对象。 默认情况下，Active Directory 中的对象上的权限设置为最安全的设置。 | 您必须为将访问 Microsoft 云服务的每个用户创建一个帐户。 此外可以更改用户帐户，或在不再需要时删除它们。 默认情况下用户不具有管理员权限，但也可以选择分配它们。 有关详细信息，请参阅[在 Azure 广告管理用户](active-directory-create-users.md)。 <br><br> 在 Azure Active Directory 中的主要功能之一是能够管理对资源的访问。 这些资源可以是一部分的目录，如管理通过角色中的目录或目录，如 SaaS 应用程序、 Azure 服务和 SharePoint 网站或对内部资源与外部资源的对象的权限的情况。 <br><br> 该中心的 Azure 活动目录的访问权限管理解决方案是安全组。 资源所有者 （或目录管理员） 可以分配一组提供直接到他们所拥有的资源的访问。 组的成员将获得访问权限，以及资源所有者可以委派管理给别人--例如，部门经理或帮助台管理员组的成员列表的权限<br> <br> 在 Azure 广告主题的管理组管理组通过访问提供了更多信息。| 通过同步和联盟云扩展 Active Directory 标识 |

## <a name="role-based-access-control"></a>基于角色的访问控制
基于角色的访问控制 (RBAC) 使用角色和资源调配策略评估、 测试和实施您的业务流程和规则向用户授予访问权限。 密钥管理员创建配置策略，并将用户分配给角色，这些角色的资源定义的权利集。 RBAC 扩展标识管理解决方案使用基于软件的流程并减少用户在设置过程中的人工交互。
Azure AD RBAC 使公司限制的个人可以做后他有权 Azure 管理门户的操作量。 通过使用 RBAC 控制对门户网站的访问，IT 管理员 ca 使用以下访问权限管理方法委派访问权限︰

- **基于组的角色分配**︰ 您分配访问权限可以同步的 Azure AD 组从您本地的活动目录。 这使您可以利用您的组织已在工具和流程的管理组中的现有投资。 您还可以使用 Azure AD 特优的委派的组管理功能。
- **利用内置的 Azure 中的角色**︰ 您可以使用三个角色 — — 所有者、 参与者和读卡器，以确保用户和组有权执行完成其工作所需的任务。
- **Granular 对资源的访问**︰ 您可以将角色分配给用户和组的特定订阅、 资源组或单个 Azure 网站或数据库等资源。 这种方式，您可以确保用户具有访问他们所需的所有资源并不能访问它们不需要管理的资源。

## <a name="provisioning-and-other-customization-options"></a>资源调配和其他自定义选项
您的团队可以使用业务计划和需求来决定多少自定义标识解决方案。 例如，一家大型企业可能需要分阶段的推广计划工作流和基于增量供应得到广泛的跨地区使用的应用程序的时间线的自定义适配器。 两个或多个应用程序设置的整个组织，在测试成功后可能提供另一个自定义计划。 可以自定义应用程序的用户交互，并调配资源的程序可能会更改以适应自动化的资源调配。

您可以取消设置中删除服务或组件。 例如，撤消帐户即从资源删除帐户。

调配资源的混合模型结合请求和基于角色的方法，同时支持的 Azure 的广告。 雇员或托管的系统的子集，企业可能需要自动化与基于角色的工作分配的访问权限。 所有其他访问请求或异常通过基于请求的模型，还可以处理业务。 一些公司可能开始手动分配，并发展混合模型，是企图在将来时间完全基于角色的部署。

其他公司可能会发现出于业务原因，以实现完整的基于角色的资源调配，是不切实际和目标需要目标作为一种混合方法。 其他公司仍可能满意仅基于请求的资源调配，并不想购买额外的工作来定义和管理基于角色的、 自动化的资源调配策略。

## <a name="license-management"></a>许可证管理
在 Azure 广告中的基于组的许可证管理允许管理员将用户分配给安全组和 Azure 广告自动将许可证分配给组的所有成员。 如果用户随后添加，或从组中删除，许可证将自动分配或删除相应地。

您可以使用从同步的组内部广告或在 Azure 广告管理。 配对这与 Azure 广告高级自助式组管理可以方便地委托许可证分配给适当的决策者。 您完全可以放心，自动分拣许可证冲突和位置数据丢失等问题。

## <a name="self-regulating-user-administration"></a>自我控制用户管理
当您的组织开始跨所有内部组织调配资源时，实现自我管理的用户管理功能。 您可以跨越组织边界实现的优势和好处的配置的用户。 在此环境中，用户的状态的更改将自动反映在访问权限跨组织边界和地理区域。 可以降低供应成本，并简化访问和审核过程。 实现认识到您的组织中实现基于角色的访问控制，实现端到端访问管理的全部潜能。 您可以减少管理成本，通过控制用户资源调配的自动化过程。 您可以自动执行安全策略的强制执行，从而提高安全性和精简和集中用户生命周期管理和资源调配的大量用户。

>[AZURE.NOTE]
有关详细信息，请参阅设置自助服务应用程序访问管理 Azure 广告

基于许可证 （基于权利） 的 Azure 广告通过激活订阅您 Azure 的广告目录/服务租户的服务工作。 激活订阅后可以由目录/服务管理员管理服务能力，并已授权的用户使用。 有关详细信息，请参阅如何进行授权工作的 Azure 广告吗？
与其他第三方提供商集成

Azure 的 Active Directory 提供单一登录和增强到成千上万的 SaaS 应用程序和内部部署 web 应用程序的应用程序访问安全。 Azure Active Directory 支持的 SaaS 应用程序的应用程序库的详细列表，请参阅 Azure Active Directory 联合身份验证兼容性列表︰ 可用来实现单一登录的第三方身份提供程序

## <a name="define-synchronization-management"></a>定义同步管理
与 Azure AD 集成您的内部目录使您的用户更高效通过提供公用的标识访问云以及内部的资源。 通过这种集成用户和组织可以利用下列︰

- 在内部部署或基于云的服务利用 Windows 服务器的 Active Directory，然后连接到 Azure Active Directory，组织可以为用户提供常见的混合身份。
- 管理员可以根据应用程序的资源、 设备和用户标识、 网络位置和多因素身份验证的条件访问。
- 在 Azure AD 到 Office 365，Intune，SaaS 应用程序和第三方应用程序中，用户可以利用其公共帐户通过身份。
- 开发人员可以构建应用程序利用通用标识模型集成到部署 Active Directory 或 Azure 应用程序，基于云的应用程序

下图中标识同步过程的高级视图的一个示例。

![](./media/hybrid-id-design-considerations/identitysync.png)

标识同步过程

查看下表，以比较同步选项︰

| 同步管理选项          | 优势                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 缺点                                                                                                                                                                                                                                                                                  |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 基于同步 （通过目录同步或 AADConnect） | 用户和组同步从内部部署和云 <br>  **策略控制**︰ 通过 Active Directory，使管理员能够管理密码策略、 工作站、 限制、 锁定控件，以及更多内容，而不必在云中执行其他任务，则可以设置帐户策略。  <br>  **访问控制**︰ 以便服务可以通过企业环境中，通过和 / 或在线服务器，可以限制对云服务的访问。 <br>  减少了支持电话︰ 如果用户有较少需要记忆的密码，他们不太可能会忘记它们。 <br>  安全性︰ 用户身份和信息保护，因为所有的服务器和服务使用单一登录，掌握和控制内部。 <br>  用于强身份验证的支持︰ 可以在云服务中使用强身份验证 （也称为双因素身份验证）。 但是，如果您使用强身份验证，您必须使用单一登录。 |                                                                                                                                                                                                                                                                                                |
| （通过 AD FS) 联合身份验证基于           | 启用的安全令牌服务 (STS)。 在配置 STS 提供单一登录访问 Microsoft 云服务时，您将创建联合的信任内部 STS 和 Azure AD 租户中所指定的联盟的域之间。 <br> 允许最终用户使用相同的凭据集来获取访问多个资源 <br>最终用户不需要维护多个凭据集。 然而，用户必须提供给每个其凭据 B2B 和 B2C 的方案支持。 参与的资源。，                                                                                                                                                                                                                                                                                                                                                                                                             | 需要专业的人员为部署和维护的专用上 prem AD FS 服务器。 如果您计划使用 AD FS 您 STS，有使用强身份验证的限制。 有关详细信息，请参阅[配置 AD fs 2.0 的高级选项](http://go.microsoft.com/fwlink/?linkid=235649)。 |

>[AZURE.NOTE]
有关详细信息，请参阅[集成内部标识使用 Azure 活动目录](active-directory-aadconnect.md)。


## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
