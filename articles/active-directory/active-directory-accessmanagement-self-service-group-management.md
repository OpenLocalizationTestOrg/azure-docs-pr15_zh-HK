<properties
    pageTitle="设置 Azure Active Directory 自助服务应用程序访问管理 |Microsoft Azure"
    description="自助服务组管理使用户能够创建和管理安全组或 Office 365 Azure Active Directory 中的组以及向用户提供对请求安全组或 Office 365 的组成员身份的可能性"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>设置 Azure Active Directory 自助服务组管理

自助服务组管理功能让用户可以创建和管理安全组或 Office 365 Azure 活动目录 (AD Azure) 中的组。 安全组或 Office 365 的组成员身份，也可以请求的用户和组的所有者可以批准或拒绝成员然后。 这种方式，可以了解业务上下文的成员资格的人员到委派日常控制的组成员身份。 自助服务组管理功能是用只为安全组和 Office 365 组，而不是已启用邮件的安全组或通讯组列表。

自助服务组管理目前包括两个基本方案︰ 委派组管理和自助服务组管理。

- **委派组管理**
  示例是用于管理访问公司使用 SaaS 应用程序的管理员。 管理这些访问权限变得比较麻烦，所以此管理员要求企业所有者创建新组。 管理员将分配到新组中，该应用程序的访问，并向组中添加已访问与应用程序的所有人员。 企业所有者然后可以添加更多用户，并自动向应用程序提供了这些用户。 企业所有者不需要等待管理员来管理用户的访问。 如果管理员授予给经理在不同的业务组相同的权限，然后此人还可以管理其自己用户的访问。 企业所有者和经理都不可以查看或管理其他的用户。 管理员仍可以看到所有用户有权访问的应用程序和块如果所需的访问权限。

- **自助服务组管理**
  这种情况的一个示例就是两个用户有两个他们独立设置的 SharePoint Online 网站。 他们想让彼此的团队访问他们的网站。 若要实现此目的，他们可以在 Azure 广告，创建一个组，每个 SharePoint Online 中选择该组提供对站点的访问。 当有人想访问时，请求访问面板中，从和批准之后他们有权两个 SharePoint Online 网站自动。 之后，其中的一个决定访问站点中的所有人还应都获取特定 SaaS 应用程序的访问。 SaaS 应用程序的管理员可以添加为 SharePoint Online 网站的应用程序的访问权限。 从此以后，获得批准的任何请求，访问两个 SharePoint Online 网站以及对此的 SaaS 应用程序。

## <a name="making-a-group-available-for-end-user-self-service"></a>提供一组用于最终用户自助服务

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，打开 Azure 的广告目录。

2. 在**配置**选项卡上设置为启用**委派组管理**。

3. 将**用户可以创建的安全组**或**用户可以创建 Office 组**设置为已启用。

**用户可以创建的安全组**启用时，您的目录中的所有用户都允许创建新的安全组，并添加到这些组的成员。 这些新的组也将显示在所有其他用户访问权面板。 如果组策略设置允许，则其他用户可以创建请求加入这些组。 如果禁用**的用户可以创建安全组**，则用户不能创建组，并不能更改现有的组，它们所拥有的。 但是，它们仍可以管理这些组的成员资格，批准从其他用户的请求加入它们的组。

**用户可以使用安全组，自助服务**还可用于为用户实现更细粒度的访问控制，通过自助服务组管理。 启用**用户可以创建组**后，您的目录中的所有用户都允许创建新组并将成员添加到这些组。 通过对一些还设置**哪些用户可以使用安全组，自助服务用户**，您是限制组到只有有限的一组用户的管理。 如果此开关设置为一些，必须将用户添加到组 SSGMSecurityGroupsUsers，他们才能创建新组并向其添加成员。 通过设置为所有**用户可以使用安全组的自助服务的人员**，所有用户都启用目录以创建新的组中。

您可以使用**可使用自助服务安全组的组**框中指定组的成员可以使用自助服务的自定义名称。

## <a name="additional-information"></a>其他信息

这些文章在 Azure Active Directory 提供附加信息。

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [配置组设置 azure Active Directory cmdlet](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
