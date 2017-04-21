<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Sprinklr |Microsoft Azure" 
    description="了解如何使用 Sprinklr Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>教程︰ 与 Sprinklr 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Sprinklr 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Sprinklr 租户
  
后学完本教程，您已分配给 Sprinklr Azure AD 用户将能够到 Sprinklr 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Sprinklr
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-sprinklr-tutorial/IC782900.png "方案")

##<a name="enabling-the-application-integration-for-sprinklr"></a>启用应用程序集成为 Sprinklr
  
本部分的目的是概述如何启用应用程序集成为 Sprinklr。

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>若要启用应用程序集成为 Sprinklr，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-sprinklr-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-sprinklr-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-sprinklr-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-sprinklr-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Sprinklr**。

    ![应用程序库](./media/active-directory-saas-sprinklr-tutorial/IC782901.png "应用程序库")

7.  在结果窗格中，选择**Sprinklr**，，然后单击**完成**添加应用程序。

    ![Sprinklr](./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Sprinklr。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Sprinklr**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-sprinklr-tutorial/IC782903.png "配置单一登录")

2.  在**您想怎样来登录到 Sprinklr 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-sprinklr-tutorial/IC782904.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**Sprinklr 号中 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。 sprinklr.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-sprinklr-tutorial/IC782905.png "配置应用程序的 URL")

4.  在**配置单一登录 Sprinklr 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-sprinklr-tutorial/IC782906.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Sprinklr 公司网站。

6.  转到**管理\>设置**。

    ![管理](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "管理")

7.  转到**管理合作伙伴\>单一登录**上从左边的窗格。

    ![管理合作伙伴](./media/active-directory-saas-sprinklr-tutorial/IC782908.png "管理合作伙伴")

8.  单击**+ 添加单一登录项**。

    ![单个标记加载项](./media/active-directory-saas-sprinklr-tutorial/IC782909.png "单个标记加载项")

9.  在**单一登录**页面上，请执行以下步骤︰

    ![单个标记加载项](./media/active-directory-saas-sprinklr-tutorial/IC782910.png "单个标记加载项")

    1.  在**名称**文本框中，键入您的配置的名称 (例如︰ *WAADSSOTest*)。
    2.  选择**启用**。
    3.  选择**使用新的 SSO 证书**。
    4.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    5.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**标识提供程序证书**文本框中，
    6.  在 Azure 经典门户中，在**配置单一登录在 Sprinklr**对话框页面上，**标识提供程序 ID**值，复制，然后将其粘贴到**实体 Id**文本框。
    7.  在 Azure 经典门户中，在**配置单一登录在 Sprinklr**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**标识提供程序的登录 URL**文本框。
    8.  在 Azure 经典门户中，在**配置单一登录在 Sprinklr**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**标识提供程序注销 URL**文本框。
    9.  作为**SAML 用户 ID 类型**，选择**断言包含用户"s sprinklr.com 用户名**。
    10. 作为**SAML 用户 ID 位置**，选择**用户 ID 是主题语句的名称标识符元素中**。
    11. 关闭**保存**。

        ![SAML](./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-sprinklr-tutorial/IC782912.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
AAD 用户能够登录，他们必须为 Sprinklr 应用程序内访问设置。  
本部分介绍如何创建 AAD 内 Sprinklr 的用户帐户。

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>若要设置 Sprinklr 中的用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的 Sprinklr 公司网站。

2.  转到**管理\>设置**。

    ![管理](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "管理")

3.  转到**管理客户端\>用户**从左边的窗格。

    ![设置](./media/active-directory-saas-sprinklr-tutorial/IC782914.png "设置")

4.  单击**添加用户**。

    ![设置](./media/active-directory-saas-sprinklr-tutorial/IC782915.png "设置")

5.  在**编辑用户**对话框中，请执行以下步骤︰

    ![编辑用户](./media/active-directory-saas-sprinklr-tutorial/IC782916.png "编辑用户")

    1.  在**电子邮件****名**和**姓氏**文本框，键入 Azure AD 用户帐户的信息所需设置。
    2.  选择**禁用密码**。
    3.  选择一种**语言**。
    4.  选择**用户类型**。
    5.  单击**更新**。

    >[AZURE.IMPORTANT] **禁用密码**必须选择要使用户能够登录标识提供程序通过。

6.  进入**角色**，然后再执行以下步骤︰

    ![合作伙伴角色](./media/active-directory-saas-sprinklr-tutorial/IC782917.png "合作伙伴角色")

    1.  从**全局**列表中，选择**所有\_权限**。
    2.  单击**更新**。

>[AZURE.NOTE] 您可以使用任何其他 Sprinklr 用户帐户创建工具或 Api 提供的 Sprinklr 设置 Azure AD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>要将用户分配到 Sprinklr 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Sprinklr**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-sprinklr-tutorial/IC782918.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-sprinklr-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。