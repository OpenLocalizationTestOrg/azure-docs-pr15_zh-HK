<properties
    pageTitle="Azure 的活动目录︰ 创建租户支持主题 |Microsoft Azure"
    description="创建 Azure Active Directory 租户或 Azure 活动目录 B2C 租户︰ 问题和解决方法"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="creating-an-azure-active-directory-azure-ad-tenant-or-azure-ad-b2c-tenant-issues-and-resolutions"></a>创建 Azure 活动目录 (AD Azure) 租户或 Azure AD B2C 租户︰ 问题和解决方法

## <a name="creating-an-azure-ad-tenant"></a>创建 Azure AD 租户

如果您不能创建 Azure AD 租户第一次，请再试一次。 如果问题仍然存在，请联系支持。

## <a name="creating-an-azure-ad-b2c-tenant"></a>创建 Azure AD B2C 租户

如果您在[Azure AD B2C 租户的创建](active-directory-b2c-get-started.md)过程中遇到问题，请尝试以下方法︰
 
- 如果 Azure AD B2C 租户不会显示在列表中的承租人，请再试一次。
- 如果 Azure AD B2C 租户承租人的列表中显示，并且您会收到一条错误消息 （"无法完成的 B2C 租户 contosob2c 的创建。 请访问以下[链接](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409)获得更多的指导。"），删除刚创建的组织，然后再试。
- 请注意，存在一些已知问题删除现有 B2C 租户和重新使用相同的域名创建它时。 您必须使用不同的域名创建 B2C 租户。
- 如果这些解决方法不适合您，请联系支持。 了解更多有关[如何 Azure AD B2C 的文件的支持请求](active-directory-b2c-support.md)。
