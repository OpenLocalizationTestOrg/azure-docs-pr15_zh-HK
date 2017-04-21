<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Lucidchart |Microsoft Azure" 
    description="了解如何使用 Lucidchart Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>教程︰ 与 Lucidchart 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Lucidchart 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Lucidchart-启用单一登录的订阅
  
后学完本教程，您已分配给 Lucidchart Azure AD 用户将能够到 Lucidchart 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Lucidchart
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-lucidchart-tutorial/IC791183.png "方案")
##<a name="enabling-the-application-integration-for-lucidchart"></a>启用应用程序集成为 Lucidchart
  
本部分的目的是概述如何启用应用程序集成为 Lucidchart。

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>若要启用应用程序集成为 Lucidchart，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-lucidchart-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-lucidchart-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-lucidchart-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-lucidchart-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Lucidchart**。

    ![应用程序库](./media/active-directory-saas-lucidchart-tutorial/IC791184.png "应用程序库")

7.  在结果窗格中，选择**Lucidchart**，，然后单击**完成**添加应用程序。

    ![Lucidchart](./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Lucidchart。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Lucidchart**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-lucidchart-tutorial/IC791186.png "配置单一登录")

2.  在**您想怎样来登录到 Lucidchart 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-lucidchart-tutorial/IC791187.png "配置单一登录")

3.  在**配置应用程序 URL**页面上的**Lucidchart 号上的 URL**文本框中，键入用户用于登录到您的 Lucidchart 应用程序的 URL (例如:"*https://chart2.office.lucidchart.com/saml/sso/azure*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-lucidchart-tutorial/IC791188.png "配置应用程序的 URL")

4.  在**配置单一登录 Lucidchart 在**页面上，要下载元数据，请单击**下载元数据**，然后保存在您的计算机上的本地数据文件。

    ![配置单一登录](./media/active-directory-saas-lucidchart-tutorial/IC791189.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Lucidchart 公司网站。

6.  在菜单上，单击**工作组**。

    ![团队](./media/active-directory-saas-lucidchart-tutorial/IC791190.png "团队")

7.  单击**应用程序\>管理 SAML**。

    ![管理 SAML](./media/active-directory-saas-lucidchart-tutorial/IC791191.png "管理 SAML")

8.  在**SAML 验证设置**对话框页面上，请执行以下步骤︰

    1.  选择**启用 SAML 身份验证**，然后单击**可选**。
        ![SAML 身份验证设置](./media/active-directory-saas-lucidchart-tutorial/IC791192.png "SAML 身份验证设置")
    2.  在**域**文本框中，键入您的域，然后单击**更改证书**。
        ![更改证书](./media/active-directory-saas-lucidchart-tutorial/IC791193.png "更改证书")
    3.  打开下载元数据文件，复制的内容，然后将其粘贴到文本框中**上载的元数据**。
        ![上载的元数据](./media/active-directory-saas-lucidchart-tutorial/IC791194.png "上载的元数据")
    4.  选择**自动将新用户添加到组**，然后单击**保存更改**。
        ![保存更改](./media/active-directory-saas-lucidchart-tutorial/IC791195.png "保存更改")

9.  选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-lucidchart-tutorial/IC791196.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
没有为您配置用户供应到 Lucidchart 操作项。  
当分派的用户尝试登录到使用访问权限面板中的 Lucidchart 时，Lucidchart 将检查用户是否存在。  
如果没有用户帐户可还，它是自动创建的 Lucidchart。
##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>要将用户分配到 Lucidchart 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Lucidchart**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-lucidchart-tutorial/IC791197.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-lucidchart-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。