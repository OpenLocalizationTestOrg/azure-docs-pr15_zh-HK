<properties
    pageTitle="Azure AD 连接在 Microsoft 云德国"
    description="Azure AD 连接将与 Azure Active Directory 集成您的内部目录。 这使您可以为 Office 365、 Azure，SaaS 应用程序集成在一起 Azure 广告提供公用标识。"
    keywords="介绍到 Azure AD 连接，Azure AD 连接概述，什么是 Azure AD 连接，安装 active directory，德国黑林"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD 连接在 Microsoft 云德国-公共预览

## <a name="introduction"></a>介绍
Azure AD 连接提供内部部署 Active Directory 和 Azure Active Directory 之间的同步。
目前，很多[Microsoft 云](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx)德国的方案必须由运营商完成。 当使用 Microsoft 云德国，您必须了解下列情况︰


- 必须对代理服务器进行同步，要成功进行打开下面的 Url:
    - *。 microsoftonline.de
    - *。 windows.net
    - + 证书吊销列表

- 当您登录到 Azure 的广告目录时，您必须在 onmicrosoft.de 域中使用的帐户。
- 不可用的下列功能︰
    - Azure AD 连接的运行状况
    - 自动更新
    - 密码写回

## <a name="download"></a>下载
您可以从门户中 Azure AD 连接刀片式服务器下载 Azure AD 连接。  使用下面的说明来查找 Azure AD 连接刀片式服务器。

### <a name="the-azure-ad-connect-blade"></a>Azure AD 连接刀片式服务器

一旦您已登录到 Azure 的门户，请执行以下操作︰

1. 转到浏览
2.  选择 Azure 的活动目录
3.  然后选择 Azure AD 连接

您应该看到以下项目︰

![Azure AD 连接刀片式服务器](media\active-directory-aadconnect-germany\germany1.png)

 
下表描述了显示刀片式服务器中的功能。


标题|说明|
----- | ----- |
同步状态|让您知道是否启用或禁用同步。|
上次同步|上一次同步成功完成。|
联盟的域|显示当前配置的联盟域的数目。|


## <a name="installation"></a>安装
若要安装 Azure AD 连接，您可以使用的文档[在此处](active-directory-aadconnect.md#install-azure-ad-connect)。

## <a name="advanced-features-and-additional-information"></a>高级的功能和附加信息
有关其他信息和自定义设置或高级的配置的指导，开始了[集成与 Azure Active Directory 内部标识](active-directory-aadconnect.md)。  此页提供的更多指导信息和链接。
