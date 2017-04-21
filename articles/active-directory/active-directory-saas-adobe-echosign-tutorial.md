<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Adobe EchoSign |Microsoft Azure" 
    description="了解如何使用 Adobe EchoSign Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>教程︰ Azure Active Directory 集成与 Adobe EchoSign

本教程的目的是显示 Azure 和 Adobe EchoSign 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   一个 Adobe EchoSign 登录启用订阅

后学完本教程，您已分配给 Adobe EchoSign Azure AD 用户将能够到 Adobe EchoSign 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Adobe EchoSign
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "方案")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>启用应用程序集成为 Adobe EchoSign

本部分的目的是概述如何启用 Adobe EchoSign 应用程序的集成。

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>若要启用应用程序集成为 Adobe EchoSign，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Adobe EchoSign**。

    ![应用程序库](./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "应用程序库")

7.  在结果窗格中，选择**Adobe EchoSign**，，然后单击**完成**添加应用程序。

    ![Adobe EchoSign](./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Adobe EchoSign。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Adobe EchoSign**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "配置单一登录")

2.  在**您想怎样来登录到 Adobe EchoSign 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**Adobe EchoSign 号上 URL**文本框中，键入您的 URL 使用以下模式"*https://company.echosign.com/*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "配置应用程序的 URL")

4.  在**配置单一登录 Adobe EchoSign 在**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Adobe EchoSign 公司网站。

6.  在顶部菜单中，单击**帐户**，然后在左凹模导航窗格中，在**帐户设置**下单击**SAML 设置**。

    ![帐户](./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "帐户")

7.  在 SAML 设置部分中，执行以下步骤︰

    ![SAML 设置](./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML 设置")

    1.  作为**SAML 模式**，选择**SAML 必填字段**。
    2.  选择**允许 EchoSign 帐户管理员使用其 EchoSign 凭据进行登录**。
    3.  为**用户创建**，选择**自动添加 SAML 通过身份验证的用户**。

8.  移动上，执行以下步骤︰

    ![SAML 设置](./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML 设置")

    1.  在 Azure 经典门户中，在**配置单一登录在 Adobe EchoSign**对话框页面上的**实体 ID**值，复制，然后将其粘贴到**IdP 实体 ID**文本框。
    2.  在 Azure 经典门户中，在**配置单一登录在 Adobe EchoSign**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**IdP 登录 URL**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在 Adobe EchoSign**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**IdP 注销 URL**文本框。
    4.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)
 5。  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到 6 **IdP 证书**文本框。  单击**保存更改**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 Adobe EchoSign，他们必须到 Adobe EchoSign 配置。  
在 Adobe EchoSign 资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**Adobe EchoSign**公司网站。

2.  在顶部菜单中，单击**帐户**，然后，在左凹模导航窗格中，单击**用户和组**，然后单击**创建新用户**。

    ![帐户](./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "帐户")

3.  在**创建新用户**部分中，执行以下步骤︰

    ![创建用户](./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "创建用户")

    1.  键入**电子邮件地址**、**名字**和**姓氏**要到相关文本框设置有效 AAD 帐户。
    2.  单击**创建用户**。

        >[AZURE.NOTE] Azure Active Directory 帐户持有者会收到电子邮件，其中包含的链接来确认该帐户，然后将变为活动状态。

>[AZURE.NOTE] 您可以使用任何其他 Adobe EchoSign 用户帐户创建工具或 Api 由 Adobe EchoSign 来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>若要将用户分配到 Adobe EchoSign，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Adobe EchoSign**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
