<properties 
    pageTitle="教程︰ 使用 Igloo 软件的 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Igloo 软件启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>教程︰ 使用 Igloo 软件的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Igloo 软件的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   [Igloo 软件](http://www.igloosoftware.com/)单一登录启用订阅
  
学完本教程后, 已分配给 Igloo 软件的 Azure AD 用户将能够到在 Igloo 软件公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用 Igloo 软件的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-igloo-software-tutorial/IC783961.png "方案")
##<a name="enabling-the-application-integration-for-igloo-software"></a>启用 Igloo 软件的应用程序集成
  
本部分的目的是概述如何启用 Igloo 软件的应用程序集成。

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>若要启用应用程序集成的 Igloo 软件，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-igloo-software-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-igloo-software-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-igloo-software-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-igloo-software-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Igloo 软件**。

    ![应用程序库](./media/active-directory-saas-igloo-software-tutorial/IC783962.png "应用程序库")

7.  在结果窗格中，选择**Igloo 软件**，然后单击**完成**添加应用程序。

    ![Igloo](./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Igloo 软件。  
作为此过程的一部分，您都需要将 base 64 编码的证书上传到中央桌面租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Igloo 软件**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-igloo-software-tutorial/IC783964.png "配置单一登录")

2.  在**您想怎样来登录到 Igloo 软件的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![Microsoft Azure 广告单登录](./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure 广告单登录")

3.  在**配置应用程序 URL**页上，在**Igloo 软件符号在 URL**文本框中，键入您的 URL 使用以下模式"*https://company.igloocommunities.com/?signin*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-igloo-software-tutorial/IC773625.png "配置应用程序的 URL")

4.  在**配置单一登录 Igloo 软件**页面上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-igloo-software-tutorial/IC783966.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Igloo 软件公司网站。

6.  请转到**控制面板**。

    ![控制面板](./media/active-directory-saas-igloo-software-tutorial/IC799949.png "控制面板")

7.  在**成员资格**选项卡，单击**签名中设置**。

    ![登录设置](./media/active-directory-saas-igloo-software-tutorial/IC783968.png "登录设置")

8.  在 SAML 配置部分中，单击**配置 SAML 身份验证**。

    ![SAML 配置](./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML 配置")

9.  在**常规配置**部分中，执行以下步骤︰

    ![常规配置](./media/active-directory-saas-igloo-software-tutorial/IC783970.png "常规配置")

    1.  在**连接名称**文本框中，键入您的配置的自定义名称。
    2.  在 Azure 经典门户中，在**配置单一登录 Igloo 软件**的对话页上的**远程登录 URL**值，复制，然后将其粘贴到**IdP 登录 URL**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录 Igloo 软件**的对话页上，**远程注销 URL**值，复制，然后将其粘贴到**IdP 注销 URL**文本框。
    4.  作为**注销响应和请求 HTTP 类型**，选择**公告**。
    5.  从下载证书创建一个文本文件。
        
        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    6.  从您的证书的文字文件版本中删除的第一行和最后一行，复制剩余的证书文本，然后将其粘贴到**公用证书**文本框。

10. 在**响应和身份验证配置**，请执行以下步骤︰

    ![响应和身份验证配置](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "响应和身份验证配置")

    1.  为**身份标识提供程序**，请选择**Microsoft ADF**。
    2.  作为**标识符类型**，选择**电子邮件地址**。
    3.  在**电子邮件属性**文本框中，键入**电子邮件地址**。
    4.  在**第一个名称属性**文本框中，键入**givenname**。
    5.  在**最后的名称属性**文本框中，键入**姓**。

11. 执行以下步骤以完成配置︰

    ![在登录的用户创建](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "在登录的用户创建")

    1.  **在登录的用户创建**，选择**创建新的用户在您的网站在他们登录时**。
    2.  **登录设置**，选择**"登录"屏幕上的使用 SAML 按钮**。
    3.  单击**保存**。

12. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-igloo-software-tutorial/IC783973.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
没有为您配置 Igloo 软件为用户提供操作项。  
当分派的用户尝试登录到使用访问权限面板中的 Igloo 软件时，Igloo 软件检查用户是否存在。  
如果没有用户帐户可还，它是自动创建的 Igloo 软件。
##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>若要将用户分配到 Igloo 软件，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Igloo 软件**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-igloo-software-tutorial/IC783974.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-igloo-software-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。