<properties 
    pageTitle="教程︰ Azure Active Directory 集成与新 Relic |Microsoft Azure" 
    description="了解如何使用新的 Relic Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>教程︰ 与新的 Relic 的 Azure Active Directory 集成
  
本教程的目的是显示如何将 Azure Active Directory 和新 Relic 之间的单一登录设置。
  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   新的 Relic-启用单一登录的订阅
  
后学完本教程，您已分配给新的 Relic Azure Active Directory 用户将能够为单一登录使用 AAD 访问面板。

1.  启用应用程序集成的新 Relic
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-new-relic-tutorial/IC797030.png "方案")
##<a name="enabling-the-application-integration-for-new-relic"></a>启用应用程序集成的新 Relic
  
本部分的目的是概述如何启用新的 Relic 应用程序的集成。

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>若要启用应用程序集成的新 Relic，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-new-relic-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-new-relic-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-new-relic-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-new-relic-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**新的 Relic**。

    ![应用程序库](./media/active-directory-saas-new-relic-tutorial/IC797031.png "应用程序库")

7.  在结果窗格中，选择**新的 Relic**，，然后单击**完成**添加应用程序。

    ![新 Relic](./media/active-directory-saas-new-relic-tutorial/IC797032.png "新 Relic")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
这一节概述如何使用户能够与他们 Azure Active Directory，使用联合身份验证基于 SAML 协议中的帐户验证到新的 Relic。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**新的 Relic**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-new-relic-tutorial/IC769534.png "配置单一登录")

2.  在**您想怎样来登录到新的 Relic 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-new-relic-tutorial/IC797033.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**新 Relic 号上的 URL**文本框中，键入用户用于登录到新的 Relic 应用程序的 URL，然后单击**下一步**。 

    应用程序的 URL 是您新的 Relic 的租户 URL (例如︰ *https://rpm.newrelic.com*):

    ![配置应用程序的 URL](./media/active-directory-saas-new-relic-tutorial/IC797034.png "配置应用程序的 URL")

4.  在**配置单一登录新 Relic 在**页上，下载您的证书，单击**下载证书**，然后将证书文件本地保存到您的计算机。

    ![配置单一登录](./media/active-directory-saas-new-relic-tutorial/IC797035.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，登录到您的**新的 Relic**公司网站以管理员的身份。

6.  在菜单上，单击**帐户设置**。

    ![帐户设置](./media/active-directory-saas-new-relic-tutorial/IC797036.png "帐户设置")

7.  单击**安全和身份验证**选项卡，然后单击**单一签入**选项卡。

    ![单一登录](./media/active-directory-saas-new-relic-tutorial/IC797037.png "单一登录")

8.  在 SAML 对话框页面上，请执行以下步骤︰

    ![SAML](./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  单击**选择文件**以上载您已下载的 Azure Active Directory 证书。
    2.  在 Azure 经典门户中，**配置单一登录新 Relic 在**页上复制的**远程登录 URL**值，，然后将其粘贴到**远程登录 URL**文本框。
    3.  在 Azure 经典门户中，**配置单一登录新 Relic 在**页上复制的**远程注销 URL**值，然后将其粘贴到**注销登录 URL**文本框。
    4.  单击**保存我的更改**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-new-relic-tutorial/IC797039.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure Active Directory 用户能够登录到新的 Relic，他们必须到新的 Relic 设置。  
在新的 Relic，资源调配是手动任务。

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>若要设置到新的 Relic 的用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**新的 Relic**公司网站。

2.  在菜单上，单击**帐户设置**。

    ![帐户设置](./media/active-directory-saas-new-relic-tutorial/IC797040.png "帐户设置")

3.  在左侧的**帐户**窗格中，单击**摘要**，然后单击**添加用户**。

    ![帐户设置](./media/active-directory-saas-new-relic-tutorial/IC797041.png "帐户设置")

4.  在**活动用户**对话框中，请执行以下步骤︰

    ![活动用户](./media/active-directory-saas-new-relic-tutorial/IC797042.png "活动用户")

    1.  在**电子邮件**文本框中，键入您要提供有效的 Azure Active Directory 用户的电子邮件地址。
    2.  为**角色**选择**用户**。
    3.  单击**添加该用户**。

>[AZURE.NOTE]您可以使用任何其他新的 Relic 用户帐户创建工具或 Api 提供的新的 Relic 来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>若要将用户分配到新的 Relic，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**新的 Relic**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-new-relic-tutorial/IC797043.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-new-relic-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。




