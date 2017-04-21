<properties
   pageTitle="在 Azure Active Directory 管理单位管理"
   description="使用更精细的 Azure Active Directory 中的权限委派的管理单位"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>管理单元管理 Azure 做广告的公共预览

本文介绍了管理单位--新的 Azure 的 Active Directory 容器可用于委派管理权限的用户和应用到的用户的子集策略子集上的资源。 在 Azure Active Directory 管理单元启用中心管理员委派区域管理员的权限，或在某一粒度级别设置策略。

这是具有独立部门，例如，大型大学所组成的许多自治学校 （商业学校、 工程学校等） 彼此之间的相互独立的组织中有用。 这种划分具有自己 IT 管理员对控制访问、 管理用户，并设置专门针对其部门的策略。 中心管理员希望能授予这些部门对其特定部门中用户的管理员权限。 更具体地说，使用本示例，中心管理员可以为实例，创建特定学校 （商学院） 的一个管理单元，填充业务的学校用户。 中心管理员在商学院的 IT 员工可以添加一个指定了作用域的角色，换句话说，准予商业学校管理权限的 IT 人员只能通过商业学校的管理单元。

> [AZURE.IMPORTANT]
> 您可以创建和使用管理的单位，仅当您启用 Azure 活动目录津贴。 有关详细信息，请参阅[入门 Azure AD 津贴](active-directory-get-started-premium.md)。

从中心管理员的角度来看，一个管理单元是可以创建并填充其资源的目录对象。 **在此版本中，这些资源可以是仅用户。** 创建并填充后，管理部门可为范围只能通过管理单元中包含的资源限制授予的权限。

## <a name="managing-administrative-units"></a>管理管理单元

在此预览版本中，您可以创建和管理管理单元使用 Azure 活动目录模块用于 Windows PowerShell cmdlet。

有关软件要求和安装 Azure AD 模块中，详细信息和有关的 Azure AD 模块 cmdlet 管理单元，包括语法、 参数说明和示例，请参阅[管理 Azure 使用 Windows PowerShell 的广告](https://msdn.microsoft.com/library/azure/jj151815.aspx)。


## <a name="next-steps"></a>下一步行动
[Azure Active Directory 版本](active-directory-editions.md)
