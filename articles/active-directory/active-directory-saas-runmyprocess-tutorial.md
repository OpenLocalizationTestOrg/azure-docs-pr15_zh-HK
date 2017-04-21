<properties
    pageTitle="教程︰ Azure Active Directory 集成与 RunMyProcess |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 RunMyProcess 之间。"
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>教程︰ 与 RunMyProcess 的 Azure Active Directory 集成

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 RunMyProcess。

与 Azure AD 集成 RunMyProcess 提供了以下好处︰

- 您可以控制有权访问 RunMyProcess Azure AD 中
- 您可以使用户可以自动获取签名的 RunMyProcess （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 RunMyProcess Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 RunMyProcess 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 RunMyProcess
2. 配置和测试 Azure AD 单一登录


## <a name="adding-runmyprocess-from-the-gallery"></a>从库中添加 RunMyProcess
若要配置 RunMyProcess 集成到 Azure 的广告，您需要将 RunMyProcess 从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 RunMyProcess，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**RunMyProcess**。

    ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. 在结果窗格中，选择**RunMyProcess**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和测试 Azure AD 单一登录 RunMyProcess 基于一个名为"Britta Simon"的测试用户。

单一登录工作，是 Azure 的广告需求要了解 RunMyProcess 中的对应的用户到用户在 Azure 的广告。 换句话说，Azure AD 用户和 RunMyProcess 中的相关的用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为**用户名**RunMyProcess 中的值指定的**用户名**的值。

要配置和测试 Azure AD 单一登录使用 RunMyProcess，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 RunMyProcess 测试用户](#creating-a-runmyprocess-test-user)**的链接到她的 Azure 广告表示的 RunMyProcess 中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您 Azure AD single sign-on 等传统门户中启用并配置单一登录，RunMyProcess 应用程序中。

**以 RunMyProcess Azure AD 单一登录配置，请执行以下步骤︰**

1. 在经典的门户，在**RunMyProcess**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. 在**您想怎样来登录到 RunMyProcess 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    一。 在**符号上 URL**文本框中，键入 URL 使用以下模式︰ `https://live.runmyprocess.com/live/<tenant id>`。
        
    b。 单击**下一步**

    > [AZURE.NOTE] 请注意，您需要更新的值与实际 URL 符号上。 若要获取此值，请联系 RunMyProcess 的支持团队，通过<mailto:support@runmyprocess.com>。
 
4. 在**配置单一登录 RunMyProcess 在**页上，单击**下载证书**，然后将保存在您的计算机上的文件︰

    ![配置单一登录](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. 在不同的 web 浏览器窗口中，登录到管理员为您 RunMyProcess 租户。

6. 在左侧的导览面板中，单击**帐户**，选择**配置**。

    ![在应用程序端配置单一登录](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. 转到**身份验证方法**部分中，执行以下步骤︰

    ![在应用程序端配置单一登录](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    一。 作为**方法**，选择**具有 Samlv2 SSO**。

    b。 在**SSO 重定向**文本框从 Azure AD 应用程序配置向导将**SAML SSO URL**的值。

    c。 在**注销重定向**文本框从 Azure AD 应用程序配置向导将**单个 Sign-Out 服务 URL**的值。

    d。 在**名称 Id 格式**文本框从 Azure AD 应用程序配置向导将**名称标识符的格式**的值。

    电子。 将复制的内容的下载的证书文件，然后将其粘贴到文本框中**的证书**。 

    f。 单击**保存**图标。
    
8. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

9. 在**单一登录确认**页上，单击**完成**。  
 
    ![Azure 的广告单登录][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon 的经典门户创建的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰  ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   

### <a name="creating-a-runmyprocess-test-user"></a>RunMyProcess 测试用户
  
为了使 Azure AD 用户能够登录到 RunMyProcess，他们必须到 RunMyProcess 配置。 对于 RunMyProcess，资源调配是手动任务。

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的 RunMyProcess 公司网站。

2.  单击**帐户**并在左侧的导览面板中，选择**用户**，然后单击**新用户**。

    ![新用户](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "新用户")

3.  在**用户设置**部分中，执行以下步骤︰

    ![配置文件](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "配置文件")

    一。 键入的**名称**和您要到相关文本框提供有效 AAD 帐户的**电子邮件**。

    b。 选择**IDE 语言**、 一种**语言**和**配置文件**。

    c。 选择**发送帐户创建电子邮件给我**。

    d。 单击**保存**。

    >[AZURE.NOTE] 您可以使用任何其他 RunMyProcess 用户帐户创建工具或 Api 提供的 RunMyProcess 设置 Azure 活动目录的用户帐户。
    

### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon RunMyProcess 授予她访问使用 Azure 单一登录。

![分配用户][200] 

**若要分配 Britta Simon RunMyProcess，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**RunMyProcess**。

    ![配置单一登录](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. 在菜单上，单击**用户**。

    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 条形图的底部，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。
 
单击访问权限面板中的 RunMyProcess 平铺时，您应该获取自动签名对 RunMyProcess 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
