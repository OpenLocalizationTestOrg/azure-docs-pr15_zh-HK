<properties
    pageTitle="教程︰ Azure Active Directory 集成与 LMS 的画布 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 画布 LMS 启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>教程︰ Azure Active Directory 集成与 LMS 的画布

本教程的目的是显示 Azure 和画布上的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   画布租户

后学完本教程，您已分配给画布 Azure AD 用户将能够到画布上公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的画布
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "方案")
##<a name="enabling-the-application-integration-for-canvas"></a>启用应用程序集成的画布

本部分的目的是概述如何启用应用程序集成的画布。

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>若要启用应用程序集成为画布，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**画布**。

    ![应用程序库](./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "应用程序库")

7.  在结果窗格中，选择**画布**，，然后单击**完成**添加应用程序。

    ![画布](./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "画布")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到画布。  
画布的单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**画布上**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "配置单一登录")

2.  在**您想怎样用户能够登录到画布**上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**画布上符号在 URL**文本框中，键入您的 URL 使用以下模式`https://<tenant-name>.instructure.com`，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "配置应用程序的 URL")

4.  在**配置单一登录在画布**上，以下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到画布上公司网站。

6.  转到**课程\>托管帐户\>Microsoft**。

    ![画布](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "画布")

7.  在左侧导航窗格中，选择**身份验证**，然后单击**添加新的 SAML 配置**。

    ![身份验证](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "身份验证")

8.  在当前的集成页面上，请执行以下步骤︰

    ![当前的集成](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "当前的集成")

    1.  Azure 传统门户网站，在**配置单一登录在画布**对话框页中的**实体 ID**值，复制，然后将其粘贴到**IdP 实体 ID**文本框。
    2.  在 Azure 经典门户中，在**配置单一登录在画布**对话框页复制的**远程登录 URL**值，，然后将其粘贴到该**日志上 URL**文本框。
    3.  在 Azure 经典门户中，在**配置单一登录在画布**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**日志出 URL**文本框。
    4.  在 Azure 经典门户中，在**配置单一登录在画布**对话框页复制**更改密码 URL**值，，然后将其粘贴到**更改密码链接**文本框。
    5.  从导出的证书，复制的**指纹**值，然后将其粘贴到**证书指纹**文本框。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    6.  从**登录属性**列表中，选择**NameID**。
    7.  从**标识符的格式**列表中，选择**电子邮件地址**。
    8.  单击**保存身份验证设置**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到画布上，他们必须到画布设置。  
在画布上，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  登录到**画布**租户。

2.  转到**课程\>托管帐户\>Microsoft**。

    ![画布](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "画布")

3.  单击**用户**。

    ![用户](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "用户")

4.  单击**添加新用户**。

    ![用户](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "用户")

5.  在添加新用户对话框页，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "添加用户")

    1.  在**全名**文本框中，键入用户的名称。
    2.  在**电子邮件**文本框中，键入该用户的电子邮件地址。
    3.  在**登录**文本框中，键入该用户的 Azure 的广告电子邮件地址。
    4.  选择**用户创建此帐户的电子邮件**。
    5.  单击**添加用户**。

>[AZURE.NOTE] 您可以使用任何其他画布用户帐户创建工具或 Api 提供的画布来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>若要将用户分配到画布上，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**画布上**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
