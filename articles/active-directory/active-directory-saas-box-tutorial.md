<properties 
    pageTitle="教程︰ Azure Active Directory 集成框 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 中启用单一登录、 自动化资源调配，以及更多 ！" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>教程︰ Azure Active Directory 集成框


  
本教程的目的是显示集成的 Azure 和框。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在框中测试租户
  
在完成本教程之后, 已分配给框 Azure AD 用户将能够到框公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成框
2.  配置单一登录
3.  配置用户和组设置
4.  分配用户

![方案](./media/active-directory-saas-box-tutorial/IC769537.png "方案")



##<a name="enabling-the-application-integration-for-box"></a>启用应用程序集成框
  
本部分的目的是概述如何启用应用程序集成的框。

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>若要启用应用程序集成的框中，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-box-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-box-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-box-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-box-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**框**。

    ![应用程序库](./media/active-directory-saas-box-tutorial/IC701023.png "应用程序库")

7.  在结果窗格中，选中**框**，，然后单击**完成**添加应用程序。

    ![框中](./media/active-directory-saas-box-tutorial/IC701024.png "框中")



##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够通过身份验证框中使用他们的帐户在 Azure AD 使用基于 SAML 协议的联合。 作为此过程的一部分，您需要将元数据上传到 Box.com。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**框中**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC769538.png "配置单一登录")

2.  在**您想怎样来登录到框中的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC769539.png "配置单一登录")

3.  在**配置应用程序 URL**页面上的**框租户 URL**文本框中，键入框租户 URL (例如︰ https://<mydomainname>。 box.com)，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-box-tutorial/IC669826.png "配置应用程序的 URL")

4.  在**配置单一登录框在**页面上，可以下载元数据，请单击**下载元数据**，然后在您的计算机上的本地数据文件。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC669824.png "配置单一登录")

5.  转发到框中该元数据文件支持团队。 支持团队需要为您的单一登录配置。

6.  选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC769540.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
本部分的目的是概述如何启用 Active Directory 中的用户帐户的资源调配。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1. 在 Azure 传统门户网站，在**框中**应用程序集成页上，单击**配置用户设置**以打开**配置用户设置**对话框。 

    ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769541.png "启用用户自动资源调配")

2. 在**启用用户供应到框**对话框页面上，单击**启用用户供应**。 

    ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769544.png "启用用户自动资源调配")

3. 在**登录框为授予访问权限**页上提供所需的凭据，然后单击**授权**。 

    ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769546.png "启用用户自动资源调配")


4. 单击**授予访问权限框中的**授权此操作并返回到 Azure 的传统门户网站。 

    ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769549.png "启用用户自动资源调配")


5. 在**提供选项**页面上**设置的对象类型**对应的复选框允许您选择组对象用户对象除了调配到框中。  有关详细信息，参阅下面"指派用户和组部分"。


6. 若要完成配置，请单击完成按钮。 

    ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769551.png "启用用户自动资源调配")



##<a name="assigning-a-test-user"></a>分配测试用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>若要将用户分配到框中，请执行以下步骤︰

1. 在 Azure 的传统门户网站，将创建一个测试帐户。

2. 在**框中**应用程序集成页上，单击**将用户分配**。 

    ![分配用户](./media/active-directory-saas-box-tutorial/IC769552.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。 

    ![是](./media/active-directory-saas-box-tutorial/IC767830.png "是")
  
您现在应该等待 10 分钟并验证帐户的已同步到框中。

作为第一个验证步骤，您可以检查供应状态，通过单击本页框应用程序集成 Azure 的传统门户网站上 D 中的仪表板。

![仪表板](./media/active-directory-saas-box-tutorial/IC769553.png "仪表板")

由相关的状态指示已成功完成资源调配周期用户︰

![集成状态](./media/active-directory-saas-box-tutorial/IC769555.png "集成状态")


在框中您租户中，同步的用户列在**管理控制台**中的**管理用户**。

![集成状态](./media/active-directory-saas-box-tutorial/IC769556.png "集成状态")


##<a name="assigning-users-and-groups"></a>指派用户和组

**框 > 用户和组**Azure 的经典门户中的选项卡允许您指定哪些用户和组应被授予访问权限框中。 分配的用户或组将导致发生的下列事项︰

* Azure 广告允许分配的用户 （或者通过直接赋值或组成员资格） 对框进行身份验证。 如果不指定用户，Azure 的广告将不允许其登录框，并在 Azure 广告登录页将返回错误。

* 框中的应用程序图块添加到用户的[应用程序启动器](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)。

* 如果启用了自动设置，然后指派的用户和/或组添加到自动配置设置队列。

    * 如果只被配置为用户对象来进行配置，然后直接指派的所有用户都放置在资源调配的队列中，和任何已分配的组的成员的所有用户将被都都放置在资源调配的队列。 
    
    * 如果组对象已配置为可调配，然后所有分配的组对象调配给框，以及将这些组的成员的所有用户。 时被写到框中的组和用户的成员资格会被保留。
    
您可以使用**属性 > 单一登录**选项卡来配置哪些用户属性 （或声明） 面前框基于 SAML 的身份验证过程和**属性 > 资源调配**选项卡以配置方式用户和组的属性从流向 Azure 广告框自动配置操作过程。 请参阅以下资源以获取详细信息。


## <a name="additional-resources"></a>其他资源

* [自定义发出 SAML 令牌中的声明](active-directory-saml-claims-customization.md)
* [资源调配︰ 自定义属性映射](active-directory-saas-customizing-attribute-mappings.md)
* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)
