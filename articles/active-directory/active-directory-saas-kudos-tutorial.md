<properties 
    pageTitle="教程︰ Azure Active Directory 集成与感谢 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 感谢启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>教程︰ 与感谢的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和感谢的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   感谢租户
  
后学完本教程，您已分配给感谢 Azure AD 用户将能够到感谢公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的感谢
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-kudos-tutorial/IC787799.png "方案")
##<a name="enabling-the-application-integration-for-kudos"></a>启用应用程序集成的感谢
  
本部分的目的是概述如何启用应用程序集成的感谢。

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>若要启用应用程序集成为感谢，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-kudos-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-kudos-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-kudos-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-kudos-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**感谢**。

    ![应用程序库](./media/active-directory-saas-kudos-tutorial/IC787800.png "应用程序库")

7.  在结果窗格中，选择**感谢**，，然后单击**完成**添加应用程序。

    ![感谢](./media/active-directory-saas-kudos-tutorial/IC787801.png "感谢")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到感谢。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**感谢**集成应用程序页，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-kudos-tutorial/IC787802.png "配置单一登录")

2.  在**您想怎样用户能够登录到感谢**页面上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-kudos-tutorial/IC787803.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**感谢号在 URL**文本框中，键入您的 URL 使用以下模式"*https://company.kudosnow.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-kudos-tutorial/IC787804.png "配置应用程序的 URL")

4.  在**配置单一登录在感谢**页面上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-kudos-tutorial/IC787805.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您感谢公司网站。

6.  在菜单上，单击**设置**。

    ![设置](./media/active-directory-saas-kudos-tutorial/IC787806.png "设置")

7.  单击**集成\>SSO**。

8.  在**SSO**部分中，执行以下步骤︰

    ![SSO](./media/active-directory-saas-kudos-tutorial/IC787807.png "SSO")

    1.  Azure 传统门户网站，在**配置单一登录在感谢**对话框页中的**单一登录服务 URL**值，复制，然后将其粘贴到**登录 URL**文本框。
    2.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP]
        有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    3.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**X.509 证书**文本框
    4.  在 Azure 经典门户中，在**配置单一登录在感谢**对话框页复制**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**注销到 URL**文本框。
    5.  在**您感谢 URL**文本框中，键入您的公司名称。
    6.  单击**保存**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-kudos-tutorial/IC787808.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到感谢，它们必须被调配到感谢。  
感谢，在资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**感谢**公司网站。

2.  在菜单上，单击**设置**。

    ![设置](./media/active-directory-saas-kudos-tutorial/IC787806.png "设置")

3.  单击**用户管理**。

4.  单击**用户**选项卡，然后单击**添加用户**。

    ![用户管理员](./media/active-directory-saas-kudos-tutorial/IC787809.png "用户管理员")

5.  在**添加用户**部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-kudos-tutorial/IC787810.png "添加用户")

    1.  键入**名字**、**姓氏**、**电子邮件**和其他有效的 Azure Active Directory 帐户要到相关文本框设置的详细信息。
    2.  单击**创建用户**。

>[AZURE.NOTE]您可以使用任何其他感谢用户帐户创建工具或 Api 提供的感谢来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>要将用户分配到感谢，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**感谢**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-kudos-tutorial/IC787811.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-kudos-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。