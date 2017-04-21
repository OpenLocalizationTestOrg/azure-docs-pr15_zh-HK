<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Asana |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Asana 之间。"
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>教程︰ 与 Asana 的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 Asana。

与 Azure AD 集成 Asana 提供了以下好处︰

- 您可以控制有权访问 Asana Azure AD 中
- 您可以使用户可以自动获取签名上 Asana （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 Asana Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了**Asana**单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。 在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Asana
2. 配置和测试 Azure AD 单一登录


## <a name="adding-asana-from-the-gallery"></a>从库中添加 Asana
若要配置 Asana 集成到 Azure 的广告，您需要将 Asana 从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 Asana，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Asana**。

    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. 在结果窗格中，选择**Asana**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，配置和测试 Azure AD 单一登录 Asana 基于一个名为"Britta Simon"的测试用户。

单一登录工作，Azure 广告需要知道 Asana 对应的用户到用户在 Azure 的广告。 换句话说，Azure AD 用户和相关的 Asana 中用户之间的链接关系需要建立。
此链接关系的建立在 Azure AD Asana**的用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录 Asana 的测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 Asana 测试用户](#creating-an-Asana-test-user)**-中，链接到她的 Azure 广告表示 Asana 具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 配置单一登录

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录 Asana 应用程序中。

**要 Asana Azure AD 单一登录配置，请执行以下步骤︰**

1. 在顶部菜单中，单击**快速启动**。

    ![配置单一登录][6]
2. 在经典的门户，在**Asana**集成应用程序页，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][7] 

3. 在**您想怎样来登录到 Asana 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。
    
    ![配置单一登录](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰ 

    ![配置单一登录](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    一。 在符号上 URL 文本框中，键入 URL 使用以下模式︰`https://app.asana.com`

    c。 单击**下一步**。

5. 在**配置单一登录 Asana 在**页上，单击**下载证书**，然后将保存在您的计算机上的文件。 此外，SAML SSO url 复制的值。
    
    ![配置单一登录](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. 右键单击证书，然后打开证书文件使用记事本或您首选的文本编辑器。 将复制的开始和结束证书标题之间的内容。 这是您将使用中 Asana 配置 SSO 的 X.509 证书。

6. 在不同的浏览器窗口中，登录到您 Asana 的应用程序。 Asana 配置 SSO，请通过单击右上角的屏幕上的工作区名称访问工作区设置。 然后，单击的**\<您的工作区名称\>设置**。 

    ![配置单一登录](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. 在**组织设置**窗口中，单击**管理**。 然后，单击**成员必须通过 SAML 登录**启用 SSO 配置。 执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    一。 在**登录页的 URL** texbox 中，粘贴 SAML 号从 Azure 的广告的 URL。

    b。 在**X.509 证书**文本框中，粘贴已从 Azure AD 复制的 X.509 证书。

9. 单击**保存**。 如果需要进一步帮助，请转到[Asana 指南设置 SSO](https://asana.com/guide/help/premium/authentication#gl-saml) 。

7. 转到**配置单一登录 Asana 在**页上在 Azure 的广告中，选择单一登录配置进行确认，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

8. 在**单一登录确认**页上，单击**完成**。  
    
    ![Azure 的广告单登录][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰
 
    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-an-asana-test-user"></a>创建一个 Asana 的测试用户

在本节中，您创建称为 Britta Simon Asana 的用户。

1. 在**Asana**，转至明**团队**在左面板上。 单击加号按钮。 

    ![Azure 广告测试用户](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. 键入电子邮件britta.simon@contoso.com在文本框中，然后选择**邀请**。
3. 单击**发送邀请**。 新用户将收到一封电子邮件到她的电子邮件帐户。 她将需要创建和验证帐户。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon Asana 授予她访问使用 Azure 单一登录。

![分配用户][200] 

**要赋予 Asana Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**Asana**。

    ![配置单一登录](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在所有用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

本节的目的是测试您 Azure AD 单一登录。

请转到 Asana 登录页。 在电子邮件地址文本框中插入电子邮件地址britta.simon@contoso.com。 将密码文本框留空在，然后单击**登录**。 您将被重定向到 Azure 广告登录页。 完成您 Azure AD 的凭据。 现在，您已登录到 Asana。

## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
