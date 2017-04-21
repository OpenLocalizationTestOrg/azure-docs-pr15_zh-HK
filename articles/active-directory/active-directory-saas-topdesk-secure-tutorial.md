<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 TOPdesk-安全 |Microsoft Azure"
    description="了解如何使用 TOPdesk-通过 Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多安全保障 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>教程︰ 与 TOPdesk-安全的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 TOPdesk-安全的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   A TOPdesk-安全-启用单一登录的订阅
  
学完本教程后, 将单一登录到应用程序在您的 TOPdesk-安全公司网站 （服务提供程序初始化登录），或使用[简介接入面板](active-directory-saas-access-panel-introduction.md)能够 Azure AD 用户已分配给 TOPdesk 的安全。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 TOPdesk-安全
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "方案")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>启用应用程序集成为 TOPdesk-安全
  
本部分的目的是概述如何启用 TOPdesk 的安全应用程序的集成。

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>若要启用应用程序集成为 TOPdesk-安全，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**TOPdesk-安全**。

    ![应用程序库](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "应用程序库")

7.  在结果窗格中，选择**TOPdesk-安全**，，然后单击**完成**添加应用程序。

    ![TOPdesk-安全](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk-安全")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够通过身份验证的 TOPdesk-在 Azure AD 使用基于 SAML 协议联合使用他们的帐户安全。  
配置单一登录的 TOPdesk-安全要求您上载徽标图标文件。 若要获取该图标文件，请与 TOPdesk 的支持团队。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  以管理员身份登录到您的**TOPdesk-安全**公司网站。

2.  在**TOPdesk**菜单上，单击**设置**。

    ![设置](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "设置")

3.  单击**登录设置**。

    ![登录设置](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "登录设置")

4.  展开**登录设置**菜单，然后单击**常规**。

    ![常规](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "常规")

5.  在**SAML 登录**配置节**安全**部分中，执行以下步骤︰

    ![技术设置](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "技术设置")

    1.  单击**下载**以下载公共元数据文件，然后在您的计算机上本地保存。
    2.  打开元数据文件，然后找到**AssertionConsumerService**节点。
        ![断言使用者服务](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "断言使用者服务")
    3.  复制的**AssertionConsumerService**值。  

        >[AZURE.NOTE] 在本教程后面部分，您将需要**配置应用程序 URL**部分中的值。

6.  在不同的 web 浏览器窗口中，以管理员身份登录到**Azure 的传统门户网站**。

7.  在**TOPdesk 的安全**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "配置单一登录")

8.  在**您想怎样来登录到 TOPdesk 的安全的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "配置单一登录")

9.  在**配置应用程序 URL**页上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "配置应用程序的 URL")

    1.  在**TOPdesk-安全符号在 URL**文本框中，键入用户用于登录到您的 TOPdesk 的安全应用程序的 URL (例如:"*https://qssolutions.topdesk.net*")。
    2.  在**TOPdesk – 公共回复 URL**文本框中，粘贴**TOPdesk-安全的 AssertionConsumerService URL** (例如:"*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  单击**下一步**。

10. 在**配置单一登录在 TOPdesk-安全**页中，要下载元数据文件，单击**下载元数据**，然后保存在您的计算机上的本地文件。

    ![配置单一登录](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "配置单一登录")

11. 若要创建一个证书文件，请执行以下步骤︰

    ![证书](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "证书")

    1.  打开下载元数据文件。
    2.  展开的**xsi: type**的**RoleDescriptor**节点**纸︰ ApplicationServiceType**。
    3.  复制的**x509 证书**节点的值。
    4.  本地保存到计算机上的文件中复制的**x509 证书**值。

12. 在您的 TOPdesk-安全公司网站，在**TOPdesk**菜单上，单击**设置**。

    ![设置](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "设置")

13. 单击**登录设置**。

    ![登录设置](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "登录设置")

14. 展开**登录设置**菜单，然后单击**常规**。

    ![常规](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "常规")

15. 在**公用**部分，单击**添加**。

    ![添加](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "添加")

16. 在**SAML 配置助手**对话框页面上，请执行以下步骤︰

    ![SAML 配置助手](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML 配置助手")

    1.  要上载下载元数据文件下**联合元数据**，, 请单击**浏览**。
    2.  要上载您的证书文件下**证书 (RSA)**，, 请单击**浏览**。
    3.  将从 TOPdesk 获得徽标文件上载支持团队下**徽标图标**，单击**浏览。**
    4.  在**用户名属性**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**。
    5.  在**显示名称**文本框中，键入您的配置的名称。
    6.  单击**保存**。

17. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 TOPdesk 的安全，他们必须被调配到 TOPdesk 的安全。  
TOPdesk-在安全的、 资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**TOPdesk-安全**公司网站。

2.  在菜单上，单击**TOPdesk \> New\>支持文件\>运算符**。

    ![运算符](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "运算符")

3.  在**新建操作员**对话框中，请执行以下步骤︰

    ![New 运算符](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New 运算符")

    1.  单击常规选项卡。
    2.  在**常规**部分中的**姓氏**文本框中，键入您要提供一个有效的 Azure Active Directory 帐户的姓氏。
    3.  在**位置**部分中选择该帐户的**站点**。
    4.  在**TOPdesk 登录**部分的**登录名**文本框中，键入用户的登录名。
    5.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 TOPdesk-安全用户帐户创建工具或 Api 提供的 TOPdesk-安全地调配 AAD 的用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>若要将用户分配到 TOPdesk 的安全，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**TOPdesk 的安全**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。