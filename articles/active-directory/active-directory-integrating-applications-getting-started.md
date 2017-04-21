<properties
   pageTitle="将 Azure Active Directory 集成与应用程序获取启动指南 | Microsoft Azure"
   description="这篇文章是一个获取入门的指南 Azure 活动目录 (AD) 与内部应用程序和云应用程序集成在一起。"
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>将 Azure Active Directory 集成与应用程序获取启动指南
## <a name="overview"></a>概述
本主题旨在为您提供集成使用 Azure 活动目录 (AD) 的应用程序的一个路线图。 以下各节中的每个包含更详细主题的简短摘要，以便您可以确定此获取指南中的哪些部分会与您相关。  按照每个主题的链接，获取更深入的了解。

## <a name="before-you-begin-take-inventory"></a>在开始之前，清点
您在跳转到 Azure 广告与集成的应用程序之前，请务必知道您身在何处，您想要。  以下问题旨在帮助您考虑广告 Azure 应用程序集成项目。

### <a name="application-inventory"></a>应用程序清单
- 在哪里所有的应用程序？ 谁拥有它们？
- 您的应用程序是否需要哪种类型的身份验证？
- 谁需要访问哪些应用程序？
- 若要部署新的应用程序吗？
  - 将生成内部并将其部署在 Azure 计算实例？
  - 您将使用一个在 Azure 应用程序库中的可用？

### <a name="user-and-group-inventory"></a>用户和组的库存
- 您的用户帐户驻留在什么位置？
 - 内部部署 Active Directory
 - Azure 的广告
 - 在您拥有单独的应用数据库
 - 在未批准的应用程序
 - 以上各项
- 哪种权限和角色分配单个用户当前有？ 您需要查看他们的访问权限，或确实，现在用户访问权限和角色分配是适当？
- 组已建立内部活动目录中？
 - 您的组是如何组织的？
 - 该组的成员是谁？
 - 哪种权限/角色分配组目前有？
- 您需要集成前清理用户/组数据库？  （这是一个非常重要的问题。 垃圾中，垃圾出。）

### <a name="access-management-inventory"></a>访问管理库存
- 您如何当前管理对应用程序的用户访问？ 它是否需要更改？  您是否考虑过其他的方法来管理访问，如与[RBAC](role-based-access-control-configure.md)例如？
- 谁需要什么权？

也许不预先有所有这些问题的答案，但是没有关系。  本指南可以帮助您回答一些问题，并作出某些明智的决策。

## <a name="prerequisites"></a>系统必备组件
- Azure 的订阅和 Azure Active Directory 目录。  如果您尚没有 Azure 的订阅，可以尝试免费为 30 天的 Azure。 [试一下 ！](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>与 Azure AD 的应用程序集成
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>查找未批准云与云应用程序查询的应用程序
正如前面提到的可能还没有已经被组织迄今为止所管理的应用程序。  作为清单进程的一部分，则可能要查找未批准的云应用程序。 请参阅[查找未批准的云与云应用程序查询的应用程序](active-directory-cloudappdiscovery-whatis.md)。

### <a name="authentication-types"></a>身份验证类型
每个应用程序可能有不同的身份验证要求。 Azure 的广告，与签名证书可以使用的应用程序使用 SAML 2.0、 WS 联合身份验证或 OpenID 连接协议以及单一登录密码进行。 有关应用程序的详细信息[密码基于单一登录](active-directory-appssoaccess-whatis.md)和[管理联合单一登录的 Azure Active Directory 中的证书](active-directory-sso-certs.md)，请参阅使用 Azure AD 身份验证类型。

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>启用 SSO 使用 Azure AD 的应用程序代理
与 Microsoft Azure AD 应用程序代理服务器，可以提供对应用程序位于您的专用网络内部安全，任何地方和任何设备上访问。 在您的环境中安装应用程序代理服务器连接器后，它可以很容易地配置 Azure 的广告。

### <a name="integrating-applications-with-azure-ad"></a>对于 Azure AD 集成应用程序
下面的文章讨论了不同的方式应用程序集成使用 Azure 的广告，并提供一些指导。

- [确定要使用哪个活动目录](active-directory-administer.md)
- [在 Azure 应用程序库中使用的应用程序](active-directory-appssoaccess-whatis.md)
- [将集成 SaaS 应用程序教程列表](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>对应用程序的管理访问
下列文章介绍集成使用 Azure 广告使用 Azure AD 连接器和 Azure 的广告后，您可以管理访问应用程序的方式。

- [管理应用程序使用 Azure AD 的访问](active-directory-managing-access-to-apps.md)
- [自动化与 Azure AD 连接器](active-directory-saas-app-provisioning.md)
- [将用户分配到应用程序](active-directory-applications-guiding-developers-assigning-users.md)
- [将组分配到应用程序](active-directory-applications-guiding-developers-assigning-groups.md)
- [共享帐户](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>将自定义应用程序集成
如果您正在编写新的应用程序，并且想要帮助开发人员利用 Azure 广告的威力，看到[Guiding 的开发人员](active-directory-applications-guiding-developers-for-lob-applications.md)。

如果您想要添加自定义应用程序到 Azure 应用程序库，请参阅[使用 Azure AD 自助式 SAML 配置的"携带您自己的应用程序"](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)。

## <a name="see-also"></a>请参见

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
