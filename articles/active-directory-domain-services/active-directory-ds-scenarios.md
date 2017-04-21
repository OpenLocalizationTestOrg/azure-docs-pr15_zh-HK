<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 部署方案 |Microsoft Azure"
    description="Azure AD 域服务的部署方案"
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
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>部署方案和用例
在本节中，我们看几个方案和用例从 Azure 活动目录 (AD) 域服务中受益。

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>安全、 易于管理的 Azure 的虚拟机
Azure Active Directory 域服务可用于简化的方式管理 Azure 的虚拟机。 Azure 的虚拟机可以加入到托管域，从而使您可以使用您公司的广告凭据登录。 这种方法有助于避免维护每个 Azure 的虚拟机上的本地管理员帐户的凭据管理障碍。

服务器虚拟机加入到托管域，还可以管理并使用组策略保护。 可以应用到 Azure 虚拟机所需的安全基线并锁定按照公司的安全准则。 例如，可以使用组策略管理功能来限制可以在这些虚拟机启动的应用程序的类型。

![Azure 的虚拟机的简化的管理](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

服务器和其他基础设施达到使用寿命结束，Contoso 将移动当前驻留在部署到云上的许多应用程序。 其当前的 IT 标准规定服务器托管公司的应用程序必须加入域，并使用组策略管理。 Contoso 的 IT 管理员倾向于给域连接虚拟机部署在 Azure，使管理更加容易。 因此，管理员和用户可以使用其企业凭据登录。 同时，计算机可以配置为符合必需的安全基线使用组策略。 Contoso 不愿意需要部署、 监视和管理在 Azure 力保 Azure 的虚拟机中的域控制器。 因此，Azure AD 域服务是非常适合于该用例。

**部署注意事项**

请考虑此部署方案下列要点︰

- 默认情况下，Azure AD 域服务所提供的托管的域提供一个单一的平面 OU （组织单位） 结构。 所有加入域的计算机位于一个平面 OU 中。 但是，您可以选择创建自定义 Ou。

- Azure AD 域服务形式的内置 GPO 每个用户和计算机支持简单的组策略容器。 无法为目标 OU/部门的 GP、 执行 WMI 筛选，或创建自定义的 Gpo。

- Azure AD 域服务支持 AD 计算机对象的基本架构。 不能扩展的计算机对象的架构。


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>升降班次的内部应用程序使用 LDAP 绑定到 Azure 的基础结构服务的身份验证

![LDAP 绑定](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso 已购许多年前从 ISV 应用程序内部。 应用程序当前处于维护模式由 ISV 和请求更改应用程序是为 Contoso 非常昂贵。 此应用程序的基于 web 的前端收集使用 web 窗体的用户凭据，然后通过执行 LDAP 绑定到公司活动目录验证用户的身份。 Contoso 谨此应用程序迁移到 Azure 的基础结构服务。 最好是，应用程序的工作一样，而不需要任何更改。 此外，用户应该能够使用其现有企业凭据进行身份验证，而不必重新培训用户以不同的方式做事情。 换句话说，最终用户应该运行该应用程序的直观，迁移应该透明给他们。

**部署注意事项**

请考虑此部署方案下列要点︰

- 请确保该应用程序不需要修改/写入该目录。 不支持到 Azure AD 域服务所提供的托管域 LDAP 写访问权限。

- 您不能更改密码直接针对托管域。 最终用户也可以更改其密码使用 Azure AD 自助密码更改机制或针对内部目录。 这些更改会在托管域自动同步且可用。


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>升降班次的内部应用程序使用 LDAP 读取访问 Azure 的基础结构服务的目录
Contoso 已内部的业务线 (LOB) 应用程序开发几乎在十年前。 此应用程序目录注意而且旨在使用 Windows 服务器 AD。 应用程序使用 LDAP （轻型目录访问协议） 从 Active Directory 中读取有关用户的信息/属性。 应用程序不修改属性或否则写入该目录。 Contoso 想要此应用程序迁移到 Azure 的基础结构服务并淘汰老化的内部部署硬件当前承载此应用程序。 不能重写应用程序以使用现代目录等的基于 REST 的 Azure 广告图形 API 的 Api。 因此，升降班次选项，可将应用程序迁移而无需修改代码或重写应用程序运行在云中，由此需要。

**部署注意事项**

请考虑此部署方案下列要点︰

- 请确保该应用程序不需要修改/写入该目录。 不支持到 Azure AD 域服务所提供的托管域 LDAP 写访问权限。

- 确保应用程序不需要自定义/扩展 Active Directory 架构。 架构扩展不支持在 Azure AD 域服务。


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>内部服务或守护程序将应用程序迁移到 Azure 的基础结构服务
某些应用程序包含多个存储层，一层需要执行身份验证到后端数据库层如一层调用。 活动目录服务帐户通常用于这些用例。 可升降班次到 Azure 的基础结构服务并使用 Azure AD 域服务，这些应用程序的标识需要为此类应用程序。 您可以选择使用相同的服务帐户从本地目录同步到 Azure 的广告。 或者，可以首先创建一个自定义 OU，然后在该 OU 中，部署这种应用程序创建单独的服务帐户。

![使用 WIA 服务帐户](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso 已定制的软件存储库包含的应用程序 web 前端、 SQL 服务器和后端 FTP 服务器。 服务帐户的 Windows 集成身份验证用于验证 web 前端到 FTP 服务器。 Web 前端设置作为一个服务帐户来运行。 后端服务器配置为授予访问权限的服务帐户为 web 前端。 Contoso 不倾向必须部署在云中将 Azure 的基础结构服务此应用程序域控制器虚拟机。 Contoso 的 IT 管理员可以部署承载 web 前端、 SQL 服务器和 FTP 服务器到 Azure 的虚拟机的服务器。 这些计算机被加入到 Azure AD 域服务托管域。 然后，他们可以使用相同的服务帐户在其内部目录中应用程序的身份验证。 此服务帐户同步到 Azure AD 域服务托管域，并可供使用。

**部署注意事项**

请考虑此部署方案下列要点︰

- 确保应用程序使用用户名/密码身份验证。 Azure AD 域服务不支持基于证书/智能卡身份验证。

- 您不能更改密码直接针对托管域。 最终用户也可以更改其密码使用 Azure AD 自助密码更改机制或针对内部目录。 这些更改会在托管域自动同步且可用。


## <a name="azure-remoteapp"></a>Azure 的远程应用程序
Azure RemoteApp 使 Contoso 管理员能够创建加入域的集合。 此功能允许远程应用程序服务的 Azure RemoteApp 在加入域的计算机上运行并访问其他资源使用 Windows 集成身份验证。 Contoso 可以使用 Azure AD 域服务提供管理的域使用 Azure RemoteApp 加入域的集合。

![Azure 的远程应用程序](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

此部署方案有关的详细信息，请参阅远程桌面服务博客文[升降班次 Azure RemoteApp 和 Azure AD 域服务与您的工作负载](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx)。
