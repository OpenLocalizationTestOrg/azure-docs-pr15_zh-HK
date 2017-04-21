<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 UserVoice |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory UserVoice 启用单一登录、 自动化资源调配，和更多。" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>教程︰ 与 UserVoice 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 UserVoice 集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   UserVoice 租户
  
后学完本教程，您已分配给 UserVoice Azure AD 用户将能够到 UserVoice 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 UserVoice
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-uservoice-tutorial/IC777514.png "方案")

##<a name="enabling-the-application-integration-for-uservoice"></a>启用应用程序集成为 UserVoice
  
本部分的目的是概述如何启用 UserVoice 用于应用程序集成。

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>若要启用 UserVoice 用于应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-uservoice-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-uservoice-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-uservoice-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-uservoice-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**UserVoice**。

    ![应用程序库](./media/active-directory-saas-uservoice-tutorial/IC777513.png "应用程序库")

7.  在结果窗格中，选择**UserVoice**，，然后单击**完成**添加应用程序。

    ![UserVoice](./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 UserVoice。  
配置单一登录为 UserVoice 需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 经典门户， **UserVoice**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-uservoice-tutorial/IC777515.png "配置单一登录")

2.  在**您想怎样用户能够登录到 UserVoice**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-uservoice-tutorial/IC777516.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**UserVoice 号在 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。UserVoice.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-uservoice-tutorial/IC777517.png "配置应用程序的 URL")

4.  在**配置单一登录 UserVoice 在**页上，下载您的证书，单击**证书下载**，然后保存为本地证书文件**c:\\UserVoice.cer**。

    ![配置单一登录](./media/active-directory-saas-uservoice-tutorial/IC777518.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您 UserVoice 公司网站。

6.  在顶部的工具栏中，单击设置，然后从菜单中选择 Web 门户。

    ![设置](./media/active-directory-saas-uservoice-tutorial/IC777519.png "设置")

7.  **Web 门户网站**选项卡上的在**用户验证**部分中，单击**编辑**以打开**编辑用户身份验证**对话框页

    ![Web 门户](./media/active-directory-saas-uservoice-tutorial/IC777520.png "Web 门户")

8.  在**编辑用户身份验证**对话框页面上，请执行以下步骤︰

    ![编辑用户身份验证](./media/active-directory-saas-uservoice-tutorial/IC777521.png "编辑用户身份验证")

    1.  **单一登录 (SSO)**，请单击。
    2.  在 Azure 经典门户中，在**配置单一登录在 UserVoice**对话框页面上，复制的**远程登录 URL**值，，然后将其粘贴到**SSO 远程登录的**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在 UserVoice**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**SSO 远程 Sign-Out 文本框**。
    4.  从导出的证书，复制的**指纹**值，然后将其粘贴到**当前证书的 SHA1 指纹**文本框。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    5.  单击**保存身份验证设置**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-uservoice-tutorial/IC777522.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 UserVoice，他们必须到 UserVoice 调配。  
对于 UserVoice，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  登录到**UserVoice**租户。

2.  请转到**设置**。

    ![设置](./media/active-directory-saas-uservoice-tutorial/IC777811.png "设置")

3.  单击**常规**。

4.  单击**代理和权限**。

    ![代理和权限](./media/active-directory-saas-uservoice-tutorial/IC777812.png "代理和权限")

5.  单击**添加管理员**。

    ![添加管理员](./media/active-directory-saas-uservoice-tutorial/IC777813.png "添加管理员")

6.  在**邀请管理员**对话框中，请执行以下步骤︰

    ![邀请管理](./media/active-directory-saas-uservoice-tutorial/IC777814.png "邀请管理")

    1.  在电子邮件 texbox 中，键入要提供，，然后单击**添加**的帐户的电子邮件地址。
    2.  单击**邀请**。

>[AZURE.NOTE] 您可以使用任何其他 UserVoice 用户帐户创建工具或 Api 提供 UserVoice 来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>若要将用户分配到 UserVoice，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**UserVoice**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-uservoice-tutorial/IC777523.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-uservoice-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。