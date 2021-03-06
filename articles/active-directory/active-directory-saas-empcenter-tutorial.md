<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 EmpCenter |Microsoft Azure" 
    description="了解如何使用 EmpCenter Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>教程︰ 与 EmpCenter 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 EmpCenter 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   EmpCenter-启用单一登录的订阅
  
后学完本教程，您已分配给 EmpCenter Azure AD 用户将能够到 EmpCenter 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 EmpCenter
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-empcenter-tutorial/IC802916.png "方案")
##<a name="enabling-the-application-integration-for-empcenter"></a>启用应用程序集成为 EmpCenter
  
本部分的目的是概述如何启用应用程序集成为 EmpCenter。

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>若要启用应用程序集成为 EmpCenter，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-empcenter-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-empcenter-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-empcenter-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-empcenter-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**EmpCenter**。

    ![应用程序库](./media/active-directory-saas-empcenter-tutorial/IC802917.png "应用程序库")

7.  在结果窗格中，选择**EmpCenter**，，然后单击**完成**添加应用程序。

    ![EmpCentral](./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 EmpCenter。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**EmpCenter**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802919.png "配置单一登录")

2.  在**您想怎样来登录到 EmpCenter 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802920.png "配置单一登录")

3.  在**配置应用程序设置**页中，请执行以下步骤︰

    ![配置应用程序设置](./media/active-directory-saas-empcenter-tutorial/IC802921.png "配置应用程序设置")

    1.  在**符号上 URL**文本框中，键入您为登录到 EmpCenter 应用程序的用户使用的 URL (例如︰ *https://partner-authenticati.empcenter.com/workforce/SSO.do*)。
    2.  单击**下一步**

4.  在**配置单一登录 EmpCenter 在**页上，要下载元数据，请单击**下载元数据**，然后将保存在您的计算机上的元数据文件。

    ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802922.png "配置单一登录")

5.  将下载元数据文件发送到 EmpCenter 的支持团队。

    >[AZURE.NOTE] EmpCenter 的支持团队就不得不执行实际的 SSO 配置。
当您的订阅已启用 SSO，您将得到通知。

6.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802923.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 EmpCenter，他们必须到 EmpCenter 配置。  
对于 EmpCenter，用户帐户需要创建的 EmpCenter 的支持团队。

>[AZURE.NOTE] 您可以使用任何其他 EmpCenter 用户帐户创建工具或 Api 提供的 EmpCenter 设置 Azure 活动目录的用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>要将用户分配到 EmpCenter 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**EmpCenter**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-empcenter-tutorial/IC802924.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-empcenter-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。