<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Onit |Microsoft Azure" 
    description="了解如何使用 Onit Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>教程︰ 与 Onit 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Onit 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Onit-启用单一登录的订阅
  
后学完本教程，您已分配给 Onit Azure AD 用户将能够到 Onit 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Onit
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-onit-tutorial/IC791166.png "方案")
##<a name="enabling-the-application-integration-for-onit"></a>启用应用程序集成为 Onit
  
本部分的目的是概述如何启用应用程序集成为 Onit。

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>若要启用应用程序集成为 Onit，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-onit-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-onit-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-onit-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-onit-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Onit**。

    ![应用程序库](./media/active-directory-saas-onit-tutorial/IC791167.png "应用程序库")

7.  在结果窗格中，选择**Onit**，，然后单击**完成**添加应用程序。

    ![Onit](./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Onit。  
Onit 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。
  
Onit 应用程序需要以特定的格式，这就需要你对**saml 令牌属性**配置中添加自定义属性映射 SAML 断言。  
下面的屏幕快照显示此示例。

![单一登录](./media/active-directory-saas-onit-tutorial/IC791168.png "单一登录")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 经典门户中， **Onit**应用程序集成在页上，在菜单上，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-onit-tutorial/IC791169.png "属性")

2.  若要添加所需的属性映射，请执行以下步骤︰

    
  	|属性名称|属性值|
  	|---|---|
  	|名称|User.userprincipalname|
  	|电子邮件|User.mail|

    1.  上表中每个数据行，请单击**添加用户属性**。
    2.  在**属性名称**文本框中，键入该行显示的属性名称。
    3.  从**属性值**列表中，选择的行中显示的属性值。
    4.  单击**完成**。

3.  单击**应用更改**。

4.  在浏览器中，单击**后**再次打开**快速入门**对话框中。

5.  **配置单一登录**，打开**配置单一登录**对话框中单击。

    ![配置单一登录](./media/active-directory-saas-onit-tutorial/IC791170.png "配置单一登录")

6.  在**您想怎样来登录到 Onit 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-onit-tutorial/IC791171.png "配置单一登录")

7.  在**配置应用程序 URL**页面上的**Onit 号上的 URL**文本框中，键入用户用于登录到您的 Onit 应用程序的 URL (例如:"*https://ms-sso-test.onit.com*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-onit-tutorial/IC791172.png "配置应用程序的 URL")

8.  在**配置单一登录 Onit 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-onit-tutorial/IC791173.png "配置单一登录")

9.  在不同的 web 浏览器窗口中，以管理员身份登录到 Onit 公司网站。

10. 在菜单上，单击**管理**。

    ![管理](./media/active-directory-saas-onit-tutorial/IC791174.png "管理")

11. 单击**编辑公司**。

    ![编辑公司](./media/active-directory-saas-onit-tutorial/IC791175.png "编辑公司")

12. 单击**安全**选项卡。

    ![编辑公司信息](./media/active-directory-saas-onit-tutorial/IC791176.png "编辑公司信息")

13. 在**安全性**选项卡，请执行以下步骤︰

    ![单一登录](./media/active-directory-saas-onit-tutorial/IC791177.png "单一登录")

    1.  作为**身份验证策略**，选择**单一登录和密码**。
    2.  在 Azure 经典门户中，在**配置单一登录在 Onit**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**Idp 目标 URL**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在 Onit**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**Idp 注销 URL**文本框。
    4.  从导出的证书，复制的**指纹**值，然后将其粘贴到**Idp 证书指纹 (SHA1)**文本框。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    5.  作为**SSO 类型**，选择**SAML**。
    6.  在**SSO 登录按钮文本**文本框中，键入您希望按钮文本。
    7.  选择**使用 SSO 登录︰ 所需的每个用户的域下**，在相关的文本框中，键入一个测试用户的电子邮件地址，然后单击**更新**。
        ![编辑公司](./media/active-directory-saas-onit-tutorial/IC791178.png "编辑公司")

14. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-onit-tutorial/IC791179.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Onit，他们必须到 Onit 配置。  
对于 Onit，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Onit**公司网站。

2.  单击**添加用户**。

    ![管理](./media/active-directory-saas-onit-tutorial/IC791180.png "管理")

3.  在**添加用户**对话框页，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-onit-tutorial/IC791181.png "添加用户")

    1.  键入**名称**和要到相关文本框设置有效的 AAD 帐户的**电子邮件地址**。
    2.  单击**创建**。  

        >[AZURE.NOTE] 帐户属主会包括确认该帐户之前将变为活动状态的链接的电子邮件。

>[AZURE.NOTE] 您可以使用任何其他 Onit 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Onit 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>要将用户分配到 Onit 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Onit**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-onit-tutorial/IC791182.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-onit-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。