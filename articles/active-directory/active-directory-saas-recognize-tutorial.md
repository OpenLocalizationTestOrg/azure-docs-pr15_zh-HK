<properties
    pageTitle="教程︰ Azure Active Directory 集成识别与 |Microsoft Azure"
    description="了解如何配置单一登录之间 Azure Active Directory 和识别。"
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>教程︰ Azure Active Directory 集成与识别

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成识别。

与 Azure AD 集成识别提供了以下好处︰

- 您可以控制有权访问识别 Azure AD 中
- 您可以使用户可以自动获取签名的识别 （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置识别 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了识别单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库添加识别
2. 配置和测试 Azure AD 单一登录


## <a name="adding-recognize-from-the-gallery"></a>从库添加识别
若要配置识别集成到 Azure 广告，需要将识别从库添加到托管的 SaaS 应用程序的列表。

**要从库添加识别，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。
    
    ![应用程序][2]

4. 单击页面底部的**添加**。
    
    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**识别**。

    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. 在结果面板中，选择**识别**，，然后单击**完成**添加应用程序。

    ![在库中选择应用程序](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和 Azure AD 单一登录测试与识别基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 的广告需要知道在 Azure AD 用户识别中对应的用户。 换句话说，Azure AD 用户和相关的识别中用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为**用户名**识别中的值指定的**用户名**的值。

要配置和测试 Azure AD 单一登录与识别，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建识别测试用户](#creating-a-recognize-test-user)**的链接到她的 Azure 广告表示的识别中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您启用 Azure AD 单一登录的传统门户并识别应用程序中配置单一登录。

**要识别 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在经典的门户，在**识别**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. 在**您想怎样来登录到识别的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。
    
    ![配置单一登录](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤，并单击**下一步**︰

    ![配置单一登录](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    一。 在**符号上 URL**文本框中，键入 URL 使用以下模式︰ `https://recognizeapp.com/<your-domain>/saml/sso`。

    b。 在**标识符**文本框中，键入一个 URL 使用以下模式︰ `https://recognizeapp.com/<your-domain>/saml/metadata`。

    c。 单击**下一步**

    > [AZURE.NOTE] 如果您不了解这些 Url，键入示例模式的示例 Url。 若要获取这些值，可以更多详细信息，请第 9 步或与通过识别支持团队联系<mailto:support@recognizeapp.com>。

4. 在**配置单一登录在识别**页面上，单击**下载证书**，然后保存您的计算机上的文件︰

    ![配置单一登录](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. 在不同的 web 浏览器窗口中，登录到管理员为您识别租户。

6. 在右上角，单击**菜单**。 转到**公司管理**。

    ![配置单一登录在应用程序端](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. 在左侧的导航窗格中，单击**设置**。

    ![配置单一登录在应用程序端](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. 在**SSO 设置**部分中执行以下步骤。

    ![配置单一登录在应用程序端](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    一。 作为**启用 SSO**，选择**打开**。

    b。 在**IDP 实体 ID**文本框从 Azure AD 应用程序配置向导将**颁发者 URL**的值。

    c。 在**Sso 目标 url**文本框从 Azure AD 应用程序配置向导将**单一登录服务 URL**的值。

    d。 在**Slo 目标 url**文本框从 Azure AD 应用程序配置向导将**单个 Sign-Out 服务 URL**的值。

    电子。 在记事本中打开您已下载的证书的文件，将其内容复制到剪贴板，然后将其粘贴到**证书**文本框。 

    f。 单击**保存设置**按钮。 

9. 旁边**SSO 设置**部分中，将复制**服务提供程序元数据 url**下的 URL。
    
    ![配置单一登录在应用程序端](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. 打开**元数据 URL 链接**下空白浏览器下载元数据文档。 然后使用 EntityDescriptor 提供的值识别您的**标识符**在**配置应用程序设置**对话框。

    ![配置单一登录在应用程序端](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

12. 在**单一登录确认**页上，单击**完成**。  
    
    ![Azure 的广告单登录][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon 的经典门户创建的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框中，键入**Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰
    
    ![Azure 广告测试用户](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-recognize-test-user"></a>创建识别测试用户

为了使 Azure AD 用户能够登录到识别，必须到识别配置它们。 识别，在资源调配是手动任务。

此应用程序不支持 SCIM 资源调配，但有规定用户可选的用户同步。 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您识别公司的网站。

2.  在右上角，单击**菜单**。 转到**公司管理**。

3.  在左侧的导航窗格中，单击**设置**。

4.  **用户同步**部分执行以下步骤。
    
    ![新用户](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "新用户")

    一。 为**启用同步**，选择**ON**。

    b。 作为**选择同步提供程序**，选择**Microsoft / Office 365**。

    c。 单击**运行用户同步**

### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 她访问权限授予识别使用 Azure 单一登录。
    
![分配用户][200]

**要 Britta Simon 为识别，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。
    
    ![分配用户][201]

2. 在应用程序列表中，选择**识别**。
    
    ![配置单一登录](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. 在菜单上，单击**用户**。
    
    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。
    
    ![分配用户][205]

### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。
 
当单击接入面板中的识别平铺时，您应该获取自动签名对识别应用程序。

## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
