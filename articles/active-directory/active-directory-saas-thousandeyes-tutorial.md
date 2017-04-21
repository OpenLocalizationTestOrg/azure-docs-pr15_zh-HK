<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 ThousandEyes |Microsoft Azure" 
    description="了解如何使用 ThousandEyes Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>教程︰ 与 ThousandEyes 的 Azure Active Directory 集成
  
本教程的目的是显示如何将 Azure 活动目录 (AD Azure) 和 ThousandEyes 之间的单一登录设置。
  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在已启用的订阅单个 ThousandEyes 号
  
完成本教程后, 必须向其分配访问 ThousandEyes 的 AAD 用户将能够到 ThousandEyes 公司网站 （服务提供程序初始化登录），或使用 AAD 访问权限面板应用程序的单一登录。

1.  启用应用程序集成为 ThousandEyes
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-thousandeyes-tutorial/IC790059.png "方案")

##<a name="enabling-the-application-integration-for-thousandeyes"></a>启用应用程序集成为 ThousandEyes
  
本部分的目的是概述如何启用应用程序集成为 ThousandEyes。

###<a name="to-enable-the-application-integration-for-thousandeyes-perform-the-following-steps"></a>若要启用应用程序集成为 ThousandEyes，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-thousandeyes-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-thousandeyes-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-thousandeyes-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-thousandeyes-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**ThousandEyes**。

    ![应用程序库](./media/active-directory-saas-thousandeyes-tutorial/IC790060.png "应用程序库")

7.  在结果窗格中，选择**ThousandEyes**，，然后单击**完成**添加应用程序。

    ![ThousandEyes](./media/active-directory-saas-thousandeyes-tutorial/IC790061.png "ThousandEyes")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
这一节概述如何使用户能够与他们 Azure Active Directory，使用联合身份验证基于 SAML 协议中的帐户验证到 ThousandEyes。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**ThousandEyes**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-thousandeyes-tutorial/IC790062.png "配置单一登录")

2.  在**您想怎样来登录到 ThousandEyes 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-thousandeyes-tutorial/IC790063.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**ThousandEyes 号上的 URL**文本框中，使用的 URL 的用户登录到您的 ThousandEyes 应用程序的类型 (例如:"*https://app.thousandeyes.com/login/sso*")，然后单击**下一步**。 

    ![配置应用程序的 URL](./media/active-directory-saas-thousandeyes-tutorial/IC790064.png "配置应用程序的 URL")

4.  在**配置单一登录 ThousandEyes 在**页上，下载您的证书，单击**下载证书**，然后将证书文件本地保存到您的计算机。

    ![配置单一登录](./media/active-directory-saas-thousandeyes-tutorial/IC790065.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，登录到您的**ThousandEyes**公司网站作为管理员。

6.  在菜单上，单击**设置**。

    ![设置](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "设置")

7.  单击**帐户**

    ![帐户](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "帐户")

8.  单击**安全和身份验证**选项卡。

    ![安全性和身份验证](./media/active-directory-saas-thousandeyes-tutorial/IC790068.png "安全性和身份验证")

9.  在**安装单一登录**部分中，执行以下步骤︰

    ![单一登录设置](./media/active-directory-saas-thousandeyes-tutorial/IC790069.png "单一登录设置")

    1.  选择**启用单一登录**。
    2.  在 Microsoft Azure 经典门户中，**配置单一登录 ThousandEyes 在**页上复制的**远程登录 URL**值，，然后将其粘贴到**登录页的 URL**文本框。
    3.  在 Microsoft Azure 经典门户中，**配置单一登录 ThousandEyes 在**页上复制的**远程注销 URL**值，然后将其粘贴到**注销页 URL**文本框。
    4.  在 Microsoft Azure 经典门户中，在**配置单一登录 ThousandEyes 在**页上，将**颁发者 URL**值，复制，然后将其粘贴到**标识提供商颁发者**文本框。
    5.  **标识提供程序证书**，单击**选择文件**，然后上载您从 Microsoft Azure 经典门户下载证书。
    6.  单击**保存**。

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-thousandeyes-tutorial/IC790070.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 ThousandEyes，他们必须到 ThousandEyes 配置。  
对于 ThousandEyes，资源调配是手动任务。

###<a name="to-provision-a-user-account-to-thousandeyes-perform-the-following-steps"></a>若要设置到 ThousandEyes 的用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的 ThousandEyes 公司网站。

2.  单击**设置**。

    ![设置](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "设置")

3.  单击**帐户**。

    ![帐户](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "帐户")

4.  单击**帐户和用户**选项卡。

    ![帐户和用户](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "帐户和用户")

5.  在**添加用户和帐户**部分中，执行以下步骤︰

    ![添加用户帐户](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "添加用户帐户")

    1.  键入**名称**、**电子邮件**和其他有效的 Azure Active Directory 帐户要到相关文本框设置的详细信息。
    2.  单击**添加新用户帐户**。

        >[AZURE.NOTE] AAD 帐户持有人将获得包括链接确认并激活该帐户的电子邮件。

>[AZURE.NOTE] 您可以使用任何其他 ThousandEyes 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 ThousandEyes 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-thousandeyes-perform-the-following-steps"></a>要将用户分配到 ThousandEyes 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**ThousandEyes**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-thousandeyes-tutorial/IC790075.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-thousandeyes-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
