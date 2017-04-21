<properties 
    pageTitle="教程︰ 与 FM 的 Azure Active Directory 集成︰ 系统 |Microsoft Azure" 
    description="了解如何使用 FM︰ 系统使用 Azure 活动目录启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>教程︰ 与 FM 的 Azure Active Directory 集成︰ 系统
  
本教程的目的是显示 Azure 和 FM:Systems 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   FM:Systems-启用单一登录的订阅
  
后学完本教程，您已分配给 FM:Systems Azure AD 用户将能够到 FM:Systems 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 FM:Systems
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-fm-systems-tutorial/IC795899.png "方案")
##<a name="enabling-the-application-integration-for-fmsystems"></a>启用应用程序集成为 FM:Systems
  
本部分的目的是概述如何启用应用程序集成为 FM:Systems。

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>若要启用应用程序集成为 FM:Systems，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-fm-systems-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-fm-systems-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-fm-systems-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-fm-systems-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**FM:Systems**。

    ![应用程序库](./media/active-directory-saas-fm-systems-tutorial/IC795900.png "应用程序库")

7.  在结果窗格中，选择**FM:Systems**，，然后单击**完成**添加应用程序。

    ![FM︰ 系统](./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM︰ 系统")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够通过身份验证的使用他们的帐户 FM:Systems Azure AD 使用联合身份验证基于 SAML 协议中。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**FM:Systems**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-fm-systems-tutorial/IC790810.png "配置单一登录")

2.  在**您想怎样来登录到 FM:Systems 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-fm-systems-tutorial/IC795901.png "配置单一登录")

3.  在**配置应用程序 URL**页上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-fm-systems-tutorial/IC795902.png "配置应用程序的 URL")

    1.  在**FM:Systems 号上的 URL**文本框中，键入 FM:Systems**回复 URL** (例如︰ *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*)。  

        >[AZURE.WARNING] 您可以从您 FM 获取此值︰ 系统支持团队。

    2.  单击**下一步**

4.  在**配置单一登录 FM:Systems 在**页面上，要下载元数据，请单击**下载元数据**，然后将元数据保存在您的计算机上。

    ![配置单一登录](./media/active-directory-saas-fm-systems-tutorial/IC795903.png "配置单一登录")

5.  提交到您 FM 下载元数据文件︰ 系统支持团队。

    >[AZURE.NOTE] 您 FM︰ 系统支持团队必须执行实际的 SSO 配置。
当您的订阅已启用 SSO，您将得到通知。

6.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-fm-systems-tutorial/IC795904.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 FM:Systems，他们必须到 FM:Systems 配置。  
对于 FM:Systems，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  在 web 浏览器窗口中，以管理员身份登录到 FM:Systems 公司网站。

2.  转到**系统管理\>管理安全性\>用户\>用户列表**。

    ![系统管理](./media/active-directory-saas-fm-systems-tutorial/IC795905.png "系统管理")

3.  单击**创建新用户**。

    ![创建新用户](./media/active-directory-saas-fm-systems-tutorial/IC795906.png "创建新用户")

4.  在**创建用户**部分中，执行以下步骤︰

    ![创建用户](./media/active-directory-saas-fm-systems-tutorial/IC795907.png "创建用户")

    1.  键入用户名、 密码和确认、 电子邮件地址和有效的 Azure Active Directory 帐户要到相关文本框设置的员工 ID。
    2.  单击**下一步**。

>[AZURE.NOTE] 您可以使用任何其他 FM:Systems 用户帐户创建工具或 Api 提供的 FM:Systems 来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>要将用户分配到 FM:Systems 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**FM:Systems**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-fm-systems-tutorial/IC795908.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-fm-systems-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。