<properties 
    pageTitle="教程︰ Azure Active Directory 集成使用 Citrix ShareFile |Microsoft Azure" 
    description="了解如何使用 Citrix ShareFile Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>教程︰ 使用 citrix 服务器 ShareFile 的 Azure Active Directory 集成

本教程的目的是显示 Azure 和 citrix 服务器 ShareFile 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Citrix ShareFile 租户

后学完本教程，您已分配给 Citrix ShareFile Azure AD 用户将能够到在 Citrix ShareFile 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用用于 Citrix ShareFile 应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "方案")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>启用用于 Citrix ShareFile 应用程序集成

本部分的目的是概述如何启用用于 Citrix ShareFile 应用程序集成。

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>若要启用用于 Citrix ShareFile 应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Citrix ShareFile**。

    ![应用程序库](./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "应用程序库")

7.  在结果窗格中，选择**citrix 服务器 ShareFile**，，然后单击**完成**添加应用程序。

    ![Citrix ShareFile](./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 citrix 服务器 ShareFile。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Citrix ShareFile**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![启用单一登录](./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "启用单一登录")

2.  **您希望如何对 citrix 服务器 ShareFile 上注册用户**在页上，选择**Microsoft Azure AD 单一登录**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "配置单一登录")

3.  在**配置应用程序 URL**页面上**Citrix ShareFile 号上 URL**文本框中，键入您的 URL 使用以下模式`https://<tenant-name>.shareFile.com`，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "配置应用程序的 URL")

4.  在**配置单一登录在 Citrix ShareFile**页面上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![ConfigureSingle 号上](./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle 号上")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到**Citrix ShareFile**公司网站。

6.  在顶部工具栏上，单击**管理**。

7.  在左侧的导航窗格中，选择**配置单一登录**。

    ![帐户管理](./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "帐户管理")

8.  在**单一登录 / SAML 2.0 配置**对话框页面下**的基本设置**，请执行以下步骤︰

    ![单一登录](./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "单一登录")

    1.  单击**启用 SAML**。
    2.  在 Azure 经典门户中，在**配置单一登录在 Citrix ShareFile**对话框页复制**实体 ID**值，并粘贴到**您 IDP 颁发者 / 实体 ID**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在 Citrix ShareFile**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**登录 URL**文本框。
    4.  在 Azure 经典门户中，**配置单一登录在 Citrix ShareFile**对话框页面上的**远程注销 URL**值，复制，然后将其粘贴到**注销 URL**文本框。
    5.  **X.509 证书**字段旁边单击**更改**，然后上载从 Azure AD 传统门户网站上下载的证书。
        ![基本设置](./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "基本设置")

9.  在 Citrix ShareFile 管理门户网站上，单击**保存**。

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 citrix 服务器 ShareFile，他们必须到 Citrix ShareFile 设置。  
在 citrix 服务器 ShareFile，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  登录到**Citrix ShareFile**租户。

2.  单击**管理用户\>管理用户主页\>+ 创建雇员**。

    ![创建雇员](./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "创建雇员")

3.  输入**的电子邮件**、**名字**和有效的 Azure 广告的**姓**考虑您希望提供。

    ![基本信息](./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "基本信息")

4.  单击**添加用户**。

    >[AZURE.NOTE] AAD 帐户持有者将收到一封电子邮件，点击链接以确认他们的帐户，再将变为活动状态。

>[AZURE.NOTE] 您可以使用任何其他 Citrix ShareFile 用户帐户创建工具或 Api 来设置 AAD 用户帐户提供 citrix 服务器 ShareFile。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>若要将用户分配到 citrix 服务器 ShareFile，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Citrix ShareFile**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
