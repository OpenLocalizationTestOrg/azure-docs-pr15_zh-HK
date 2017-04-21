<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Gigya |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Gigya 启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>教程︰ 与 Gigya 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Gigya 集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Gigya 单一登录启用订阅
  
后学完本教程，您已分配给 Gigya Azure AD 用户将能够到 Gigya 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Gigya
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![配置单一登录](./media/active-directory-saas-gigya-tutorial/IC789512.png "配置单一登录")
##<a name="enabling-the-application-integration-for-gigya"></a>启用应用程序集成为 Gigya
  
本部分的目的是概述如何启用 Gigya 用于应用程序集成。

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>若要启用 Gigya 用于应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-gigya-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-gigya-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-gigya-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-gigya-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Gigya**。

    ![应用程序库](./media/active-directory-saas-gigya-tutorial/IC789513.png "应用程序库")

7.  在结果窗格中，选择**Gigya**，，然后单击**完成**添加应用程序。

    ![Gigya](./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Gigya。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Gigya**集成应用程序页，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-gigya-tutorial/IC789528.png "配置单一登录")

2.  在**您想怎样用户能够登录到 Gigya**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-gigya-tutorial/IC789529.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**Gigya 号在 URL**文本框中，键入您的 URL 使用以下模式"*http://company.gigya.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-gigya-tutorial/IC789530.png "配置应用程序的 URL")

4.  在**配置单一登录 Gigya 在**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-gigya-tutorial/IC789531.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您 Gigya 公司网站。

6.  转到**设置\>SAML 登录**，然后单击**添加**按钮。

    ![SAML 登录](./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML 登录")

7.  在**SAML 登录**部分中，执行以下步骤︰

    ![SAML 配置](./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML 配置")

    1.  在**名称**文本框中，键入您的配置的名称。
    2.  在 Azure 经典门户中，在**配置单一登录在 Gigya**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在 Gigya**对话框页面上，将**单一登录服务 URL**值，复制，然后将其粘贴到**单一登录服务 URL**文本框。
    4.  在 Azure 经典门户中，在**配置单一登录在 Gigya**对话框页面上，复制**名称标识符的格式**值，，然后将其粘贴到**名称 ID 格式**文本框。
    5.  创建您已下载的证书的**base 64 编码**的文件。
        
        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    6.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**X.509 证书**文本框。
    7.  单击**保存设置**。

8.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-gigya-tutorial/IC789534.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Gigya，他们必须到 Gigya 提供。  
对于 Gigya，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到**Gigya**公司网站。

2.  转到**管理\>管理用户**，然后单击**邀请用户**。

    ![管理用户](./media/active-directory-saas-gigya-tutorial/IC789535.png "管理用户")

3.  在邀请用户对话框中，请执行以下步骤︰

    ![邀请用户](./media/active-directory-saas-gigya-tutorial/IC789536.png "邀请用户")

    1.  在**电子邮件**文本框中，键入您要提供一个有效的 Azure Active Directory 帐户的电子邮件别名。
    2.  单击**邀请用户**。
    
        >[AZURE.NOTE] Azure Active Directory 帐户持有者会收到电子邮件，其中包含的链接来确认该帐户，然后将变为活动状态。

>[AZURE.NOTE] 您可以使用任何其他 Gigya 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Gigya 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>若要将用户分配到 Gigya，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Gigya**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-gigya-tutorial/IC789537.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-gigya-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。