<properties 
    pageTitle="教程︰ Azure Active Directory 集成指导 Microsoft |Microsoft Azure" 
    description="了解如何使用 Microsoft 的 Azure Active Directory 方向启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>与 Microsoft 的指导教程︰ Azure Active Directory 集成

本教程的目的是要显示在 Microsoft 的 Azure Active Directory 和方向的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   有关 Microsoft 订购

如果 Microsoft 订购还没有联合的指导，通过电子邮件发送到的请求"*service@DirectionsOnMicrosoft.com*"。

后学完本教程，您已分配给 Microsoft 的指导的 Azure Active Directory 用户将能够到单一登录到应用程序中使用单一登录方式进行登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Microsoft 的指导
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "方案")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>启用应用程序集成为 Microsoft 的指导

本部分的目的是概述如何启用应用程序集成为 Microsoft 的指导。

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>若要启用应用程序集成为 Microsoft 的指导，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**有关 Microsoft 的方向**。

    ![应用程序库](./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "应用程序库")

7.  在结果窗格中，选择**Microsoft 的指导**，然后单击**完成**添加应用程序。

    ![方案](./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "方案")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Microsoft 的指导。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**有关 Microsoft**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![启用单一登录](./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "启用单一登录")

2.  **您想怎样用户能够登录到有关 Microsoft 如何**在页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![Microsoft Azure AD Singel 登录](./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel 登录")

3.  在**配置应用程序 URL**页上，在符号上 URL 文本框中，键入**https://www.directionsonmicrosoft.com/user/login**，，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "配置应用程序的 URL")

4.  在**配置单一登录有关 Microsoft 如何在**页上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "配置单一登录")

5.  将元数据文件发送给 Microsoft 技术支持人员的指导 (*service@DirectionsOnMicrosoft.com*)。 要启用 Microsoft 支持小组上的提示，以找到您的站点联盟成员身份，请包括您的电子邮件中您公司的信息。

    >[AZURE.NOTE] Microsoft 的指导为单一登录需要启用由 Microsoft 技术支持人员的指导。
已启用单一登录时，您将收到通知。

6.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

没有要将资源调配给 Microsoft 的指导用户配置操作项。  
当分派的用户试图登录到 Microsoft 使用接入面板的指导时，Microsoft 有关如何检查用户是否存在。 如果没有用户帐户可还，它是自动创建 Microsoft 的指导。
##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>若要将用户分配给 Microsoft 的指导，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  **说明在 Microsoft**应用程序集成在页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "是")
