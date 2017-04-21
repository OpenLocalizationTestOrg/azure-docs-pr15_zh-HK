<properties 
    pageTitle="教程︰ Azure Active Directory 集成基础需用 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 基石按需启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>教程︰ 与基础按需的 Azure Active Directory 集成

本教程的目的是显示 Azure 和基石按需的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   按需基础租户

学完本教程后, 已分配给基础需 Azure AD 用户将能够到基石需公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为基础按需
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "方案")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>启用应用程序集成为基础按需

本部分的目的是概述如何启用应用程序集成为基础按需。

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>若要启用应用程序集成为基础按需，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**基础位置**。

    ![应用程序库](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "应用程序库")

7.  在结果窗格中，选择**基础的位置**，，然后单击**完成**添加应用程序。

    ![按需基础](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "按需基础")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到基石按需。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**基础位置**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![启用单一登录](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "启用单一登录")

2.  在**您想怎样为基础按需登录的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![Microsoft Azure 广告单登录](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure 广告单登录")

3.  在**配置应用程序 URL**页上，在**基础位置号在 URL**文本框中，键入您的 URL 使用以下模式"*http://company.csod.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "配置应用程序的 URL")

4.  在**配置单一登录基石需在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "配置单一登录")

5.  将发送以下各项为基础按需支持团队︰

    1.  下载的证书
    2.  **远程登录 URL**值
    3.  **远程注销 URL**的值。

    >[AZURE.NOTE] 单一登录需要配置基础按需支持小组。
配置完成后，将从支持团队获得通知。

6.  选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到基石需，他们必须为基础按需调配。  
对于基础需配备是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  发送的信息 (例如︰ 名称、 Emial) 有关要为基础按需设置 Azure AD 用户支持小组。

>[AZURE.NOTE] 您可以使用任何其他基础需用户帐户创建工具或 Api 提供的基础按需调配 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>若要将用户分配到基础位置，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**基础位置**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![分配用户](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "分配用户")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
