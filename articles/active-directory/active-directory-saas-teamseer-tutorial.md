<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 TeamSeer |Microsoft Azure" 
    description="了解如何使用 TeamSeer Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>教程︰ 与 TeamSeer 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 TeamSeer 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   TeamSeer 租户
  
后学完本教程，您已分配给 TeamSeer Azure AD 用户将能够到 TeamSeer 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 TeamSeer
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-teamseer-tutorial/IC789618.png "方案")

##<a name="enabling-the-application-integration-for-teamseer"></a>启用应用程序集成为 TeamSeer
  
本部分的目的是概述如何启用应用程序集成为 TeamSeer。

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>若要启用应用程序集成为 TeamSeer，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-teamseer-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-teamseer-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-teamseer-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-teamseer-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**TeamSeer**。

    ![应用程序库](./media/active-directory-saas-teamseer-tutorial/IC789619.png "应用程序库")

7.  在结果窗格中，选择**TeamSeer**，，然后单击**完成**添加应用程序。

    ![TeamSeer](./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 TeamSeer。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**TeamSeer**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-teamseer-tutorial/IC789621.png "配置单一登录")

2.  在**您想怎样来登录到 TeamSeer 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-teamseer-tutorial/IC789628.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**TeamSeer 号中 URL**文本框中，键入您的 URL 使用以下模式"*http://www.teamseer.com/companyid*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-teamseer-tutorial/IC789629.png "配置应用程序的 URL")

4.  在**配置单一登录 TeamSeer 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-teamseer-tutorial/IC789630.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 TeamSeer 公司网站。

6.  转到**人力资源管理**。

    ![人力资源管理](./media/active-directory-saas-teamseer-tutorial/IC789634.png "人力资源管理")

7.  单击**设置**。

    ![安装程序](./media/active-directory-saas-teamseer-tutorial/IC789635.png "安装程序")

8.  单击**设置 SAML 提供详细信息**。

    ![SAML 设置](./media/active-directory-saas-teamseer-tutorial/IC789636.png "SAML 设置")

9.  在 SAML 提供程序的详细信息部分中，执行以下步骤︰

    ![SAML 设置](./media/active-directory-saas-teamseer-tutorial/IC789637.png "SAML 设置")

    1.  在 Azure 经典门户中，在**配置单一登录在 TeamSeer**对话框页面上，**单一登录服务 URL**值，复制，然后将其粘贴到**URL**文本框。
    2.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    3.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**IdP 公共证书**文本框。

10. 若要完成 SAML 提供程序配置，请执行以下步骤︰

    ![SAML 设置](./media/active-directory-saas-teamseer-tutorial/IC789638.png "SAML 设置")

    1.  在**测试电子邮件地址**中，键入测试用户的电子邮件地址。
    2.  在**颁发者**文本框中，键入服务提供商颁发者 URL。
    3.  单击**保存**。

11. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-teamseer-tutorial/IC789639.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 TeamSeer，他们必须到 ShiftPlanning 配置。  
对于 TeamSeer，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**TeamSeer**公司网站。

2.  执行以下步骤︰

    ![人力资源管理](./media/active-directory-saas-teamseer-tutorial/IC789640.png "人力资源管理")

    1.  转到**人力资源管理\>用户**。
    2.  单击**运行向导新建用户**。

3.  在**用户详细信息**部分中，执行以下步骤︰

    ![用户详细信息](./media/active-directory-saas-teamseer-tutorial/IC789641.png "用户详细信息")

    1.  键入要到相关文本框设置有效的 AAD 帐户的**名字**，**姓氏**，**用户名 （电子邮件地址）** 。
    2.  单击**下一步**。

4.  请按照屏幕上的说明添加一个新用户，然后单击**完成**。

>[AZURE.NOTE] 您可以使用任何其他 TeamSeer 用户帐户创建工具或 Api 提供的 TeamSeer 设置 Azure AD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>要将用户分配到 TeamSeer 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**TeamSeer**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-teamseer-tutorial/IC789642.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-teamseer-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。