<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 SumoLogic |Microsoft Azure" 
    description="了解如何使用 SumoLogic Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>教程︰ 与 SumoLogic 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 SumoLogic 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   SumoLogic 租户
  
后学完本教程，您已分配给 SumoLogicwill 到在您的 SumoLogic 应用程序将能够为单个符号 Azure AD 用户公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 SumoLogic
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-sumologic-tutorial/IC778549.png "方案")

##<a name="enabling-the-application-integration-for-sumologic"></a>启用应用程序集成为 SumoLogic
  
本部分的目的是概述如何启用应用程序集成为 SumoLogic。

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>若要启用应用程序集成为 SumoLogic，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-sumologic-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-sumologic-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-sumologic-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-sumologic-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**sumologic**。

    ![应用程序库](./media/active-directory-saas-sumologic-tutorial/IC778550.png "应用程序库")

7.  在结果窗格中，选择**SumoLogic**，，然后单击**完成**添加应用程序。

    ![SumoLogic](./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 SumoLogic。  
作为此过程的一部分，您都需要将 base 64 编码的证书上传到您的 SumoLogictenant。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**SumoLogic**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-sumologic-tutorial/IC778552.png "配置单一登录")

2.  在**您想怎样来登录到 SumoLogic 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-sumologic-tutorial/IC778553.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**SumoLogic 号中 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。SumoLogic.com*"，然后单击**下一步**。

    ![配置 aoo URL](./media/active-directory-saas-sumologic-tutorial/IC778554.png "配置 aoo URL")

4.  在**配置单一登录 SumoLogic 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-sumologic-tutorial/IC778555.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 SumoLogic 公司网站。

6.  转到**管理\>安全**。

    ![管理](./media/active-directory-saas-sumologic-tutorial/IC778556.png "管理")

7.  单击**SAML**。

    ![全局安全设置](./media/active-directory-saas-sumologic-tutorial/IC778557.png "全局安全设置")

8.  **选择一种配置，或创建一个新**列表中，选择**Azure 的广告**，，然后单击**配置**。

    ![配置 SAML 2.0](./media/active-directory-saas-sumologic-tutorial/IC778558.png "配置 SAML 2.0")

9.  在**配置 SAML 2.0**对话框中，请执行以下步骤︰

    ![配置 SAML 2.0](./media/active-directory-saas-sumologic-tutorial/IC778559.png "配置 SAML 2.0")

    1.  在**配置名称**文本框中，键入**Azure 的广告**。
    2.  选择**调试模式**。
    3.  在 Azure 经典门户中，在**配置单一登录在 SumoLogic**的对话页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框。
    4.  在 Azure 经典门户中，在**配置单一登录在 SumoLogic**的对话页上，将**身份验证请求 URL**值，复制，然后将其粘贴到的**身份验证请求 URL**文本框。
    5.  创建您已下载的证书的**Base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    6.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将整个证书粘贴到文本框**的 X.509 证书**。
    7.  作为**电子邮件属性**，选择**使用 SAML 主题**。
    8.  选择**SP 启动登录配置**。
    9.  在**登录路径**文本框中，键入**Azure**。
    10. 单击**保存**。

10. 在 Azure 经典门户中，在**配置单一登录在 SumoLogic**的对话页上，选择单一登录配置确认，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-sumologic-tutorial/IC778560.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 SumoLogic，他们必须为 SumoLogic 配置。  
对于 SumoLogic，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  登录到**SumoLogic**租户。

2.  转到**管理\>用户**。

    ![用户](./media/active-directory-saas-sumologic-tutorial/IC778561.png "用户")

3.  单击**添加**。

    ![用户](./media/active-directory-saas-sumologic-tutorial/IC778562.png "用户")

4.  在**新建用户**对话框中，请执行以下步骤︰

    ![新用户](./media/active-directory-saas-sumologic-tutorial/IC778563.png "新用户")

    1.  键入要设置到**名字**、**姓氏**和**电子邮件**文本框的 Azure 的广告帐户的相关的信息。
    2.  选择一个角色。
    3.  作为**状态**，选择**活动**。
    4.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 SumoLogic 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 SumoLogic 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>要将用户分配到 SumoLogic 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**SumoLogic**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-sumologic-tutorial/IC778564.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-sumologic-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。