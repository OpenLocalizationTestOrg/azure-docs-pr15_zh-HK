<properties 
    pageTitle="教程︰ 与 Cisco Webex 的 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Cisco Webex 启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>教程︰ 与 Cisco Webex 的 Azure Active Directory 集成

本教程的目的是显示 Azure 和 Cisco Webex 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Cisco Webex 租户

后学完本教程，您已分配给 Cisco Webex Azure AD 用户将能够到 Cisco Webex 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用 Cisco Webex 应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "方案")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>启用 Cisco Webex 应用程序集成

本部分的目的是概述如何启用 Cisco Webex 应用程序的集成。

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>若要启用 Cisco Webex 应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Cisco Webex**。

    ![应用程序库](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "应用程序库")

7.  在结果窗格中，选中**Cisco Webex**，，然后单击**完成**添加应用程序。

    ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Cisco Webex。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 经典门户中， **Cisco Webex**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "配置单一登录")

2.  在**您想怎样来登录到 Cisco Webex 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "配置单一登录")

3.  在**配置应用程序 URL**页上，执行下面的步骤，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "配置应用程序的 URL")

    1.  在**唱歌在 URL**文本框中，键入您的 Cisco Webex 租户 URL (例如︰ *http://contoso.webex.com*)。
    2.  在**Cisco Webex 回复 URL**文本框中，键入您的**Cisco Webex AssertionConsumerService URL** (例如︰ *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*)。

4.  在**配置单一登录 Cisco Webex 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Cisco Webex 公司网站。

6.  在菜单上，单击**网站管理**。

    ![网站管理](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "网站管理")

7.  在**管理网站**部分中，单击**SSO 配置**。

    ![SSO 配置](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO 配置")

8.  在联合 Web SSO 配置部分中，执行以下步骤︰

    ![联合 SSO 配置](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "联合 SSO 配置")

    1.  从**联合身份验证协议**列表中，选择**SAML 2.0**。
    2.  创建您已下载的证书的**Base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    3.  在记事本中打开您 base 64 编码的证书，然后复制其内容。
    4.  **导入 SAML 元数据**，请单击，然后粘贴 base 64 编码的证书。
    5.  在 Azure 经典门户中，在**配置单一登录在 Cisco Webex**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者作为 SAML (IdP ID)**文本框。
    6.  在 Azure 经典门户中，在**配置单一登录在 Cisco Webex**对话框页面上，复制的**远程登录 URL**值，，然后将其粘贴到**客户 SSO 服务登录 URL**文本框。
    7.  **NameID 格式**列表中，选择**电子邮件地址**。
    8.  在**AuthnContextClassRef**文本框中，键入**urn: oasis︰ 姓名︰ tc: SAML:2.0:ac:classes:Password**。
    9.  在 Azure 经典门户中，在**配置单一登录在 Cisco Webex**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**客户 SSO 服务注销 URL**文本框。
    10. 单击**更新**。

9.  在 Azure 经典门户中，在**配置单一登录在 Cisco Webex**对话框页面上，选择单一登录配置进行确认，，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 Cisco Webex，他们必须到 Cisco Webex 调配。  
在 Cisco Webex 资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  登录到**Cisco Webex**租户。

2.  转到**管理用户\>添加用户**。

    ![添加用户](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "添加用户")

3.  在添加用户部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "添加用户")

    1.  作为**帐户类型**，选择**主机**。
    2.  在下面的文本框中键入现有 Azure AD 用户的信息︰**第一名、 姓**、**用户名**、**电子邮件**、**密码**、**确认密码**。
    3.  单击**添加**。

>[AZURE.NOTE] 您可以使用任何其他 Cisco Webex 用户帐户创建工具或 Api 提供的 Cisco Webex 来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>要将用户分配到 Cisco Webex，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Cisco Webex**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
