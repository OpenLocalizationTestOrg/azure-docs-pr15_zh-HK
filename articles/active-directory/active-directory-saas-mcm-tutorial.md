<properties 
    pageTitle="教程︰ 使用 MCM 的 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory MCM 启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>教程︰ 使用 MCM 的 Azure Active Directory 集成
  
本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 MCM。

与 Azure AD 集成 MCM 提供了以下好处︰

- 您可以控制有权访问 MCM Azure AD 中
- 您可以使用户可以自动获取签名的 MCM （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 MCM Azure 广告集成，您需要以下各项︰

- 有效的 Azure 订购
- 在已启用的订阅了 MCM 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 MCM
2. 配置和测试 Azure AD 单一登录

## <a name="adding-mcm-from-the-gallery"></a>从库中添加 MCM
若要配置 MCM 集成到 Azure 的广告，您需要将 MCM 从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 MCM，请执行以下步骤︰**

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**MCM**。

    ![应用程序库](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "应用程序库")

7.  在结果窗格中，选择**MCM**，，然后单击**完成**添加应用程序。

    ![MCM](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和基于一个名为"Britta Simon"的测试用户的 MCM Azure AD 单一登录测试。

对于单一登录工作，Azure 广告需要知道在 Azure AD 用户 MCM 中对应的用户。 换句话说，Azure AD 用户和 MCM 中相关的用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为 MCM**的用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录使用 MCM 测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 MCM 测试用户](#creating-a-mcm-test-user)**的链接到她的 Azure 广告表示的 MCM 中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置
  
在本节中，您启用 Azure AD 单一登录的传统门户和 MCM 应用程序中配置单一登录。

**要 MCM Azure AD 单一登录配置，请执行以下步骤︰**

1.  在 Azure 传统门户网站，在**MCM**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "配置单一登录")

2.  在**您想怎样来登录到 MCM 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![Microsoft Azure 广告单登录](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure 广告单登录")

3.  在配置应用程序设置对话框页面上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "配置应用程序的 URL")

    一。 在**符号上 URL**文本框中，键入︰ `https://myaba.co.uk/client-access/<company name>/saml.php`。
    
    b。 单击**下一步**

4.  在**配置单一登录在 MCM**页上，单击**下载元数据**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "配置单一登录")

5. 若要获得 SSO 配置您的应用程序，请联系 MCM 的支持团队。 附加的下载元数据文件并共享与 MCM 小组在其一侧上设置 SSO。

6.  在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "配置单一登录")

7. 在**单一登录确认**页上，单击**完成**。

    ![配置单一登录](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "配置单一登录")


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户

本节的目的是在调用 Britta Simon 的经典门户创建的测试用户。

![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    一。 记下**新密码**的值。

    b。 单击**完成**。   

###<a name="creating-a-mcm-test-user"></a>MCM 测试用户
  
在本节中，您创建在 MCM 中称为 Britta Simon 的用户。 请使用 MCM 的支持小组来 MCM 平台中添加用户。

>[AZURE.NOTE]您可以使用任何其他 MCM 用户帐户创建工具或 Api 提供的 MCM 预置 AAD 用户帐户。


###<a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配
  
本节的目的是使 Britta Simon MCM 中授予其访问权限使用 Azure 单一登录。
    
![分配用户](./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "分配用户")

**要为 MCM Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。
    
    ![分配用户](./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "分配用户")

2. 在应用程序列表中，选择**MCM**。
    
    ![配置单一登录](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. 在菜单上，单击**用户**。
    
    ![分配用户](./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "分配用户")

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。
    
    ![分配用户](./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "分配用户")


### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。
 
当您单击访问权限面板中的 MCM 拼贴时，您应该获取自动签名对 MCM 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)