<properties
   pageTitle="对 Azure Active Directory 添加应用程序的方式。"
   description="本文介绍了如何将应用程序添加到 Azure Active Directory 的实例。"
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>如何以及为什么将应用程序添加到 Azure 的广告

在 Azure Active directory 实例中查看的应用程序列表时最初棘手的事情之一了解应用程序的来源，以及为什么他们会出现。  这篇文章将提供如何应用程序目录中表示，为您提供上下文，可帮助您理解应用程序如何在您的目录中的高级别概述。

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Azure 广告为应用程序提供什么样的服务？

应用程序被添加到 Azure 的广告，利用一个或多个它所提供的服务。  这些服务包括︰

* 应用程序身份验证和授权
* 用户身份验证和授权
* 单一登录 (SSO) 使用联合身份验证或密码
* 用户设置和同步
* 基于角色的访问控制;使用目录来定义应用程序角色执行角色基于应用程序中的进行授权检查。
* oAuth 授权服务 （使用 Office 365 和其他 Microsoft 应用程序授权访问 Api/资源）。
* 应用程序发布和代理;发布到 internet 的应用程序从专用网络

## <a name="how-are-applications-represented-in-the-directory"></a>如何表示目录中应用程序？

在 Azure AD 使用 2 对象中表示的应用程序︰ 应用程序对象和服务的主要对象。  一个应用程序对象，在"主页"中注册 /"所有者"发布"目录和一个或更多服务主体对象表示的应用程序中每个目录，它的作用。  

应用程序对象描述对 Azure AD 的应用程序 （多租户服务） 和可能包括以下任何内容: (*注意*︰ 这不是一个详尽的列表。)

* 名称、 徽标和发布服务器
* 机密信息 （用来验证应用程序对称和/或非对称密钥）
* API 依赖项 (oAuth)
* Api/资源/范围发布 (oAuth)
* 应用程序角色 (RBAC)
* SSO 的元数据和配置 (SSO)
* 设置元数据和配置的用户
* 代理服务器的元数据和配置

服务主要是在每个目录，应用程序在其中作用包括其主目录的应用程序的记录。  服务主体︰

* 回指的是通过应用程序 id 属性的应用程序对象
* 记录本地用户和组应用程序角色分配
* 向应用程序授予记录本地用户和管理员权限
    * 例如︰ 若要访问特定用户的电子邮件应用程序的权限
* 记录本地策略包括条件访问策略
* 记录本地替代本地设置应用程序
    * 声明转换规则
    * 属性映射 （用户资源调配）
    * （如果该应用程序支持自定义角色） 租户特定的应用程序角色
    * 名称/徽标

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>应用程序对象和跨目录服务主体的关系图

![用图表演示应用程序对象和服务主体 Azure AD 实例与现有的方式。][apps_service_principals_directory]

您可以看到上面的关系图。  Microsoft 保留这两个目录用于发布应用程序内部 （在左侧）。

* 一个用于 Microsoft 应用程序 （Microsoft 服务目录）
* 一个用于预先集成第三方应用程序 （应用程序库目录）

应用程序发行商/供应商将与 Azure AD 集成都需要有一个发布目录。  （一些 SAAS 目录）。

将您自己添加的应用程序包括︰

* 应用程序开发 （与 AAD 集成）
* 应用程序连接的单点登录
* 发布的应用程序使用 Azure AD 应用程序代理服务器。

### <a name="a-couple-of-notes-and-exceptions"></a>几个说明和例外情况

* 不是所有的服务主体点返回到应用程序对象。  是吧？ Azure AD 最初生成时为应用程序提供的服务更多的限制，服务主体是不足以建立一个应用程序标识。  Windows 服务器的 Active Directory 服务帐户形状接近原始服务主体。  出于此原因的仍然可以创建无需首先创建应用程序对象使用 Azure AD PowerShell 的服务主体。  Graph API 创建服务主体之前要求的应用程序对象。
* 不是所有上述信息公开目前以编程方式。  以下是仅在用户界面中可用︰
    * 声明转换规则
    * 属性映射 （用户资源调配）
* 有关详细信息，在服务主体和 Azure 广告图形 REST API 参考文档，请参阅应用程序对象。  *提示*︰ Azure 广告图形 API 文档是 Azure 的广告，是目前仅次于架构引用。  
    * [应用程序](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [服务主体](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>如何将应用程序添加到我的 Azure AD 实例？
有多种方法可以将应用程序添加到 Azure 的广告︰

* 从[Azure 活动目录的应用程序库](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)中添加应用程序
* 符号向上/向第三方应用程序集成在一起 Azure Active Directory (例如︰ [Smartsheet](https://app.smartsheet.com/b/home)或[DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * 启动/用户在注册时需要提供应用程序的权限来访问其配置文件和其他权限。  第一个的人给予同意的情况下会导致服务主体表示该应用程序添加到该目录。
* 签署上/到 Microsoft [Office 365](http://products.office.com/)等的在线服务
    * 在 Office 365 提供订阅或开始表示用于提供的所有功能与 Office 365 提供的各种服务目录中创建一个或多个服务主体的试用版。
    * 某些 Office 365 提供服务，如 SharePoint 允许组件包括工作流之间的安全通信在日常基础上创建服务主体。
* 在 Azure 管理门户请添加您正在开发一个应用程序︰ https://msdn.microsoft.com/library/azure/dn132599.aspx
* 添加您正在使用 Visual Studio 开发一个应用程序，请参阅︰
    * [ASP.Net 的身份验证方法](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [连接的服务](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* 添加一个应用程序，用于使用[Azure AD 应用程序代理](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* 连接为使用 SAML 或密码 SSO 的单一登录应用程序
* 许多人在 Azure 中包括不同的开发人员体验并在 API 资源管理器遇到跨开发人员中心

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>谁有权向我 Azure AD 实例添加应用程序？

只有全局管理员可以︰

* 从 Azure AD 应用程序库 （预集成第三方应用程序） 添加应用程序
* 发布应用程序使用 Azure 的广告应用程序代理服务器

您的目录中的所有用户都有权添加他们正在开发的应用程序，并决定哪些应用程序相比它们共享/授予访问其组织的数据。  *记得上/中的用户登录到应用程序并授予权限时，可能会导致创建一个服务主体。*

此可能最初声音有关，但保留以下几点︰

* 已经能够利用多年的用户身份验证的 Windows 服务器的 Active Directory，而无需应用程序能够在目录中注册/记录的应用程序。  现在公司将改进对目录和 what for 多少应用程序使用完全可见性。
* 不是需要的驱动应用程序发布/注册过程的管理。  使用 Active Directory 联合身份验证服务，很可能，管理员必须将应用程序添加为依赖方代表开发人员。  现在，开发人员可以自助服务。
* 签名/由他们组织的帐户用于商业目的的应用程序中的用户是一件好事。  如果他们随后离开的组织将他们的帐户中它们所使用的应用程序中无法访问。
* 有记录的哪些数据共享的应用程序是一件好事。  数据是比以往任何时候更加便携，清晰记录的人员与哪个应用程序共享数据很有用。
* 使用 oAuth Azure AD 的应用程序决定完全何种权限，用户还可以向应用程序授予以及哪些需要同意拥有管理员权限。  它应该不必说只有管理员可以同意较大范围和更大的权限。
* 添加并允许应用程序访问他们的数据是的用户审核事件，以便您可以查看在 Azure 管理门户内审核报告，以确定如何为应用程序添加到目录。

**注意︰***有几个月现在使用默认配置已运行 Microsoft 本身。*

与所有的说，就可以阻止用户在您的目录中添加应用程序和方行使自己决定对哪些信息共享与应用程序通过修改 Azure 管理门户中的目录配置。  以下配置可以在目录的"配置"选项卡上的 Azure 管理门户中访问。

![配置集成应用程序设置的用户界面的屏幕抓图][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>下一步行动

了解有关如何添加到 Azure AD 的应用程序，以及如何配置应用程序的服务。

* [了解如何集成了 AAD 的应用程序](https://msdn.microsoft.com/library/azure/dn151122.aspx)的开发人员︰
* 开发人员︰[与 Azure Github 上的 Active Directory 集成在一起审查用于应用程序的示例代码](https://github.com/AzureADSamples)
* 开发人员和 IT 专业人员︰[审查 Azure 活动目录图形 API 的 REST API 文档](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* [了解如何使用从应用程序库 Azure Active Directory 预先集成的应用程序](https://msdn.microsoft.com/library/azure/dn308590.aspx)的 IT 专业人员︰
* [查找教程配置特定的预集成应用程序](https://msdn.microsoft.com/library/azure/dn893637.aspx)的 IT 专业人员︰
* [了解如何发布使用 Azure 的活动目录应用程序代理服务器应用程序](https://msdn.microsoft.com/library/azure/dn768219.aspx)的 IT 专业人员︰

## <a name="see-also"></a>请参见

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
