<properties 
    pageTitle="教程︰ 使用 Jitbit 帮助台的 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Jitbit 帮助台以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>教程︰ 使用 Jitbit 帮助台的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Jitbit 帮助台的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   帮助台 Jitbit 租户
  
在完成本教程之后, 已分配给 Jitbit 台 Azure AD 用户将能够到 Jitbit 台公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用 Jitbit 帮助台为应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "方案")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>启用 Jitbit 帮助台为应用程序集成
  
本部分的目的是概述如何启用 Jitbit 帮助台为应用程序集成。

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>若要启用 Jitbit 帮助台为应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Jitbit 帮助台**。

    ![应用程序库](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "应用程序库")

7.  在结果窗格中，选择**Jitbit 帮助台**，，然后单击**完成**添加应用程序。

    ![JitBit](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Jitbit 帮助台。 .  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

>[AZURE.IMPORTANT] 为了能够在 Jitbit 台租户上配置单一登录，您需要首先联系 Jitbit 帮助台技术支持以获取启用此功能。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 经典门户， **Jitbit 服务台**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "配置单一登录")

2.  在**您想怎样用户能够登录到 Jitbit 技术支持**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**Jitbit 台号在 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。Jitbit.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "配置应用程序的 URL")

4.  在**配置单一登录 Jitbit 台在**页上，下载您的证书，单击**证书下载**，然后保存为本地证书文件**c:\\Jitbit Helpdesk.cer**。

    ![配置单一登录](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Jitbit 台公司网站。

6.  在顶部工具栏上，单击**管理**。

    ![管理](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "管理")

7.  单击**常规设置**。

    ![用户、 公司和权限](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "用户、 公司和权限")

8.  在**身份验证设置**配置部分中，执行以下步骤︰

    ![身份验证设置](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "身份验证设置")

    1.  选择**单个的启用 SAML 2.0 登录** **OneLogin**中使用单一登录 (SSO) 登录。
    2.  在 Azure 经典门户中，在**配置单一登录在 Jitbit 帮助台**的对话页上，复制的**服务提供商 (SP) 启动终结点**值，，然后将其粘贴到该**终结点的 URL**文本框。
    3.  创建您已下载的证书的**base 64 编码**的文件。
        
        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**X.509 证书**文本框
    5.  单击**保存更改**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Jitbit 帮助台，他们必须到 Jitbit 台调配。  
在 Jitbit 帮助台的情况下调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  登录到您**帮助台 Jitbit**租户。

2.  在菜单上，单击**管理**。

    ![管理](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "管理")

3.  单击**用户、 公司和权限**。

    ![用户、 公司和权限](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "用户、 公司和权限")

4.  单击**添加用户**。

    ![添加用户](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "添加用户")

5.  在创建部分中，键入要在下面的文本框设置 Azure AD 帐户的数据︰**用户名**、**电子邮件**、**名字**、**姓氏**

    ![创建](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "创建")

6.  单击**创建**。

>[AZURE.NOTE] 您可以使用任何其他 Jitbit 台用户帐户创建工具或 Api 提供 Jitbit 帮助台来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>若要将用户分配到 Jitbit 技术支持，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Jitbit 服务台**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。