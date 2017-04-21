<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Huddle |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Huddle 启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>教程︰ 与 Huddle 的 Azure Active Directory 集成
  
本教程的目的是显示的 Azure，Huddle 集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Huddle-启用单一登录的订阅
  
后学完本教程，您已分配给 Huddle Azure AD 用户将能够到 Huddle 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的 Huddle
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![配置单一登录](./media/active-directory-saas-huddle-tutorial/IC787830.png "配置单一登录")
##<a name="enabling-the-application-integration-for-huddle"></a>启用应用程序集成的 Huddle
  
本部分的目的是概述如何启用 Huddle 用于应用程序集成。

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>若要启用应用程序集成的 Huddle，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-huddle-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-huddle-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-huddle-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-huddle-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Huddle**。

    ![应用程序库](./media/active-directory-saas-huddle-tutorial/IC787831.png "应用程序库")

7.  在结果窗格中，选择**Huddle**，，然后单击**完成**添加应用程序。

    ![Huddle](./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Huddle。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 的经典门户， **Huddle**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-huddle-tutorial/IC787833.png "配置单一登录")

2.  **如何将用户注册到 Huddle 要**在页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-huddle-tutorial/IC787834.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**Huddle 号在 URL**文本框中，键入您使用以下模式"*http://company.huddle.com*"的 Huddle 租户的 URL，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-huddle-tutorial/IC787835.png "配置应用程序的 URL")

4.  在**配置单一登录 Huddle 在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-huddle-tutorial/IC787836.png "配置单一登录")

    1.  单击**下载证书**，然后将保存在您的计算机上的证书文件。
    2.  **颁发者 URL**值， **SAML SSO URL**值和下载的证书，复制，然后将其发送给 Huddle 支持小组。

    >[AZURE.NOTE] 单一登录需要启用 Huddle 支持小组。
完成配置后，您会收到通知。

5.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-huddle-tutorial/IC787837.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Huddle，他们必须到 Huddle 调配。  
对于 Huddle，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Huddle**公司网站。

2.  单击**工作区**。

3.  单击**人\>邀请其他人**。

    ![人员](./media/active-directory-saas-huddle-tutorial/IC787838.png "人员")

4.  在**创建新邀请**部分中，执行以下步骤︰

    ![新邀请](./media/active-directory-saas-huddle-tutorial/IC787839.png "新邀请")

    1.  在**选择要邀请其他人加入一个团队**列表中，选择**团队**。
    2.  键入要到相关文本框设置有效的 AAD 帐户的**电子邮件地址**。
    3.  单击**邀请**。

    >[AZURE.NOTE] Azure 的广告帐户持有者会收到包括确认该帐户之前将变为活动状态的链接的电子邮件。

>[AZURE.NOTE] 您可以使用任何其他 Huddle 用户帐户创建工具或 Api 提供的资源调配 AAD 的 Huddle 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>若要将用户分配到 Huddle，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Huddle**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-huddle-tutorial/IC787840.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-huddle-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。