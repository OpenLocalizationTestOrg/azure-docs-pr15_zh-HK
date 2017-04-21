<properties
   pageTitle="当前预览限制 Azure 活动目录 B2B 协作 |Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure AD B2B 协作预览︰ 当前预览限制

- 多因素身份验证 (MFA) 不支持外部用户。 例如，如果 Contoso 有 MFA，但合作伙伴组织没有，然后合作伙伴组织的用户无法授予 MFA 通过 B2B 协作。
- 邀请是可能只能通过 CSV;不支持单独的邀请和 API 访问。
- 只有 Azure AD 全局管理员可以将.csv 文件上载。
- 当前不支持对使用者的电子邮件地址 （如 hotmail.com，Gmail.com 或 comcast.net） 的邀请。
- 外部用户访问内部部署的应用程序进行测试。
- 外部用户不会自动清理时实际用户从其目录中删除。
- 不支持到通讯组列表的邀请。
- 可通过 CSV 上载最大为 2000 条记录。

## <a name="related-articles"></a>相关的文章
浏览我们 Azure AD B2B 协作的其他文章︰

- [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
- [它的工作原理](active-directory-b2b-how-it-works.md)
- [详细的演练](active-directory-b2b-detailed-walkthrough.md)
- [CSV 文件格式参考](active-directory-b2b-references-csv-file-format.md)
- [外部用户令牌格式](active-directory-b2b-references-external-user-token-format.md)
- [外部用户对象属性的更改](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
