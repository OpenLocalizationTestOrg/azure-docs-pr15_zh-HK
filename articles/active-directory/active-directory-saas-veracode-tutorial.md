<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Veracode |Microsoft Azure" 
    description="了解如何使用 Veracode Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>教程︰ 与 Veracode 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Veracode 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Veracode-启用单一登录的订阅
  
后学完本教程，您已分配给 Veracode Azure AD 用户将能够到单一登录到应用程序中使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Veracode
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-veracode-tutorial/IC802903.png "方案")

##<a name="enabling-the-application-integration-for-veracode"></a>启用应用程序集成为 Veracode
  
本部分的目的是概述如何启用应用程序集成为 Veracode。

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>若要启用应用程序集成为 Veracode，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-veracode-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-veracode-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-veracode-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-veracode-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Veracode**。

    ![应用程序库](./media/active-directory-saas-veracode-tutorial/IC802904.png "应用程序库")

7.  在结果窗格中，选择**Veracode**，，然后单击**完成**添加应用程序。

    ![Veracode](./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Veracode。  
Veracode 应用程序需要以特定的格式，这就需要你对**saml 令牌属性**配置中添加自定义属性映射 SAML 断言。  
下面的屏幕快照显示此示例。

![属性](./media/active-directory-saas-veracode-tutorial/IC802906.png "属性")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Veracode**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-veracode-tutorial/IC802907.png "配置单一登录")

2.  在**您想怎样来登录到 Veracode 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-veracode-tutorial/IC802908.png "配置单一登录")

3.  在**配置应用程序设置**页上单击**下一步**。

    ![配置应用程序设置](./media/active-directory-saas-veracode-tutorial/IC802909.png "配置应用程序设置")

4.  在**配置单一登录 Veracode 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-veracode-tutorial/IC802910.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Veracode 公司网站。

6.  在菜单上，单击**设置**，然后单击**管理**。

    ![管理](./media/active-directory-saas-veracode-tutorial/IC802911.png "管理")

7.  单击**SAML**选项卡。

8.  在**组织 SAML 设置**部分中，执行以下步骤︰

    ![管理](./media/active-directory-saas-veracode-tutorial/IC802912.png "管理")

    1.  在 Azure 经典门户中，在**配置单一登录在 Veracode**对话框页面上，**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框
    2.  若要上载您已下载的证书，请单击**选择文件**。
    3.  选择**启用自助注册**。

9.  在**自动注册设置**部分中，执行下面的步骤，然后单击**保存**︰

    ![管理](./media/active-directory-saas-veracode-tutorial/IC802913.png "管理")

    1.  为**新用户激活**，选择**不激活要求**。
    2.  作为**用户数据更新**，选择**首选项 Veracode 用户数据**。
    3.  **SAML 属性的详细信息**，请选择以下︰
        -   **用户角色**
        -   **策略管理器**
        -   **审阅者**
        -   **安全主管**
        -   **总经理**
        -   **提交者**
        -   **创建者**
        -   **所有的扫描类型**
        -   **团队成员**
        -   **默认团队**

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-veracode-tutorial/IC802914.png "配置单一登录")

11. 在顶部菜单中，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-veracode-tutorial/IC795920.png "属性")

12. 若要添加所需的属性映射，请执行以下步骤︰

    ![属性](./media/active-directory-saas-veracode-tutorial/IC802906.png "属性")

  	| 属性名称 | 属性值 |
  	|:---------------|:----------------|
  	| 名字字段      | User.givenname  |
  	| 姓氏       | User.surname    |
  	| 电子邮件          | User.mail       |

    1.  上表中每个数据行，请单击**添加用户属性**。
    
    2.  在**属性名称**文本框中，键入该行显示的属性名称。

    3.  在**属性值**文本框中，选择的行中显示的属性值。

    4.  单击**完成**。

13. 单击**应用更改**。

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Veracode，他们必须到 Veracode 配置。  
对于 Veracode，资源调配是自动执行的任务。  
没有为您的操作项。.
  
用户会自动创建在必要时在第一次登录尝试。

>[AZURE.NOTE] 您可以使用任何其他 Veracode 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Veracode 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>要将用户分配到 Veracode 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Veracode**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-veracode-tutorial/IC802915.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-veracode-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。