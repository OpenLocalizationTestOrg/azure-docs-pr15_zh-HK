<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 ScreenSteps |Microsoft Azure" 
    description="了解如何使用 ScreenSteps Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>教程︰ 与 ScreenSteps 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 ScreenSteps 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   ScreenSteps 租户
  
后学完本教程，您已分配给 ScreenSteps Azure AD 用户将能够到 ScreenSteps 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 ScreenSteps
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-screensteps-tutorial/IC778516.png "方案")
##<a name="enabling-the-application-integration-for-screensteps"></a>启用应用程序集成为 ScreenSteps
  
本部分的目的是概述如何启用应用程序集成为 ScreenSteps。

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>若要启用应用程序集成为 ScreenSteps，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-screensteps-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-screensteps-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-screensteps-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-screensteps-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**ScreenSteps**。

    ![应用程序库](./media/active-directory-saas-screensteps-tutorial/IC778517.png "应用程序库")

7.  在结果窗格中，选择**ScreenSteps**，，然后单击**完成**添加应用程序。

    ![ScreenSteps](./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ScreenSteps。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**ScreenSteps**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-screensteps-tutorial/IC778519.png "配置单一登录")

2.  在**您想怎样来登录到 ScreenSteps 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-screensteps-tutorial/IC778520.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**ScreenSteps 号中 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。ScreenSteps.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-screensteps-tutorial/IC778521.png "配置应用程序的 URL")

4.  在**配置单一登录 ScreenSteps 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-screensteps-tutorial/IC778522.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 ScreenSteps 公司网站。

6.  单击**帐户管理**。

    ![帐户管理](./media/active-directory-saas-screensteps-tutorial/IC778523.png "帐户管理")

7.  单击**远程身份验证**。

    ![远程身份验证](./media/active-directory-saas-screensteps-tutorial/IC778524.png "远程身份验证")

8.  单击**创建身份验证终结点**。

    ![远程身份验证](./media/active-directory-saas-screensteps-tutorial/IC778525.png "远程身份验证")

9.  在**创建身份验证端点**部分中，执行以下步骤︰

    ![创建身份验证终结点](./media/active-directory-saas-screensteps-tutorial/IC778526.png "创建身份验证终结点")

    1.  在**标题**文本框中，键入标题。
    2.  从**模式**列表中，选择**SAML**。
    3.  单击**创建**。

10. 在**远程身份验证端点**部分中，执行以下步骤︰

    ![远程身份验证终结点](./media/active-directory-saas-screensteps-tutorial/IC778527.png "远程身份验证终结点")

    1.  在 Azure 经典门户中，**配置单一登录 ScreenSteps 在**页上复制的**远程登录 URL**值，，然后将其粘贴到**远程登录 URL**文本框。
    2.  在 Azure 经典门户中，**配置单一登录 ScreenSteps 在**页上复制的**远程注销 URL**值，然后将其粘贴到**登出 URL**文本框。
    3.  单击**选择文件**，然后上载下载的证书。
    4.  单击**更新**。

11. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-screensteps-tutorial/IC778542.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到**ScreenSteps**，他们必须到**ScreenSteps**配置。  
对于**ScreenSteps**，资源调配是手动任务。

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>若要设置到 ScreenSteps 的用户帐户，请执行以下步骤︰

1.  登录到**ScreenSteps**租户。

2.  单击**帐户管理**。

    ![帐户管理](./media/active-directory-saas-screensteps-tutorial/IC778523.png "帐户管理")

3.  单击**用户**。

    ![用户](./media/active-directory-saas-screensteps-tutorial/IC778544.png "用户")

4.  单击**创建用户**。

    ![所有用户](./media/active-directory-saas-screensteps-tutorial/IC778545.png "所有用户")

5.  从**用户角色**列表中，选择您的用户的角色。

6.  在用户角色部分中，有效的 AAD 帐户，您希望提供到相关的文本框中键入**"名字**、**姓氏**、**电子邮件**、**登录**、**密码**和**密码确认"**。

    ![新用户](./media/active-directory-saas-screensteps-tutorial/IC778546.png "新用户")

7.  在组部分中，选择**身份验证组 (SAML)**，，然后单击**创建用户**。

    ![组](./media/active-directory-saas-screensteps-tutorial/IC778547.png "组")

>[AZURE.NOTE]您可以使用任何其他 ScreenSteps 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 ScreenSteps 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>要将用户分配到 ScreenSteps 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**ScreenSteps**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-screensteps-tutorial/IC773094.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![分配用户](./media/active-directory-saas-screensteps-tutorial/IC778548.png "分配用户")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。