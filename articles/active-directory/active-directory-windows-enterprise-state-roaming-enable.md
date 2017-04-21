<properties
    pageTitle="漫游在 Azure 的 Active Directory 中启用企业状况 |Microsoft Azure"
    description="有关在 Windows 设备企业状态漫游设置的常见问题。 企业状态漫游跨其 Windows 设备为用户提供统一的体验，并减少了用于配置新设备所需的时间。"
    services="active-directory"
    keywords="漫游，windows 云如何启用企业状态漫游的企业状态"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>启用企业状况在 Azure 的 Active Directory 中漫游

企业状态漫游可供使用特优 Azure 活动目录 (AD Azure) 订阅任何组织。 有关如何获取 Azure 广告订阅的详细信息，请参阅[Azure 的广告产品页](https://azure.microsoft.com/services/active-directory)。

启用漫游企业状态时，您的组织将自动授予许可证的自由，限制使用订阅 Azure 权限管理。 此免费订阅仅限于进行加密和解密企业设置和应用程序数据同步由企业状态漫游服务;您必须付费的订阅使用 Azure 权限管理的全部功能。

之后获得特优 Azure 广告订阅，请按照以下步骤启用企业状态漫游︰

1. 登录到 Azure 的传统门户网站。
2. 在左边，选择**有效的目录**，目录，然后选择您要为其启用企业状态漫游。
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. 转到**配置**选项卡上的顶部。
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  页面向下滚动并选择**用户可能同步设置和企业应用程序数据**，然后单击**保存**。
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

对于 Windows 10 漫游设置与企业状态漫游服务设备，设备必须使用 Azure AD 身份验证身份。 对于加入到 Azure 的 AD 的设备，该用户的主登录是 Azure 的广告标识，因此不不需要任何额外的配置。 对于使用传统的内部部署 Active Directory 的设备，IT 管理员必须[连接到 Windows 10 经验的 Azure AD 的加入域的设备](active-directory-azureadjoin-devices-group-policy.md)。

## <a name="sync-data-storage"></a>同步数据存储
企业状态漫游数据位于最相符的 Azure Active Directory 实例中设置的国家/地区值与一个或多个[Azure 的区域](https://azure.microsoft.com/regions/ )中。 企业状态漫游数据分区基于三个主要地理区域︰ 北美、 EMEA 和 APAC。 企业状态漫游租户的数据位于本地使用的地理区域和区域间不会对其进行复制。  例如，其国家/地区值设置为 EMEA 国家之一的客户喜欢"华北"或者"赞比亚"将具有其数据放在一个或在欧洲内的 Azure 地区。  "美国"或"加拿大"将具有其数据驻留在一个或多个 Azure 区域内美国正确设置其国家/地区值为北美国家之一的 Azure AD 中的客户。  "澳大利亚"或"新西兰"将具有其数据驻留在一个或多个在亚洲区域内的 Azure 正确设置其国家/地区值在 APAC 国家之一的 Azure 广告客户。  南美国家和南极数据将在美国境内的一个或多个 Azure 地区承载。  国家/地区值设置 Azure 的广告目录创建过程的一部分，以后不能修改。 

如果您需要更多数据存储位置的详细信息，请文件使用[Azure 支持](https://azure.microsoft.com/support/options/)票。

## <a name="manage-enterprise-state-roaming"></a>管理企业状态漫游
Azure AD 全局管理员可以启用和禁用 Azure 的经典门户中的企业状态漫游。
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

全局管理员可以限制到特定安全组的设置同步。

全局管理员还可以通过在 Active Directory 实例**的用户**列表中选择特定的用户和**设备**选项卡上单击并选择视图**同步设置和企业应用程序数据的设备**查看每个用户设备同步状态报告。
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>数据保留
数据同步到 Azure 通过企业状态漫游将会无限期保留，除非执行手动删除操作或确定问题的数据是陈旧。 

**显式删除︰**Azure 管理员删除一个用户或一个目录或管理员显式请求，数据会被删除时，已删除的数据。

- **删除用户**︰ 漫游数据的用户帐户在 Azure AD 中删除用户时，将标记为删除，并且将被删除之间 90 到 180 天。 
- **目录删除**︰ Azure AD 中删除整个目录是直接操作。 所有设置数据都关联的目录将标记为删除，并且将被删除之间 90 到 180 天。 
- **请求删除**︰ 如果 Azure AD 管理员想要手动删除特定用户的数据或设置数据，管理员可以文件使用[Azure 支持](https://azure.microsoft.com/support/)票。 

**删除陈旧数据**︰ 一年 （"保留期"） 将会被视为陈旧的可能从 Azure 中删除未访问的数据。 保留期如有更改，但不是会少于 90 天。 陈旧的数据可能是一组特定的 Windows/应用程序设置或所有用户的设置。 例如︰
 
- 如果没有设备访问一个特定的设置集合 （例如，应用程序将从该设备，删除或如"主题"设置组禁用所有用户的设备），则该集合保持期后将变得过时，可能会被删除。 
- 如果用户关闭了其所有的设备上的设置同步，然后将访问无设置数据，并且该用户的所有设置数据将变得陈旧，保留期过后可能会被删除。 
- 如果 Azure 的广告目录管理员关闭企业状态漫游的整个目录，然后所有的用户，目录将停止同步设置，并设置所有用户的所有数据将变得陈旧，保留期过后可能会被删除。 

**恢复已删除数据**︰ 是不可配置的数据保留策略。 一旦数据已被永久删除，则它将无法恢复。 但是，它是一定要注意从 Azure，不该最终用户设备将仅删除设置数据。 如果任何设备以后重新连接到企业状态漫游服务时，将再次同步并在 Azure 存储设置。


## <a name="related-topics"></a>相关的主题
- [企业状态漫游概述](active-directory-windows-enterprise-state-roaming-overview.md)
- [设置和数据漫游的常见问题解答](active-directory-windows-enterprise-state-roaming-faqs.md)
- [组策略和每台 MDM 设置设置同步](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10 漫游设置参考](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
