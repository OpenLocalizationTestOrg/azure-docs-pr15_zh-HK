<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Overdrive 丛书 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Overdrive 簿启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>教程︰ 与 Overdrive 书籍的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 OverDrive 集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   OverDrive-启用单一登录的订阅
  
后学完本教程，您已分配给 OverDrive Azure AD 用户将能够到 OverDrive 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的 OverDrive
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "方案")
##<a name="enabling-the-application-integration-for-overdrive"></a>启用应用程序集成的 OverDrive
  
本部分的目的是概述如何启用应用程序集成的 OverDrive。

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>若要启用应用程序集成的 OverDrive，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**OverDrive**。

    ![应用程序库](./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "应用程序库")

7.  在结果窗格中，选择**OverDrive**，，然后单击**完成**添加应用程序。

    ![OverDrive](./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 OverDrive。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**OverDrive**集成应用程序页，单击**配置单一登录****配置单一登录**对话框打开。

    ![启用单一登录](./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "启用单一登录")

2.  **如何将用户注册到 OverDrive 要**在页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**OverDrive 号在 URL**文本框中，键入您的 URL 使用以下模式"*http://mslibrarytest.libraryreserve.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "配置应用程序的 URL")

4.  在**配置单一登录 OverDrive 在**页上，并将下载元数据文件中，然后将其发送给 OverDrive 支持小组。

    ![配置单一登录](./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "配置单一登录")

    >[AZURE.NOTE]OverDrive 支持团队将为您的单一登录配置和配置完成后向您发送通知。

5.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
没有为您配置用户的资源调配给 OverDrive 操作项。  
当分派的用户试图登录到 OverDrive 时，如有必要，将自动创建 OverDrive 帐户。

>[AZURE.NOTE]您可以使用任何其他 OverDrive 用户帐户创建工具或 Api 提供的 OverDrive 来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>若要将用户分配到 OverDrive，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**OverDrive**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。