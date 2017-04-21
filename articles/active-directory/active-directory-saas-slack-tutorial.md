<properties 
    pageTitle="教程︰ Azure Active Directory 集成具有可宽延时间 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 可宽延时间启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>教程︰ Azure Active Directory 集成具有可宽延时间
  
本教程的目的是显示 Azure 和可宽延时间的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   启用订阅可宽延时间单一登录
  
学完本教程后, 将单一登录到应用程序 （服务提供程序初始化登录），您可宽延时间公司网站或使用[简介接入面板](active-directory-saas-access-panel-introduction.md)能够 Azure AD 用户已分配给可宽延时间。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的可宽延时间
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-slack-tutorial/IC794980.png "方案")

##<a name="enabling-the-application-integration-for-slack"></a>启用应用程序集成的可宽延时间
  
本部分的目的是概述如何启用应用程序集成的可宽延时间。

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>若要启用应用程序集成的可宽延时间，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-slack-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-slack-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-slack-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-slack-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**可宽延时间**。

    ![应用程序库](./media/active-directory-saas-slack-tutorial/IC794981.png "应用程序库")

7.  在结果窗格中，选择**可宽延时间**，然后单击**完成**添加应用程序。

    ![方案](./media/active-directory-saas-slack-tutorial/IC796925.png "方案")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到可宽延时间。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**可宽延时间**应用程序集成页中，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-slack-tutorial/IC794982.png "配置单一登录")

2.  **如何将用户注册到可宽延时间要**在页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-slack-tutorial/IC794983.png "配置单一登录")

3.  在**配置应用程序 URL**页面上的**可宽延时间符号在 URL**文本框中，键入您可宽延时间的租户的 URL (例如:"*https://azuread.slack.com*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-slack-tutorial/IC794984.png "配置应用程序的 URL")

4.  在**配置单一登录在可宽延时间**页中，若要下载您的证书，单击**下载证书**，然后保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-slack-tutorial/IC794985.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到可宽延时间公司网站。

6.  转到**向 Microsoft Azure AD\>团队设置**。

    ![工作组设置](./media/active-directory-saas-slack-tutorial/IC794986.png "工作组设置")

7.  **团队设置**部分中，单击**身份验证**选项卡，然后单击**更改设置**。

    ![工作组设置](./media/active-directory-saas-slack-tutorial/IC794987.png "工作组设置")

8.  在**SAML 验证设置**对话框中，请执行以下步骤︰

    ![SAML 设置](./media/active-directory-saas-slack-tutorial/IC794988.png "SAML 设置")

    1.  在 Azure 经典门户中，在**配置单一登录在可宽延时间**对话框页的**SAML SSO URL**值，复制，然后将其粘贴到**SAML 2.0 终结点 (HTTP)**文本框。
    2.  在 Azure 经典门户中，在**配置单一登录在可宽延时间**对话框页的**颁发者 URL**值，复制，然后将其粘贴到**标识提供商颁发者**文本框。
    3.  创建您已下载的证书的**base 64 编码**的文件。
    
        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**公用证书**文本框。
    5.  取消选择**允许用户更改他们的电子邮件地址**。
    6.  选择**允许用户选择他们自己的用户名**。
    7.  为**必须使用身份验证为您的团队**，选择**它是可选的**。
    8.  单击**保存配置**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-slack-tutorial/IC794989.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到可宽延时间，他们必须为可宽延时间设置。
  
没有为您配置用户的资源调配可宽延时间的操作项。  
当分派的用户试图登录到可宽延时间时，在必要时自动创建的可宽延时间的帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>若要将用户分配给可宽延时间，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**可宽延时间**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-slack-tutorial/IC794990.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-slack-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。