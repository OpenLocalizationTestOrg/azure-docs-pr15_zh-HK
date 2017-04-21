<properties
   pageTitle="Azure 的活动目录 B2B 协作 |Microsoft Azure"
   description="Azure 的活动目录 B2B 协作使业务合作伙伴能够访问企业应用程序，与每个由单个 Azure AD 用户帐户"
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

# <a name="azure-active-directory-b2b-collaboration"></a>Azure 的活动目录 B2B 协作

Azure 的活动目录 (AD Azure) B2B 协作可以从合作伙伴托管标识您公司的应用程序启用访问。 您可以创建跨公司的关系通过邀请和授权用户访问您的资源的合作伙伴公司。 因为每个公司使用 Azure 活动目录一次联盟，每个用户都由一个减少了复杂性 Azure AD 帐户。 如果您的业务合作伙伴在访问权限被吊销，当伙伴用户终止从其组织，并通过内部目录中的成员资格的意外的访问不能因为 Azure AD 中管理其帐户，安全性得到了提高。 对于尚不具备 Azure 广告的业务合作伙伴，B2B 协作有提供给业务合作伙伴的 Azure 的广告帐户的简化注册体验。

-   您的业务合作伙伴使用自己的登录凭据，它使您从外部合作伙伴目录，进行管理和移除访问权限，当用户离开伙伴组织的需要。

-   您对您的应用程序独立于您的业务伙伴的客户生命周期管理访问。 这意味着，例如，您可以不必让您的业务合作伙伴执行任何操作的 IT 部门撤销访问权限。

## <a name="capabilities"></a>功能

B2B 协作，简化了管理并提高合作伙伴访问公司资源包括诸如 Office 365、 销售队伍、 Azure 服务和每个移动设备、 云的 SaaS 应用程序和内部声明感知应用程序的安全性。 B2B 协作使合作伙伴管理他们自己的帐户和企业可以将安全策略应用于合作伙伴的访问。

即使他们没有通过电子邮件验证过程自己 Azure Active Directory，协作可以很容易地配置的 azure 活动目录 B2B 简化注册为各种规模的合作伙伴。 也很容易维护与任何外部目录或根据伙伴联合身份验证配置。

## <a name="b2b-collaboration-process"></a>B2B 的协作流程

1. Azure AD B2B 协作使公司管理员可以邀请，并授权一组外部用户通过向 B2B 协作门户上载的行不超过 2000 个逗号分隔值 (CSV) 文件。

  ![CSV 文件上载对话框](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. 门户将发送给这些外部用户的电子邮件邀请。

3. 所邀请的用户将在登录到现有的工作帐户与 Microsoft （托管在 Azure 的广告），或在 Azure AD 获取新工作帐户。

4. 登录之后，用户将重定向到与他们共享的应用程序。

当前不支持对使用者的电子邮件地址 （例如，Gmail 或[*comcast.net*](http://comcast.net/)） 的邀请。

B2B 协作工作原理的详细信息，查看[该视频](http://aka.ms/aadshowb2b)。

## <a name="next-steps"></a>下一步行动
浏览我们 Azure AD B2B 协作的其他文章。

- [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
- [它的工作原理](active-directory-b2b-how-it-works.md)
- [详细的演练](active-directory-b2b-detailed-walkthrough.md)
- [CSV 文件格式参考](active-directory-b2b-references-csv-file-format.md)
- [外部用户令牌格式](active-directory-b2b-references-external-user-token-format.md)
- [外部用户对象属性的更改](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [当前预览限制](active-directory-b2b-current-preview-limitations.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
