<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Thirdlight |Microsoft Azure" 
    description="了解如何使用 Thirdlight Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>教程︰ 与 Thirdlight 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Thirdlight 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Thirdlight-启用单一登录的订阅
  
后学完本教程，您已分配给 Thirdlight Azure AD 用户将能够到 Thirdlight 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Thirdlight
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-thirdlight-tutorial/IC805836.png "方案")

##<a name="enabling-the-application-integration-for-thirdlight"></a>启用应用程序集成为 Thirdlight
  
本部分的目的是概述如何启用应用程序集成为 Thirdlight。

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>若要启用应用程序集成为 Thirdlight，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-thirdlight-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-thirdlight-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-thirdlight-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-thirdlight-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Thirdlight**。

    ![应用程序库](./media/active-directory-saas-thirdlight-tutorial/IC805837.png "应用程序库")

7.  在结果窗格中，选择**Thirdlight**，，然后单击**完成**添加应用程序。

    ![ThirdLight](./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Thirdlight。  
Thirdlight 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Thirdlight**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-thirdlight-tutorial/IC805839.png "配置单一登录")

2.  在**您想怎样来登录到 Thirdlight 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-thirdlight-tutorial/IC805840.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**Thirdlight 号中 URL**文本框中，键入用户用于登录到您的 Thirdlight 应用程序 URL (例如:"*http://azuresso2.thirdlight.com/*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-thirdlight-tutorial/IC805841.png "配置应用程序的 URL")

4.  在**配置单一登录 Thirdlight 在**页上，要下载元数据，请单击**下载元数据**，然后保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-thirdlight-tutorial/IC805842.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Thirdlight 公司网站。

6.  转到**配置\>系统管理**，然后单击**SAML2**。

    ![系统管理](./media/active-directory-saas-thirdlight-tutorial/IC805843.png "系统管理")

7.  在 SAML2 配置部分中，执行以下步骤︰

    ![SAML 单一登录](./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML 单一登录")

    1.  选择**启用 SAML2 单一登录**。
    2.  作为**IdP 元数据的源**，选择**从 XML 加载 IdP 元数据**。
    3.  打开下载元数据文件，复制的内容，然后将其粘贴到**IdP 元数据 XML**文本框。
    4.  单击**保存 SAML2 设置**。

8.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-thirdlight-tutorial/IC805845.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Thirdlight，他们必须到 Thirdlight 配置。  
对于 Thirdlight，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Thirdlight**公司网站。

2.  转到**用户**选项卡。

3.  选择**用户和组**。

4.  单击**添加新用户**按钮。

5.  输入**用户名、 名称或描述、 电子邮件、 选择预设或组的新成员**有效 AAD 帐户所需的规定。

6.  单击**创建**。

>[AZURE.NOTE] 您可以使用任何其他 Thirdlight 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Thirdlight 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>要将用户分配到 Thirdlight 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Thirdlight**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-thirdlight-tutorial/IC805846.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-thirdlight-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。