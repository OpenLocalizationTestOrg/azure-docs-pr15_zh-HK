<properties 
    pageTitle="教程︰ Azure Active Directory 集成电子生成器 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory e 生成器启用单一登录、 自动化资源调配，和更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>教程︰ 使用 e 生成器的 Azure 的 Active Directory 集成
  
本教程的目的是显示 Azure 和 e 生成器集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   E-生成器租户
  
后学完本教程，您已分配给 e 生成器 Azure AD 用户将能够到 e 生成器公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成电子生成器
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-e-builder-tutorial/IC777378.png "方案")
##<a name="enabling-the-application-integration-for-e-builder"></a>启用应用程序集成电子生成器
  
本部分的目的是概述如何启用应用程序集成电子生成器。

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>若要启用应用程序集成电子生成器，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-e-builder-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-e-builder-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-e-builder-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-e-builder-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**e 生成器**。

    ![应用程序库](./media/active-directory-saas-e-builder-tutorial/IC777379.png "应用程序库")

7.  在结果窗格中，选择**e-生成器**，然后单击**完成**添加该应用程序。

    ![e-生成器](./media/active-directory-saas-e-builder-tutorial/IC777380.png "e-生成器")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中验证到 e-生成器使用他们的帐户。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，**电子生成器**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-e-builder-tutorial/IC777381.png "配置单一登录")

2.  在**如何您希望用户能够登录到 e 生成器**页中**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-e-builder-tutorial/IC777382.png "配置单一登录")

3.  在**配置应用程序 URL**页面上的**e 生成器登录 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>.e Builder.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-e-builder-tutorial/IC777383.png "配置应用程序的 URL")

4.  **配置单一登录在 e 生成器**页面，以下载您的元数据，请单击**下载元数据**，并且数据然后为本地文件**c:\\e BuilderMetaData.xml**。

    ![配置单一登录](./media/active-directory-saas-e-builder-tutorial/IC777384.png "配置单一登录")

5.  转发该元数据文件到电子生成器支持团队。 支持团队需要为您的单一登录配置。

6.  选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-e-builder-tutorial/IC777385.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
没有配置用户供应到 e 生成器的操作项。  
当分派的用户试图登录到 e 生成器使用接入面板时，e 生成器检查用户是否存在。  
如果没有用户帐户可还，它是自动创建 e 生成器。
##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>若要将用户分配到 e 生成器，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**e 生成器**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-e-builder-tutorial/IC777386.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-e-builder-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
