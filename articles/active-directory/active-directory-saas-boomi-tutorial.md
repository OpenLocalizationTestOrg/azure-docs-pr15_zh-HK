<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Boomi |Microsoft Azure" 
    description="了解如何使用 Boomi Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>教程︰ 与 Boomi 的 Azure Active Directory 集成

本教程的目的是显示 Azure 和 Boomi 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Boomi-启用单一登录的订阅

后学完本教程，您已分配给 Boomi Azure AD 用户将能够到 Boomi 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Boomi
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-boomi-tutorial/IC791134.png "方案")
##<a name="enabling-the-application-integration-for-boomi"></a>启用应用程序集成为 Boomi

本部分的目的是概述如何启用应用程序集成为 Boomi。

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>若要启用应用程序集成为 Boomi，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-boomi-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-boomi-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-boomi-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-boomi-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Boomi**。

    ![应用程序库](./media/active-directory-saas-boomi-tutorial/IC790822.png "应用程序库")

7.  在结果窗格中，选择**Boomi**，，然后单击**完成**添加应用程序。

    ![Boomi](./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Boomi。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Boomi**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-boomi-tutorial/IC790824.png "配置单一登录")

2.  在**您想怎样来登录到 Boomi 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-boomi-tutorial/IC790825.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**Boomi 回复 URL**文本框中，键入您的**Boomi AtomSphere 登录 URL** (例如:"*https://platform.boomi.com/sso/AccountName/saml*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-boomi-tutorial/IC790826.png "配置应用程序的 URL")

4.  在**配置单一登录 Boomi 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-boomi-tutorial/IC790827.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Boomi 公司网站。

6.  在顶部的工具栏中，单击您的公司名称，然后**安装程序**。

    ![安装程序](./media/active-directory-saas-boomi-tutorial/IC790828.png "安装程序")

7.  单击**SSO 选项**。

    ![SSO 选项](./media/active-directory-saas-boomi-tutorial/IC790829.png "SSO 选项")

8.  在**单一登录选项**部分中，执行以下步骤︰

    ![单一登录选项](./media/active-directory-saas-boomi-tutorial/IC790830.png "单一登录选项")

    1.  选择**启用 SAML 单一登录**。
    2.  单击**导入**，上载下载的证书。
    3.  在 Azure 经典门户中，在**配置单一登录在 Boomi**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**标识提供程序的登录 URL**文本框。
    4.  作为**联合身份验证 Id 位置**，选择**联合身份验证 Id 是主题的 NameID 元素中**。
    5.  单击**保存**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-boomi-tutorial/IC775560.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 Boomi，他们必须被调配到 Boomi。  
对于 Boomi，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Boomi**公司网站。

2.  转到**用户管理\>用户**。

    ![用户](./media/active-directory-saas-boomi-tutorial/IC790831.png "用户")

3.  单击**添加用户**。

    ![添加用户](./media/active-directory-saas-boomi-tutorial/IC790832.png "添加用户")

4.  在**添加用户角色**对话框页，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-boomi-tutorial/IC790833.png "添加用户")

    1.  键入**名字**、**姓氏**和**电子邮件**有效 AAD 帐户，您希望提供到相关的文本框。
    2.  单击确定。

>[AZURE.NOTE] 您可以使用任何其他 Boomi 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Boomi 提供。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>要将用户分配到 Boomi 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Boomi**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-boomi-tutorial/IC790834.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-boomi-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
