<properties 
    pageTitle="教程︰ 使用 OfficeSpace 软件的 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory OfficeSpace 软件启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>教程︰ 使用 OfficeSpace 软件的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 OfficeSpace 软件的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   OfficeSpace 软件的启用单一登录的订阅
  
学完本教程后, 已分配给 OfficeSpace 软件的 Azure AD 用户将能够到在 OfficeSpace 软件公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用 OfficeSpace 软件的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-officespace-software-tutorial/IC777764.png "方案")
##<a name="enabling-the-application-integration-for-officespace-software"></a>启用 OfficeSpace 软件的应用程序集成
  
本部分的目的是概述如何启用 OfficeSpace 软件的应用程序集成。

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>若要启用应用程序集成的 OfficeSpace 软件，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-officespace-software-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-officespace-software-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-officespace-software-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-officespace-software-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**OfficeSpace 软件**。

    ![应用程序库](./media/active-directory-saas-officespace-software-tutorial/IC777765.png "应用程序库")

7.  在结果窗格中，选择**OfficeSpace 软件**，然后单击**完成**添加应用程序。

    ![OfficeSpace 软件](./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace 软件")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 OfficeSpace 软件。  
配置单一登录 OfficeSpace 软件需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**OfficeSpace 软件**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录上 =](./media/active-directory-saas-officespace-software-tutorial/IC777766.png "配置单一登录上 =")

2.  在**您希望如何登录到 OfficeSpace 软件的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-officespace-software-tutorial/IC777767.png "配置单一登录")

3.  在**配置应用程序 URL**页面上的**OfficeSpace 软件符号在 URL**文本框中，键入用户用于登录到您的 OfficeSpace 软件应用程序的 URL (例如:"*https://company.officespacesoftware.com*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-officespace-software-tutorial/IC775556.png "配置应用程序的 URL")

4.  在**配置单一登录 OfficeSpace 软件**页面上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-officespace-software-tutorial/IC793769.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 OfficeSpace 软件公司网站。

6.  转到**管理\>连接器**。

    ![管理](./media/active-directory-saas-officespace-software-tutorial/IC777769.png "管理")

7.  请单击**SAML 授权**。

    ![连接器](./media/active-directory-saas-officespace-software-tutorial/IC777770.png "连接器")

8.  在**SAML 授权**部分中，执行以下步骤︰

    ![SAML 配置](./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML 配置")

    1.  在 Azure 经典门户中，在**配置单一登录 OfficeSpace 软件**的对话页上的**远程登录 URL**值，复制，然后将其粘贴到**注销提供程序 url**文本框。
    2.  在 Azure 经典门户中，在**配置单一登录 OfficeSpace 软件**的对话页上，**远程注销 URL**值，复制，然后将其粘贴到该**客户端 idp 目标 url**文本框。
    3.  从导出的证书，复制的**指纹**值，然后将其粘贴到**idp 客户端证书指纹**文本框。  

        >[AZURE.TIP]
        有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    4.  单击**保存设置**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-officespace-software-tutorial/IC777772.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 OfficeSpace 软件，他们必须到 OfficeSpace 软件设置。 在 OfficeSpace 软件的情况下配置是自动执行的任务。  
没有为您的操作项。  
用户会自动创建在必要时在第一次登录尝试。

>[AZURE.NOTE]您可以使用任何其他 OfficeSpace 软件用户帐户创建工具或 Api 提供的 OfficeSpace 软件来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>若要将用户分配到 OfficeSpace 软件，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**OfficeSpace 软件**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-officespace-software-tutorial/IC777773.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-officespace-software-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。