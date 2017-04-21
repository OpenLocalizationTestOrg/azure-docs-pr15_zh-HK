<properties 
    pageTitle="教程︰ Azure Active Directory 集成 ArcGIS 与 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory ArcGIS 启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>教程︰ 与 ArcGIS 的 Azure Active Directory 集成

本教程的目的是显示 Azure 和 ArcGIS 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   ArcGIS-启用单一登录的订阅

后学完本教程，您已分配给 ArcGIS Azure AD 用户将能够到 ArcGIS 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用对 ArcGIS 的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-arcgis-tutorial/IC784735.png "方案")
##<a name="enabling-the-application-integration-for-arcgis"></a>启用对 ArcGIS 的应用程序集成

本部分的目的是概述如何启用 ArcGIS 的应用程序集成。

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>若要启用应用程序集成为 ArcGIS，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-arcgis-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-arcgis-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-arcgis-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-arcgis-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**ArcGIS**。

    ![Applcation 库](./media/active-directory-saas-arcgis-tutorial/IC784736.png "Applcation 库")

7.  在结果窗格中，选择**ArcGIS**，，然后单击**完成**添加应用程序。

    ![ArcGIS](./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ArcGIS。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**ArcGIS**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-arcgis-tutorial/IC784738.png "配置单一登录")

2.  在**您想怎样来登录到 ArcGIS 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-arcgis-tutorial/IC784739.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**ArcGIS 号在 URL**文本框中，URL 类型由用户用于登录使用以下模式"*https://company.maps.arcgis.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-arcgis-tutorial/IC784740.png "配置应用程序的 URL")

4.  在**配置单一登录 ArcGIS 上**页上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-arcgis-tutorial/IC784741.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 ArcGIS 公司网站。

6.  单击**编辑设置**。

    ![编辑设置](./media/active-directory-saas-arcgis-tutorial/IC784742.png "编辑设置")

7.  单击**安全**。

    ![安全](./media/active-directory-saas-arcgis-tutorial/IC784743.png "安全")

8.  在**企业登录**，单击**提供程序集的标识**。

    ![企业登录](./media/active-directory-saas-arcgis-tutorial/IC784744.png "企业登录")

9.  在**提供程序集标识**配置页上，请执行以下步骤︰

    ![设置标识提供程序](./media/active-directory-saas-arcgis-tutorial/IC784745.png "设置标识提供程序")

    1.  在名称文本框中，键入您的组织的名称。
    2.  为**将使用提供的企业标识提供程序的元数据**，请选择**文件**。
    3.  若要上载下载元数据文件，请单击**选择文件**。
    4.  单击**设置标识提供程序**。

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-arcgis-tutorial/IC784746.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 ArcGIS，他们必须到 ArcGIS 调配。  
在 ArcGIS，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  登录到**ArcGIS**租户。

2.  单击**邀请成员**。

    ![邀请成员](./media/active-directory-saas-arcgis-tutorial/IC784747.png "邀请成员")

3.  选择**添加成员自动不发送一封电子邮件**，，然后单击**下一步**。

    ![自动添加成员](./media/active-directory-saas-arcgis-tutorial/IC784748.png "自动添加成员")

4.  在**成员**对话框页，请执行以下步骤︰

    ![添加并查看](./media/active-directory-saas-arcgis-tutorial/IC784749.png "添加并查看")

    1.  规定输入**名字**、**姓氏**和**电子邮件**所需的有效 AAD 帐户。
    2.  单击**添加和查看**。

5.  检查您所输入的数据，然后单击**添加成员**。

    ![添加成员](./media/active-directory-saas-arcgis-tutorial/IC784750.png "添加成员")

>[AZURE.NOTE] 您可以使用任何其他 ArcGIS 用户帐户创建工具或 Api 提供 ArcGIS 来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>若要将用户分配到 ArcGIS，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**ArcGIS**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-arcgis-tutorial/IC784751.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-arcgis-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
