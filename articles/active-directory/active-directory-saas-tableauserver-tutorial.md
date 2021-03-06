<properties
    pageTitle="教程︰ Azure Active Directory 集成使用 Tableau 服务器 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Tableau 服务器之间。"
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>教程︰ 使用 Tableau 服务器的 Azure Active Directory 集成

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 Tableau 服务器。

对于 Azure AD 集成 Tableau 服务器提供以下好处︰

- 您可以控制有权访问服务器 Tableau Azure AD 中
- 您可以启用用户自动获得签名的 Tableau （单一登录） 的服务器到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要使用 Tableau 服务器配置 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 Tableau 服务器单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。 

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Tableau 服务器
2. 配置和测试 Azure AD 单一登录


## <a name="adding-tableau-server-from-the-gallery"></a>从库中添加 Tableau 服务器
要配置 Tableau 服务器集成到 Azure 广告，您需要从库的 Tableau 服务器添加到托管的 SaaS 应用程序的列表。

**要从库中添加 Tableau 服务器，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 
 
    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Tableau 服务器**。

    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. 在结果窗格中，选择**Tableau 服务器**，然后单击**完成**添加应用程序。

    ![在库中选择应用程序](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和使用 Tableau 服务器基于一个名为"Britta Simon"的测试用户测试 Azure AD 单一登录。

对于单一登录工作，Azure 广告需要知道在 Azure AD 用户 Tableau 服务器中对应的用户。 换句话说，Azure AD 用户和相关的 Tableau 服务器中用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为**用户名**Tableau 服务器中的值指定的**用户名**的值。

要配置和测试 Azure AD 单一登录使用 Tableau 服务器，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 Tableau 服务器测试用户](#creating-a-tableauserver-test-user)**的链接到她的 Azure 广告表示的 Tableau 服务器中具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录 Tableau 服务器应用程序中。

Tableau 服务器应用程序需要以特定格式的 SAML 断言。 下面的屏幕快照显示此示例。 

![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**使用 Tableau 服务器配置 Azure AD 单一登录，请执行以下步骤︰**


1. 在 Azure 经典门户， **Tableau 服务器**应用程序集成在页上，在菜单上，单击**属性**。

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. 在**SAML 令牌的属性**对话框中，请执行以下步骤︰

    

    一。 单击**添加用户属性**以打开**添加用户属性**对话框。

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b。 在**Attrubute 名称**文本框中，键入**用户名**。

    c。 从**属性值**列表中，selsect **user.displayname**。

    d。 单击**完成**。  
    



1. 在顶部菜单中，单击**快速启动**。

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. **配置单一登录**，打开**配置单一登录**对话框中单击。

    ![配置单一登录][6] 



2. 在**您想怎样来登录到 Tableau 服务器的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. 在**配置应用程序设置**对话框页面上，请执行以下步骤，并单击**下一步**︰

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    一。 在**符号中 URL**文本框中，键入 Tableau 服务器的 URL。 

    b。 该标识符在框中复制 

    c。 单击**下一步**


4. 在**配置单一登录 Tableau 服务器**页上，执行以下步骤，然后单击**下一步**︰

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    一。 单击**下载元数据**，然后保存该文件在您的计算机上。

    b。 单击**下一步**。


6. 若要获得 SSO 配置您的应用程序，您需要登录到您作为管理员的 Tableau 服务器租户。

    一。 在 Tableau 服务器配置中，请单击**SAML**选项卡。

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b。 选择**使用 SAML 为单一登录**的复选框。

    c。 找到从 Azure 的传统门户网站，下载联合元数据文件，然后将其上载**SAML Idp 元数据文件**中。

    d。 Tableau 服务器返回的 URL — Tableau 服务器用户将访问，如 http://tableau_server 的 URL。 不建议使用 http://localhost。 不支持使用 URL 与尾部反斜杠 (例如，http://tableau_server/)。 复制**Tableau 服务器返回的 URL**并将其粘贴到 Azure 广告**符号在 URL**文本框步骤 3 中所示

    电子。 SAML 实体 ID — 实体 ID 唯一地标识到 IdP Tableau 服务器安装。 您可以输入您 Tableau 服务器 URL 再次在这里，如果您愿意，但它并没有为 Tableau 服务器的 URL。 复制**SAML 实体 ID**并将其粘贴到 Azure 广告**标识符**文本框步骤 3 中所示。

    f。 单击**导出的元数据文件**，在文本编辑器应用程序中打开它。 查找使用 Http Post 断言使用者服务 URL 并索引 0，复制的 URL。 现在将其粘贴到 Azure AD**回复 URL**文本框步骤 3 中所示。 

    g。 单击 Tableau 服务器 Configiuration 页中的**确定**按钮。

    > [AZURE.NOTE] 如果您需要帮助 Tableau 服务器上配置 SAML 然后，请参阅本文[配置 SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. 在 Azure 经典门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

7. 在**单一登录确认**页上，单击**完成**。 
 
    ![Azure 的广告单登录][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。

在用户列表中，选择**Britta Simon**。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
 
    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    一。 作为**用户的类型**，选择**您的组织中的新用户**。

    b。 在**用户名**文本框中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. 在**获得临时密码**对话框页面上，请执行以下步骤︰
 
    ![Azure 广告测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-tableau-server-test-user"></a>创建 Tableau 服务器测试用户

这一节的目标是创建一个名 Britta Simon Tableau 服务器中的用户。 您需要提供 Tableau 服务器中的所有用户。 此外请注意该用户名的用户应与已配置**的用户名**的 Azure 广告自定义属性中的值相匹配。 正确的映射集成应使用[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)。

> [AZURE.NOTE] 如果您需要手动创建用户，您需要与您组织中的 Tableau 服务器管理员联系。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon Azure 单一登录使用 Tableau 服务器授予其访问权限。

![分配用户][200] 

**若要分配 Britta Simon Tableau 服务器，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。
 
    ![分配用户][201] 

2. 在应用程序列表中，选择**Tableau 服务器**。

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. 在菜单上，单击**用户**。

    ![分配用户][203]

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。

当您单击访问权限面板中的 Tableau 服务器拼贴时，您应该获取自动签名对 Tableau 服务器应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
