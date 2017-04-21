<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 AirWatch |Microsoft Azure" 
    description="了解如何使用 AirWatch Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>教程︰ 与 AirWatch 的 Azure Active Directory 集成

本教程的目的是显示 Azure 和 AirWatch 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   AirWatch-启用单一登录的订阅

后学完本教程，您已分配给 AirWatch Azure AD 用户将能够到 AirWatch 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 AirWatch
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>启用应用程序集成为 AirWatch

本部分的目的是概述如何启用应用程序集成为 AirWatch。

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>若要启用应用程序集成为 AirWatch，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-airwatch-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-airwatch-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-airwatch-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-airwatch-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**AirWatch**。

    ![应用程序库](./media/active-directory-saas-airwatch-tutorial/IC791914.png "应用程序库")

7.  在结果窗格中，选择**AirWatch**，，然后单击**完成**添加应用程序。

    ![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 AirWatch。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**AirWatch**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-airwatch-tutorial/IC791916.png "配置单一登录")

2.  在**您想怎样来登录到 AirWatch 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-airwatch-tutorial/IC791917.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**AirWatch 号上的 URL**文本框中，键入用户用于登录到您的 AirWatch 应用程序 URL (例如:"*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-airwatch-tutorial/IC791918.png "配置应用程序的 URL")

4.  在**配置单一登录 AirWatch 在**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-airwatch-tutorial/IC791919.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 AirWatch 公司网站。

6.  在左侧的导航窗格中，单击**帐户**，然后单击**管理员**。

    ![管理员](./media/active-directory-saas-airwatch-tutorial/IC791920.png "管理员")

7.  展开**设置**菜单，然后单击**目录服务**。

    ![设置](./media/active-directory-saas-airwatch-tutorial/IC791921.png "设置")

8.  单击**用户**选项卡上，在**基本 DN**文本字段中的，键入域的名称，然后单击**保存**。

    ![用户](./media/active-directory-saas-airwatch-tutorial/IC791922.png "用户")

9.  单击**服务器**选项卡。

    ![服务器](./media/active-directory-saas-airwatch-tutorial/IC791923.png "服务器")

10. 执行以下步骤︰

    ![将上载](./media/active-directory-saas-airwatch-tutorial/IC791924.png "将上载")

    1.  作为**目录的类型**，选择**无**。
    2.  选择**使用 SAML 进行身份验证**。
    3.  若要上载下载的证书，请单击**上载**。

11. 在**请求**部分中，执行以下步骤︰

    ![请求](./media/active-directory-saas-airwatch-tutorial/IC791925.png "请求")

    1.  作为**申请绑定类型**，选择**公告**。
    2.  在 Azure 经典门户中，在**配置单一登录在 Airwatch**对话框页面上的**单一登录服务 URL**值，复制，然后将其粘贴到**标识提供程序单个符号在 URL**文本框。
    3.  作为**NameID 格式**，请选择**电子邮件地址**。
    4.  单击**保存**。

12. 再次单击**用户**选项卡。

    ![用户](./media/active-directory-saas-airwatch-tutorial/IC791926.png "用户")

13. 在**属性**部分中，执行以下步骤︰

    ![属性](./media/active-directory-saas-airwatch-tutorial/IC791927.png "属性")

    1.  在**对象标识符**文本框中，键入**http://schemas.microsoft.com/identity/claims/objectidentifier**。
    2.  在**用户名**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**。
    3.  在**显示名称**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**。
    4.  在**名字**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**。
    5.  在**姓氏**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**。
    6.  在**电子邮件**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**。
    7.  单击**保存**。

14. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-airwatch-tutorial/IC791928.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 AirWatch，他们必须到 AirWatch 配置。  
对于 AirWatch，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**AirWatch**公司网站。

2.  在左侧导航窗格中，单击**帐户**，然后单击**用户**。

    ![用户](./media/active-directory-saas-airwatch-tutorial/IC791929.png "用户")

3.  在**用户**菜单中，单击**列表视图**，然后单击**添加\>添加用户**。

    ![添加用户](./media/active-directory-saas-airwatch-tutorial/IC791930.png "添加用户")

4.  在**添加 / 编辑用户**对话框中，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-airwatch-tutorial/IC791931.png "添加用户")

    1.  键入**用户名**、**密码**、**确认密码**、**名字**、**姓氏**、**电子邮件地址**有效 Azure Active Directory 帐户要到相关文本框设置。
    2.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 AirWatch 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 AirWatch 提供。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>要将用户分配到 AirWatch 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**AirWatch**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-airwatch-tutorial/IC791932.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-airwatch-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
