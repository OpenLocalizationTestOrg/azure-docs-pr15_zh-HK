<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Evidence.com |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Evidence.com 之间。"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>教程︰ 与 Evidence.com 的 Azure Active Directory 集成

本教程的目的是显示如何将 Azure 活动目录 (AAD) 和 Evidence.com 之间的单一登录设置。 在本教程中介绍的方案假定您已经有以下各项︰
    
* 有效的 Microsoft Azure 订阅
* 启用单一登录与 Evidence.com 订阅 (电子邮件earlyaccess@evidence.com如果没有启用基于 SAML 的单一登录)

学完本教程后, 分配 Evidence.com 访问 AAD 用户将能够单一登录到应用程序中使用 AAD 访问权限面板。

## <a name="add-evidencecom-to-your-directory"></a>将 Evidence.com 添加到您的目录

本节概述了如何将 Evidence.com 添加为在 Azure Active Directory 集成的应用程序。

**若要启用的证据的应用程序集成︰**

1.  在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

4.  若要打开应用程序库，请单击**添加**，然后单击**添加应用程序从库**。

5.  在搜索框中，键入**Evidence.com**。

6.  在结果窗格中，选择**Evidence.com**，，然后单击**完成**添加应用程序。


## <a name="configuring-single-sign-on"></a>配置单一登录

这一节概述如何使用户能够与他们 Azure Active Directory，使用联合身份验证基于 SAML 协议中的帐户验证到 Evidence.com。

**若要配置单一登录，请执行以下步骤︰**

1.  在 Azure 的传统门户网站中添加 Evidence.com 后, 单击**配置单一登录**。 
 
2.  在下一个屏幕上**Azure AD Single Sign-on**，选择，然后单击**下一步**。

3.  在配置应用程序 URL 屏幕中，输入的 URL，用户将登录到 Evidence.com 租户 URL (示例︰ https://yourtenant.evidence.com，然后单击**下一步**。 

4.  单击**下载证书**链接，并将其保存到本地驱动器。 将使用此证书和元数据 Url （实体 ID、 SSO URL 中的符号和标志出 URL） 在 Evidence.com 网站上设置 SSO。 

5.  在单独的 web 浏览器窗口中，登录到您的 Evidence.com 租户以管理员的身份，然后定位到**Admin**选项卡
      
6.  单击**机构单一登录**
 
7.  选择**SAML 基于单一登录**
 
8.  复制**颁发者 URL**，**单一登录**和**单个退出**值显示在 Azure 的传统门户网站，到 Evidence.com 中的相应字段。

9.  打开证书在步骤 4 使用文本编辑器，如 Notepad.exe，下载和内容复制并粘贴到**安全证书**框。 

10. 在 Evidence.com 中保存配置。
 
11. 在 Azure 的经典门户，检查**已配置单一登录如上所述的确认**。 检查这将使当前的证书来开始使用此应用程序的复选框。
 
12. 在单一登录确认页上，单击**完成**。  


## <a name="creating-an-evidencecom-test-user"></a>创建一个 Evidence.com 的测试用户

Azure AD 用户能够登录，他们必须为 Evidence.com 应用程序内访问设置。 本部分介绍如何创建在 Evidence.com Azure AD 用户帐户

**若要设置 Evidence.com 中的用户帐户︰**

1.  在 web 浏览器窗口中，以管理员身份登录到 Evidence.com 公司网站。

2.  导航到**Admin**选项卡。

3.  单击**添加用户**。

4.  单击**添加**按钮。

5.  添加的用户**的电子邮件地址**必须匹配在 Azure AD 您想要授予访问权限的用户的用户名。 如果用户名和电子邮件地址不是您的组织中相同的值，则可以使用**Evidence.com > 属性 > 单一登录**Azure 的传统门户网站，若要将更改发送到 Evidence.com 的电子邮件地址 nameidenitifer 部分。


## <a name="assigning-users-to-evidencecom"></a>将用户分配到 Evidence.com

调配 AAD 用户能够查看其接入面板上的 Evidence.com，他们必须将分配在 Azure 的传统门户网站的访问。

**若要将用户分配到 Evidence.com:**

1.  在 Azure 的经典门户中 Evidence.com 的快速起始页，单击**分配到 Evidence.com 的用户**。
 
2.  在**显示**菜单中，选择是否要将用户或组分配给 Evidence.com，然后单击复选标记按钮。
 
3.  在**用户**列表中，选中用户分组到您想要分配 Evidence.com 与谁。
 
4.  在页面页脚中，请单击**分配**按钮。

