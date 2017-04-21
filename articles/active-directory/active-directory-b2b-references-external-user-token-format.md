<properties
   pageTitle="外部用户令牌 Azure 活动目录 B2B 协作预览格式 |Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Azure AD B2B 协作预览︰ 外部用户令牌格式

声明为标准的 Azure AD 标记在描述[支持令牌和声明类型](active-directory-token-and-claims.md)文章 azure.microsoft.com。

经过身份验证的 B2B 协作外部用户来说是不同的声明如下所示︰<br/>
- **OID:**从资源租户的对象 ID<br/>
- **TID**︰ 从资源租户租户 ID<br/>
- **颁发者**︰ 这是资源组织<br/>
- **IDP**︰ 这是家庭组织的用户<br/>
- **AltSecId**︰ 这是对您是不透明的备选安全 ID<br/>

## <a name="related-articles"></a>相关的文章
浏览我们 Azure AD B2B 协作的其他文章︰

- [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
- [它的工作原理](active-directory-b2b-how-it-works.md)
- [详细的演练](active-directory-b2b-detailed-walkthrough.md)
- [CSV 文件格式参考](active-directory-b2b-references-csv-file-format.md)
- [外部用户对象属性的更改](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [当前预览限制](active-directory-b2b-current-preview-limitations.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
