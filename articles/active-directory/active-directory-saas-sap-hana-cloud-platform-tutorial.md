<properties 
    pageTitle="教程︰ 与 SAP HANA 云平台 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 SAP HANA 云平台 Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>与 SAP HANA 云平台的教程︰ Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 SAP HANA 云平台的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   SAP HANA 云平台帐户
  
后学完本教程，您已分配给 SAP HANA 云平台 Azure AD 用户将能够到单一登录到应用程序中使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。

>[AZURE.IMPORTANT]您需要部署自己的应用程序或订阅您 SAP HANA 云平台的帐户上的应用程序来测试上的单一登录。 在本教程中，应用程序部署中的帐户。
  
在本教程中介绍的方案由以下构造块组成︰

1.  使 SAP HANA 云平台的应用程序集成
2.  配置单一登录
3.  向用户分配角色
4.  分配用户

![方案](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "方案")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>使 SAP HANA 云平台的应用程序集成
  
本部分的目的是概述如何启用 SAP HANA 云平台的应用程序集成。

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>若要启用 SAP HANA 云平台的应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**SAP HANA 的云平台**。

    ![应用程序库](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "应用程序库")

7.  在结果窗格中，选择**SAP HANA 云平台**，，然后单击**完成**添加应用程序。

    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 SAP HANA 的云平台。  
作为此过程的一部分，您都需要将 base 64 编码的证书传到 SAP HANA 云平台租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**SAP HANA 云平台**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "配置单一登录")

2.  在**您想怎样来登录到 SAP HANA 云平台的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "配置单一登录")

3.  在不同的 web 浏览器窗口中，登录到 SAP HANA 云平台考核中心在 https://account。\<横向主机\>.ondemand.com/cockpit (例如︰ *https://account.hanatrial.ondemand.com/cockpit*)。

4.  单击**信任**选项卡。

    ![信任](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "信任")

5.  在信任管理部分中，执行以下步骤︰

    ![获取元数据](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "获取元数据")

    1.  单击**本地服务提供商**选项卡。
    2.  若要下载 SAP HANA 云平台的元数据文件，请单击**获取元数据**。

6.  在 Azure 活动传统门户网站，在**配置应用程序 URL**页上，执行以下步骤，，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "配置应用程序的 URL")

    1.  在**符号上 URL**文本框中，键入用户用于登录到**SAP HANA 云平台**应用程序的 URL。 这是在 SAP HANA 云平台应用程序的受保护资源的特定帐户的 URL。 URL 基于下面的模式︰ *https://\<应用程序名称\>\<帐户名\>。\<横向主机\>.ondemand.com/\<路径\_到\_保护\_资源\>*(例如︰ *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]这是在 SAP HANA 云平台的应用程序要求用户进行身份验证的 URL。

    2.  打开下载的 SAP HANA 云平台的元数据文件，然后找到**ns3:AssertionConsumerService**标记。
    3.  在**位置**属性的值复制，然后将其粘贴到**SAP HANA 云平台回复 URL**文本框。

7.  在**配置单一登录 SAP HANA 云平台在**页面上，要下载元数据，请单击**下载元数据**，然后保存您的计算机上的文件。

    ![配置单一登录](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "配置单一登录")

8.  在 SAP HANA 云平台考核中心中的**本地服务提供程序**部分中，执行以下步骤︰

    ![信任管理](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "信任管理")

    1.  单击**编辑**。
    2.  作为**配置类型**，选择**自定义**。
    3.  作为**本地提供程序的名称**，保留默认值。
    4.  要生成的**签名密钥**和**签名证书**的密钥对，请单击**生成密钥对**。
    5.  作为**主要的传播**中，选择**已禁用**。
    6.  与**强制身份验证**中，选择**已禁用**。
    7.  单击**保存**。

9.  单击**受信任的身份提供程序**选项卡，然后单击**添加受信任的身份提供程序**。

    ![信任管理](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "信任管理")

    >[AZURE.NOTE]管理受信任的身份提供程序的列表，您需要在本地服务提供程序部分中选择自定义配置类型。 对于默认的配置类型，您必须对 SAP ID 服务的非可编辑和隐式信任。 不，您没有任何信任设置。

10. 单击**常规**选项卡，然后单击**浏览**上载下载元数据文件。

    ![信任管理](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "信任管理")

    >[AZURE.NOTE] 上载的元数据文件之后, 会自动填充**单个登录 URL**、**一个注销 URL**和**签名证书**的值。

11. 单击**属性**选项卡。

12. 在**属性**选项卡，请执行以下步骤︰

    ![属性](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "属性")

    1.  通过单击**Add Assertion-Based 属性**，添加基于断言的下列属性︰

        |声明属性| 主体属性|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname|   名字字段|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname|        姓氏|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress|电子邮件|

    >[AZURE.NOTE]HCP 上的应用程序开发方式，即他们期望 SAML 响应中和他们的名下 （主体属性） 访问代码中的该属性的属性取决于配置的属性。
    >  
    >一。  屏幕快照中的**默认属性**是只用于说明目的。 不要求为使工作方案。  
    >
    >b。  名称和屏幕快照中所示的**主体属性**的值取决于如何开发应用程序。 很可能您的应用程序，需要不同的映射。

13. 在 Azure 经典门户中，在**配置单一登录在 SAP HANA 云平台**的对话页上，选择单一登录配置确认，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "配置单一登录")
  
作为可选步骤中，您可以配置基于断言的组，Azure 活动目录标识提供程序

>[AZURE.NOTE]在 SAP HANA 云平台上使用组允许您动态地将一个或多个用户分配到 SAP HANA 云平台应用程序，由 SAML 2.0 断言中的属性值中的一个或多个角色。 例如，如果断言包含该属性"*合同临时 =*"，您可能希望所有受影响的用户添加到的组"*临时*"。 "*临时*"组可能包含部署 SAP HANA 云平台帐户中的一个或多个应用程序中的一个或多个角色。
>  
>如果您想要大容量分配到一个或多个角色的应用程序在您的 SAP HANA 云平台帐户中的多个用户，请使用基于断言的组。 如果您只想为 (a) 建议直接在 SAP HANA 云平台考核中心的"**授权**"选项卡中指定的特定角色指定单个或小的用户数。

##<a name="assigning-a-role-to-a-user"></a>向用户分配角色
  
为了使 Azure AD 用户能够登录到 SAP HANA 云平台，必须给他们分配在 SAP HANA 云平台的角色。

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>若要向用户指定角色，请执行以下步骤︰

1.  登录到您的**SAP HANA 云平台**考核中心。

2.  执行以下步骤︰

    ![授权](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "授权")

    1.  单击**授权**。
    2.  单击**用户**选项卡。
    3.  在**用户**文本框中，键入该用户的电子邮件地址。
    4.  单击**分配**将用户分配给角色。
    5.  单击**保存**。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>要将用户分配到 SAP HANA 云平台，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**SAP HANA 云平台**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。