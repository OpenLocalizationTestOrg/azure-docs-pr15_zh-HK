<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Replicon |Microsoft Azure" 
    description="了解如何使用 Replicon Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-replicon"></a>教程︰ 与 Replicon 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Replicon 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Replicon 租户
  
后学完本教程，您已分配给 Replicon Azure AD 用户将能够到 Replicon 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Replicon
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-replicon-tutorial/IC777798.png "方案")
##<a name="enabling-the-application-integration-for-replicon"></a>启用应用程序集成为 Replicon
  
本部分的目的是概述如何启用应用程序集成为 Replicon。

###<a name="to-enable-the-application-integration-for-replicon-perform-the-following-steps"></a>若要启用应用程序集成为 Replicon，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-replicon-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-replicon-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-replicon-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Replicon**。

    ![应用程序库](./media/active-directory-saas-replicon-tutorial/IC777799.png "应用程序库")

7.  在结果窗格中，选择**Replicon**，，然后单击**完成**添加应用程序。

    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Replicon。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Replicon**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-replicon-tutorial/IC777801.png "配置单一登录")

2.  在**您想怎样来登录到 Replicon 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-replicon-tutorial/IC777802.png "配置单一登录")

3.  在**配置应用程序 URL**页上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "配置应用程序的 URL")

    1.  在**Replicon 号上的 URL**文本框中，键入 Replicon 租户 URL (例如︰ *https://na2.replicon.com/company/saml2/sp-sso/post*)。
    2.  在**Replicon 回复 URL**文本框中，键入 Replicon **AssertionConsumerService** URL (例如︰ *https://global.replicon.com/ ！ / saml2/公司/sso/后*)。  

        >[AZURE.NOTE] 可以从位于 Replicon 的元数据中获取 URL:         **https://global.replicon.com/ ！ /saml2/\<YourCompanyKey\>**。

    3.  单击**下一步**

4.  在**配置单一登录 Replicon 在**页面上，要下载元数据，请单击**下载元数据**，然后将元数据保存在您的计算机上。

    ![配置单一登录](./media/active-directory-saas-replicon-tutorial/IC777804.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Replicon 公司网站。

6.  若要配置 SAML 2.0，请执行以下步骤︰

    ![SAML 启用身份验证](./media/active-directory-saas-replicon-tutorial/IC777805.png "SAML 启用身份验证")

    1.  若要显示**EnableSAML Authentication2**对话框中，追加到您的 URL，您公司的密钥后以下︰  
        **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
        下面显示的架构的完整 URL:  
        **https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
    2.  单击**+**展开**v20Configuration**部分。
    3.  单击**+**展开**metaDataConfiguration**部分。
    4.  单击**选择文件**以选择您的标识提供程序元数据的 XML 文件，然后单击**提交**。

7.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-replicon-tutorial/IC778418.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Replicon，他们必须到 Replicon 配置。  
对于 Replicon，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  在 web 浏览器窗口中，以管理员身份登录到 Replicon 公司网站。

2.  转到**管理\>用户**。

    ![用户](./media/active-directory-saas-replicon-tutorial/IC777806.png "用户")

3.  单击**+ 添加用户**。

    ![添加用户](./media/active-directory-saas-replicon-tutorial/IC777807.png "添加用户")

4.  在**用户配置文件**部分中，执行以下步骤︰

    ![用户配置文件](./media/active-directory-saas-replicon-tutorial/IC777808.png "用户配置文件")

    1.  在**登录名**文本框中，键入您要提供的 Azure AD 用户的 Azure 的广告电子邮件地址。
    2.  作为**身份验证类型**，请选择**SSO**。
    3.  在**部门**文本框中，键入用户的部门。
    4.  作为**员工类型**，选择**管理员**。
    5.  单击**保存用户配置文件**。

>[AZURE.NOTE]您可以使用任何其他 Replicon 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Replicon 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-replicon-perform-the-following-steps"></a>要将用户分配到 Replicon 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Replicon**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-replicon-tutorial/IC777809.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-replicon-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。