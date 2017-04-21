<properties
   pageTitle="Azure AD B2B 协作预览︰ 它是如何工作 |Microsoft Azure"
   description="描述如何 Azure 活动目录 B2B 协作支持您跨公司的关系通过有选择地访问企业应用程序的业务合作伙伴"
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

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure AD B2B 协作预览︰ 它的工作原理
Azure AD B2B 协作基础邀请和兑换模型。 提供您想要使用，以及您想让他们使用的应用程序的各方的电子邮件地址。 Azure 广告发送电子邮件邀请中的链接。 合作伙伴用户追踪链接并提示使用其 Azure AD 帐户或登录登录运行新的 Azure AD 帐户。

1. 您的管理员邀请伙伴用户通过上传[一个结构化的.csv 文件](active-directory-b2b-references-csv-file-format.md)使用 Azure 的门户。
2. 此门户将发送邀请电子邮件发送给这些伙伴用户。
3. 合作伙伴用户单击电子邮件中的链接，并提示您要使用它们工作的凭据 （如果它们已在 Azure 的广告），或作为 Azure AD B2B 协作用户注册。
4. 合作伙伴的用户被重定向到他们曾获邀到，他们现在有权访问的应用程序。

## <a name="directory-operations"></a>目录操作
在 Azure 作为外部用户广告存在合作伙伴的用户。 这意味着您的管理员可以提供许可证、 指派组成员资格，并进一步授予通过 Azure 的门户网站或您的公司中的用户就像使用 Azure PowerShell 的企业应用程序的访问权限。

同时付费的 Azure 广告订阅 （基本或特优） 不需要使用 Azure AD B2B，是否已经付费的承租人 Azure 广告订阅 （基本或特优） 收到以下的额外优点︰

 - 管理员可以将组分配给应用程序，提供更简单的管理，被邀请的用户访问。
 - 品牌管理租户用于品牌邀请电子邮件和兑换经验，提供了更多的上下文邀请伙伴用户。

## <a name="related-articles"></a>相关的文章
 浏览我们 Azure AD B2B 协作的其他文章

 - [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [详细的演练](active-directory-b2b-detailed-walkthrough.md)
 - [CSV 文件格式参考](active-directory-b2b-references-csv-file-format.md)
 - [外部用户令牌格式](active-directory-b2b-references-external-user-token-format.md)
 - [外部用户对象属性的更改](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [当前预览限制](active-directory-b2b-current-preview-limitations.md)
 - [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
