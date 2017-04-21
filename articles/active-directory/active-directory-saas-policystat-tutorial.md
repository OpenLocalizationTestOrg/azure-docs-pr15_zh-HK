<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 PolicyStat |Microsoft Azure" 
    description="了解如何使用 PolicyStat Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>教程︰ 与 PolicyStat 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 PolicyStat 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   PolicyStat 租户
  
后学完本教程，您已分配给 PolicyStat Azure AD 用户将能够到 PolicyStat 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 PolicyStat
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-policystat-tutorial/IC808662.png "方案")
##<a name="enabling-the-application-integration-for-policystat"></a>启用应用程序集成为 PolicyStat
  
本部分的目的是概述如何启用应用程序集成为 PolicyStat。

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>若要启用应用程序集成为 PolicyStat，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-policystat-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-policystat-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-policystat-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-policystat-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**PolicyStat**。

    ![应用程序库](./media/active-directory-saas-policystat-tutorial/IC808627.png "应用程序库")

7.  在结果窗格中，选择**PolicyStat**，，然后单击**完成**添加应用程序。

    ![PolicyStat](./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 PolicyStat。  
PolicyStat 应用程序需要以特定的格式，这就需要你对**saml 令牌属性**配置中添加自定义属性映射 SAML 断言。  
下面的屏幕快照显示此示例。

![属性](./media/active-directory-saas-policystat-tutorial/IC808628.png "属性")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**PolicyStat**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-policystat-tutorial/IC808629.png "配置单一登录")

2.  在**您想怎样来登录到 PolicyStat 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-policystat-tutorial/IC808630.png "配置单一登录")

3.  **配置应用程序设置**页上的**符号在 URL**文本框中，键入到登录 URL PolicyStat 应用程序到您的用户使用的 URL (例如: *"https://demo-azure.policystat.com"*)，然后单击**下一步**。

    ![配置应用程序设置](./media/active-directory-saas-policystat-tutorial/IC808631.png "配置应用程序设置")

4.  在**配置单一登录 PolicyStat 在**页上，单击**下载元数据**，然后将保存在您的计算机上的元数据文件。

    ![配置单一登录](./media/active-directory-saas-policystat-tutorial/IC808632.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 PolicyStat 公司网站。

6.  单击**管理**选项卡，然后单击左侧的导航窗格中的**单一登录配置**。

    ![管理员菜单](./media/active-directory-saas-policystat-tutorial/IC808633.png "管理员菜单")

7.  在**设置**部分中，选择**启用单一登录集成**。

    ![单一登录配置](./media/active-directory-saas-policystat-tutorial/IC808634.png "单一登录配置")

8.  **配置属性**，请单击，然后在**配置属性**部分中，执行以下步骤︰

    ![单一登录配置](./media/active-directory-saas-policystat-tutorial/IC808635.png "单一登录配置")

    1.  在**用户名属性**文本框中，键入**uid**。
    2.  在**第一个名称属性**文本框中，键入**名字**。
    3.  在**最后的名称属性**文本框中，键入**姓氏**。
    4.  在**电子邮件属性**文本框中，键入**电子邮件地址**。
    5.  单击**保存更改**。

9.  **您的 IDP 元数据**，请单击，然后在**您的 IDP 元数据**部分中，执行以下步骤︰

    ![单一登录配置](./media/active-directory-saas-policystat-tutorial/IC808635.png "单一登录配置")

    1.  打开下载元数据文件，复制的内容，然后将其粘贴到**您的标识提供程序元数据**的文本框
    2.  单击**保存更改**。

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-policystat-tutorial/IC771723.png "配置单一登录")

11. 12. 在顶部菜单中，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-policystat-tutorial/IC795920.png "属性")

13. 若要添加所需的属性映射，请执行以下步骤︰

    ![属性](./media/active-directory-saas-policystat-tutorial/IC804823.png "属性")

    1.  单击**添加用户属性**。
    2.  在**属性名称**文本框中，键入**uid**。
    3.  在**属性值**文本框中，选择**ExtractMailPrefix()**。
    4.  从**邮件**列表中，选择**User.mail**。
    5.  单击**完成**。
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 PolicyStat，他们必须到 PolicyStat 配置。  
PolicyStat 只是在时间用户提供支持。 这意味着，您不需要手动将用户添加到 PolicyStat。  
用户将获得自动添加在首次登录通过单一登录上。

>[AZURE.NOTE]您可以使用任何其他 PolicyStat 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 PolicyStat 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>要将用户分配到 PolicyStat 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**PolicyStat**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-policystat-tutorial/IC808636.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-policystat-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。