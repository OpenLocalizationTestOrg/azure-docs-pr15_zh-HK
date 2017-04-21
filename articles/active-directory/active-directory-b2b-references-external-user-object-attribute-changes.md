<properties
   pageTitle="外部用户对象属性更改的 Azure 活动目录 B2B 协作预览 |Microsoft Azure"
   description="Azure 的活动目录 B2B 支持您跨公司的关系通过有选择地访问企业应用程序的业务合作伙伴"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure AD B2B 协作预览︰ 外部用户对象属性的更改

用户对象表示在 Azure 的广告目录中每个用户。 在 Azure 广告中的用户对象经历不同的特性变化的 B2B 协作阶段邀请兑换流。 用户对象表示目录中合作伙伴的用户已在更改的属性兑现的时间，当合作伙伴用户单击邀请电子邮件中的链接。 具体来说︰

- **SignInName**和**AltSecId**属性进行填充
- **显示名称**属性中更改与用户主体名称(user_fabrikam.com#EXT#@contoso.com)为登录名(user@fabrikam.com)

在 Azure AD 中跟踪这些特性可以帮助您解决伙伴用户已兑现其 B2B 协作邀请。

## <a name="related-articles"></a>相关的文章
浏览我们 Azure AD B2B 协作的其他文章︰

- [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
- [它的工作原理](active-directory-b2b-how-it-works.md)
- [详细的演练](active-directory-b2b-detailed-walkthrough.md)
- [CSV 文件格式参考](active-directory-b2b-references-csv-file-format.md)
- [外部用户令牌格式](active-directory-b2b-references-external-user-token-format.md)
- [当前预览限制](active-directory-b2b-current-preview-limitations.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
