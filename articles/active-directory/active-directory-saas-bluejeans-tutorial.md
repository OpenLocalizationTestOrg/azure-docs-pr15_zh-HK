<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 BlueJeans |Microsoft Azure" 
    description="了解如何使用 BlueJeans Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>教程︰ 与 BlueJeans 的 Azure AD 集成

本教程的目的是显示 Azure 和 BlueJeans 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   BlueJeans-启用单一登录的订阅

后学完本教程，您已分配给 BlueJeans Azure AD 用户将能够到 BlueJeans 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 BlueJeans
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-bluejeans-tutorial/IC785860.png "方案")
##<a name="enabling-the-application-integration-for-bluejeans"></a>启用应用程序集成为 BlueJeans

本部分的目的是概述如何启用应用程序集成为 BlueJeans。

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>若要启用应用程序集成为 BlueJeans，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-bluejeans-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-bluejeans-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-bluejeans-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-bluejeans-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**BlueJeans**。

    ![应用程序库](./media/active-directory-saas-bluejeans-tutorial/IC785861.png "应用程序库")

7.  在结果窗格中，选择**BlueJeans**，，然后单击**完成**添加应用程序。

    ![BlueJeans](./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 BlueJeans。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**BlueJeans**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-bluejeans-tutorial/IC785863.png "配置单一登录")

2.  在**您想怎样来登录到 BlueJeans 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-bluejeans-tutorial/IC785864.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**BlueJeans 号上的 URL**文本框中，键入您的 URL 使用以下模式"*https://company.BlueJeans.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-bluejeans-tutorial/IC785865.png "配置应用程序的 URL")

4.  在**配置单一登录 BlueJeans 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-bluejeans-tutorial/IC785866.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到**BlueJeans**公司网站。

6.  转到**管理\>组设置\>安全**。

    ![管理](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "管理")

7.  在**安全性**部分中，执行以下步骤︰

    ![SAML 单一登录](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML 单一登录")

    1.  选择**SAML 单一登录**。
    2.  选择**启用自动提供**。

8.  转与以下步骤︰

    ![证书路径](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "证书路径")

    1.  单击**选择文件**，然后上载下载的证书。
    2.  在 Azure 经典门户中，在**配置单一登录在 BlueJeans**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**登录 URL**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在 BlueJeans**对话框页复制**更改密码 URL**值，，然后将其粘贴到**密码更改 URL**文本框。
    4.  在 Azure 经典门户中，在**配置单一登录在 BlueJeans**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**注销 URL**文本框。

9.  转与以下步骤︰

    ![保存更改](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "保存更改")

    1.  在**用户 id**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**。
    2.  在**电子邮件**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**。
    3.  单击**保存更改**。

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-bluejeans-tutorial/IC785876.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 BlueJeans，他们必须被调配到 BlueJeans。  
对于 BlueJeans，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**BlueJeans**公司网站。

2.  转到**管理\>管理用户\>添加用户**。

    ![管理](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "管理")

    >[AZURE.IMPORTANT] **添加用户**选项卡才可用，在**安全选项卡**中，**启用自动提供**处于未选中状态。

3.  在**添加用户**部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "添加用户")

    1.  键入**BlueJeans 用户名**、**电子邮件地址**、 **BlueJeans 会议 ID**，**审查密码**、**全名**、 要到相关文本框提供有效的 AAD 帐户的**公司**。
    2.  单击**添加用户**。

>[AZURE.NOTE] 您可以使用任何其他 BlueJeans 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 BlueJeans 提供。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>要将用户分配到 BlueJeans 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**BlueJeans**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-bluejeans-tutorial/IC785887.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-bluejeans-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
