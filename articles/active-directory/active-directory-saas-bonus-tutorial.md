<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Bonus.ly |Microsoft Azure" 
    description="了解如何使用 Bonus.ly Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>教程︰ 与 Bonus.ly 的 Azure Active Directory 集成

本教程的目的是显示 Azure 和 Bonus.ly 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在 Bonus.ly 测试租户

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Bonus.ly
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-bonus-tutorial/IC773679.png "方案")
##<a name="enabling-the-application-integration-for-bonusly"></a>启用应用程序集成为 Bonus.ly

本部分的目的是概述如何启用应用程序集成为 Bonus.ly。

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>若要启用应用程序集成为 Bonus.ly，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![启用单一登录](./media/active-directory-saas-bonus-tutorial/IC773680.png "启用单一登录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-bonus-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-bonus-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-bonus-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Bonus.ly**。

    ![应用程序库](./media/active-directory-saas-bonus-tutorial/IC773681.png "应用程序库")

7.  在结果窗格中，选择**Bonus.ly**，，然后单击**完成**添加应用程序。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Bonus.ly。  
Bonus.ly 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Bonus.ly**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-bonus-tutorial/IC749323.png "配置单一登录")

2.  在**您想怎样来登录到 Bonus.ly 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-bonus-tutorial/IC773683.png "配置单一登录")

3.  在**配置应用程序 URL**页面上的**Bonus.ly 租户 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。Bonus.ly*"，然后单击**下一步**︰ 

    ![配置应用程序的 URL](./media/active-directory-saas-bonus-tutorial/IC773684.png "配置应用程序的 URL")

4.  在**配置单一登录 Bonus.ly 在**页上，单击**下载证书**，然后将保存为本地证书文件**c:\\Bonusly.cer**。

    ![配置单一登录](./media/active-directory-saas-bonus-tutorial/IC773685.png "配置单一登录")

5.  在不同的浏览器窗口中，登录到**Bonus.ly**租户。

6.  在顶部的工具栏中，单击**设置**，然后选择**集成和应用程序**。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  在**单一登录**，选择**SAML**。

8.  在**SAML**对话框页面上，请执行以下步骤︰

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  在 Azure 经典门户中，在**配置单一登录在 Bonus.ly**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**IdP SSO 目标 URL**文本框。
    2.  在 Azure 经典门户中，在**配置单一登录在 Bonus.ly**对话框页面上，将**颁发者 ID**值，复制，然后将其粘贴到**IdP 颁发者**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在 Bonus.ly**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**IdP 登录 URL**文本框。
    4.  从导出的证书，复制的**指纹**值，然后将其粘贴到**证书指纹**文本框。

        >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

9.  单击**保存**。

10. 在 Microsoft Azure 的传统门户网站，选择该配置确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-bonus-tutorial/IC773689.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 Bonus.ly，他们必须到 Bonus.ly 配置。  
对于 Bonus.ly，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  在 web 浏览器窗口中，登录到 Bonus.ly 租户。

2.  单击**设置**

    ![设置](./media/active-directory-saas-bonus-tutorial/IC781041.png "设置")

3.  单击**用户和奖金**选项卡。

    ![用户和奖金](./media/active-directory-saas-bonus-tutorial/IC781042.png "用户和奖金")

4.  单击**管理用户**。

    ![管理用户](./media/active-directory-saas-bonus-tutorial/IC781043.png "管理用户")

5.  单击**添加用户**。

    ![添加用户](./media/active-directory-saas-bonus-tutorial/IC781044.png "添加用户")

6.  在**添加用户**对话框中，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-bonus-tutorial/IC781045.png "添加用户")

    1.  键入"**电子邮件**，**名字**，**姓氏**"要到相关文本框设置有效 AAD 帐户。
    2.  单击**保存**。

    >[AZURE.NOTE] AAD 帐户持有者会收到电子邮件，其中包含的链接来确认该帐户，然后将变为活动状态。

>[AZURE.NOTE] 您可以使用任何其他 Bonus.ly 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Bonus.ly 提供。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>要将用户分配到 Bonus.ly 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在 Bonus.ly 应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-bonus-tutorial/IC773690.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-bonus-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
