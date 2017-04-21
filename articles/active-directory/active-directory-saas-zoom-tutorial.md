<properties 
    pageTitle="教程︰ Azure Active Directory 集成与缩放 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 缩放以启用单一登录、 自动化资源调配，以及更多。" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>教程︰ Azure Active Directory 集成与缩放
  
本教程的目的是显示 Azure 和缩放的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   缩放组织
  
后学完本教程，您已分配给缩放 Azure AD 用户将能够到缩放公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用缩放应用程序的集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-zoom-tutorial/IC784693.png "方案")

##<a name="enabling-the-application-integration-for-zoom"></a>启用缩放应用程序的集成
  
本部分的目的是概述如何启用缩放应用程序的集成。

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>若要启用缩放应用程序的集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-zoom-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-zoom-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-zoom-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-zoom-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**显示比例**。

    ![应用程序库](./media/active-directory-saas-zoom-tutorial/IC784694.png "应用程序库")

7.  在结果窗格中，选择**显示比例**，，然后单击**完成**添加应用程序。

    ![显示比例](./media/active-directory-saas-zoom-tutorial/IC784695.png "显示比例")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到缩放。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**缩放**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-zoom-tutorial/IC784696.png "配置单一登录")

2.  在**如何用户可以登录到缩放**页面上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-zoom-tutorial/IC784697.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**缩放符号在 URL**文本框中，键入您的 URL 使用以下模式"*http://company.zoom.us*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-zoom-tutorial/IC784698.png "配置应用程序的 URL")

4.  在**配置单一登录在缩放**页面上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-zoom-tutorial/IC784699.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您缩放公司网站。

6.  单击**单一登录**选项卡。

    ![单一登录](./media/active-directory-saas-zoom-tutorial/IC784700.png "单一登录")

7.  **安全控件**选项卡，然后转到**单一登录**设置。

8.  在单一登录部分中，执行以下步骤︰

    ![单一登录](./media/active-directory-saas-zoom-tutorial/IC784701.png "单一登录")

    1.  在 Azure 经典门户中，在**配置单一登录在缩放**对话框页面上，**单一登录服务 URL**值，复制，然后将其粘贴到**登录页的 URL**文本框。
    2.  在 Azure 经典门户中，在**配置单一登录在缩放**对话框页面上，复制**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**注销页 URL**文本框。
    3.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**标识提供程序证书**文本框
    5.  在 Azure 经典门户中，在**配置单一登录在缩放**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框。
    6.  单击**保存**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-zoom-tutorial/IC784702.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到缩放，他们必须到缩放设置。  
在缩放，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您**缩放**的公司网站。

2.  单击**帐户管理**选项卡，然后单击**用户管理**。

3.  在用户管理部分中，单击**添加用户**。

    ![用户管理](./media/active-directory-saas-zoom-tutorial/IC784703.png "用户管理")

4.  在**添加用户**页中，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-zoom-tutorial/IC784704.png "添加用户")

    1.  作为**用户类型**，选择**基本**。
    2.  在**电子邮件**文本框中，键入您要提供一个有效的 AAD 帐户的电子邮件地址。
    3.  单击**添加**。

>[AZURE.NOTE] 您可以使用任何其他缩放用户帐户创建工具或 Api 提供的缩放设置 AAD 的用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>若要将用户分配到缩放，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**缩放**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-zoom-tutorial/IC784705.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-zoom-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。