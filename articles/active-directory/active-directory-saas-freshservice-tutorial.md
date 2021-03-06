<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 FreshService |Microsoft Azure" 
    description="了解如何使用 FreshService Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>教程︰ 与 FreshService 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 FreshService 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   FreshService-启用单一登录的订阅
  
后学完本教程，您已分配给 FreshService Azure AD 用户将能够到单一登录到应用程序中使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 FreshService
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-freshservice-tutorial/IC790807.png "方案")
##<a name="enabling-the-application-integration-for-freshservice"></a>启用应用程序集成为 FreshService
  
本部分的目的是概述如何启用应用程序集成为 FreshService。

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>若要启用应用程序集成为 FreshService，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-freshservice-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-freshservice-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-freshservice-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-freshservice-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**FreshService**。

    ![应用程序库](./media/active-directory-saas-freshservice-tutorial/IC790808.png "应用程序库")

7.  在结果窗格中，选择**FreshService**，，然后单击**完成**添加应用程序。

    ![Freshservice](./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 FreshService。  
FreshService 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**FreshService**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-freshservice-tutorial/IC790810.png "配置单一登录")

2.  在**您想怎样来登录到 FreshService 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-freshservice-tutorial/IC790811.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**FreshService 号上的 URL**文本框中，键入用户用于登录到您的 Freshdesk 应用程序 URL (例如:"*http://democompany.freshservice.com/*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-freshservice-tutorial/IC790812.png "配置应用程序的 URL")

4.  在**配置单一登录 FreshService 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-freshservice-tutorial/IC790813.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 FreshService 公司网站。

6.  在顶部菜单中，单击**管理**。

    ![管理](./media/active-directory-saas-freshservice-tutorial/IC790814.png "管理")

7.  在**客户门户**中，单击**安全**。

    ![安全](./media/active-directory-saas-freshservice-tutorial/IC790815.png "安全")

8.  在**安全性**部分中，执行以下步骤︰

    ![单一登录](./media/active-directory-saas-freshservice-tutorial/IC790816.png "单一登录")

    1.  **单一登录 OnON**进行切换。
    2.  选择**SAML SSO**。
    3.  在 Azure 经典门户中，在**配置单一登录在 FreshService**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**SAML 登录 URL**文本框。
    4.  在 Azure 经典门户中，在**配置单一登录在 FreshService**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**注销 URL**文本框。
    5.  从导出的证书，复制的**指纹**值，然后将其粘贴到文本框中**安全证书指纹**。
    
        >[AZURE.TIP]有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-freshservice-tutorial/IC790817.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 FreshService，他们必须到 FreshService 配置。  
对于 FreshService，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**FreshService**公司网站。

2.  在顶部菜单中，单击**管理**。

    ![管理](./media/active-directory-saas-freshservice-tutorial/IC790814.png "管理")

3.  在**用户管理**部分中，单击**申请人**。

    ![请求者](./media/active-directory-saas-freshservice-tutorial/IC790818.png "请求者")

4.  单击**新的申请者**。

    ![新申请者](./media/active-directory-saas-freshservice-tutorial/IC790819.png "新申请者")

5.  在**新申请者**部分中，执行以下步骤︰

    ![新申请者](./media/active-directory-saas-freshservice-tutorial/IC790820.png "新申请者")

    1.  加入相关的文本框输入规定有效所需的 Azure Active Directory 帐户**名**和**电子邮件**属性。
    2.  单击**保存**。

    >[AZURE.NOTE] Azure Active Directory 帐户持有人将获得包括确认该帐户之前将变为活动状态的链接的电子邮件

>[AZURE.NOTE] 您可以使用任何其他 FreshService 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 FreshService 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>要将用户分配到 FreshService 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**FreshService**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-freshservice-tutorial/IC790821.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-freshservice-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。