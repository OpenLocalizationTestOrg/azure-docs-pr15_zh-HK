<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Aha ！ |Microsoft Azure" 
    description="了解如何使用 Aha ！ 使用 Azure 活动目录启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>教程︰ Azure Active Directory 集成与 Aha ！

本教程的目的是要显示 Azure 和 Aha 的集成 ！  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Aha ！ 单一登录启用了订阅

在完成本教程之后, Azure 的 AD 用户已分配给 Aha ！ 将能够到您 Aha 在应用程序的单一登录 ！ 公司网站 （服务提供程序初始化登录），或者使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的 Aha ！
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-aha-tutorial/IC798944.png "方案")
##<a name="enabling-the-application-integration-for-aha"></a>启用应用程序集成的 Aha ！

本部分的目的是概述如何启用应用程序集成的 Aha ！。

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>若要启用应用程序集成的 Aha ！，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-aha-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-aha-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-aha-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-aha-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Aha ！**。

    ![应用程序库](./media/active-directory-saas-aha-tutorial/IC798945.png "应用程序库")

7.  在结果窗格中，选择**Aha ！**，然后单击**完成**添加该应用程序。

    ![Aha ！](./media/active-directory-saas-aha-tutorial/IC802746.png "Aha ！")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够通过身份验证的 Aha ！ 在 Azure 广告使用基于 SAML 协议联合其帐户。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 的传统门户网站，在**Aha ！** 应用程序集成页面上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-aha-tutorial/IC798946.png "配置单一登录")

2.  在**如何您希望用户能够登录到 Aha ！** 页、 **Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-aha-tutorial/IC798947.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**Aha ！符号在 URL**文本框，键入 URL 由用户用于登录到您的 Aha ！ 应用程序 (例如:"*https://company.aha.io/session/new*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-aha-tutorial/IC798948.png "配置应用程序的 URL")

4.  在**在 Aha 配置单一登录 ！** 页面，以下载元数据文件，单击**下载元数据**，然后保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-aha-tutorial/IC798949.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，登录到您的 Aha ！ 以管理员身份的公司网站。

6.  在菜单上，单击**设置**。

    ![设置](./media/active-directory-saas-aha-tutorial/IC798950.png "设置")

7.  单击**帐户**。

    ![配置文件](./media/active-directory-saas-aha-tutorial/IC798951.png "配置文件")

8.  单击**安全和单一登录**。

    ![安全性和单一登录](./media/active-directory-saas-aha-tutorial/IC798952.png "安全性和单一登录")

9.  在**单一登录**部分，作为**身份标识提供程序**，选择**SAML2.0**。

    ![安全性和单一登录](./media/active-directory-saas-aha-tutorial/IC798953.png "安全性和单一登录")

10. 在**单一登录**配置页上，请执行以下步骤︰

    ![单一登录](./media/active-directory-saas-aha-tutorial/IC798954.png "单一登录")

    1.  在**名称**文本框中，键入您的配置的名称。
    2.  **配置使用**，选择**元数据文件**。
    3.  若要上载下载元数据文件，请单击**浏览**。
    4.  单击**更新**。

11. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-aha-tutorial/IC798955.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 Aha ！，它们必须配置成 Aha ！。  
在 Aha ！，资源调配是自动执行的任务。  
没有为您的操作项。
  
用户会自动创建在必要时在第一次登录尝试。

>[AZURE.NOTE] 您可以使用任何其他 Aha ！ 用户帐户创建工具或 Api 提供的 Aha ！ 若要设置 AAD 的用户帐户。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>若要将用户分配到 Aha ！，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Aha ！**应用程序集成页面上，单击**指派的用户**。

    ![分配用户](./media/active-directory-saas-aha-tutorial/IC798956.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-aha-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
