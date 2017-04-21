<properties
    pageTitle="教程︰ 与 Azure Active Directory 集成@Task|Microsoft Azure"
    description="了解如何配置单一登录之间 Azure Active Directory 和@Task。"
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-task"></a>教程︰ 与 Azure Active Directory 集成@Task

本教程的目的是向您显示如何集成@Task使用 Azure 活动目录 (AD Azure)。  
将集成@Task与 Azure 的广告为您提供了以下好处︰ 

- 您可以控制有权 Azure AD 中@Task
- 您可以使用户可以自动获得签名对@Task（单一登录） 其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件 

若要配置与 Azure AD 集成@Task,您需要以下各项︰

- Azure 广告订阅
- @Task单点登录启用订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。 

 
## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。  
在本教程中介绍的方案由三个主要的构造块组成︰

1. 添加@Task库中 
2. 配置和测试 Azure AD 单一登录


## <a name="adding-task-from-the-gallery"></a>添加@Task库中
若要配置的集成@Task到 Azure 广告，您需要添加@Task从库添加到托管的 SaaS 应用程序的列表。

**若要添加@Task从库，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1] 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2] 

4. 单击页面底部的**添加**。

    ![应用程序][3] 

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4] 

6. 在搜索框中，键入**@Task**。

    ![应用程序][5] 

7. 在结果窗格中，选择**@Task**，然后单击**完成**添加该应用程序。

    ![应用程序][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

本节的目的是向您展示如何配置和测试 Azure AD 单一登录与@Task基于一个名为"Britta Simon"的测试用户。

单一登录工作，Azure 广告需要知道哪些对应的用户在@Task到 Azure AD 中的某个用户。 换句话说，Azure AD 用户和在相关的用户之间的链接关系@Task需要建立。   
此链接关系建立的 Azure AD 中的**用户名**的值中指定的**用户名**的值@Task。
 
配置和测试 Azure AD 单一登录与@Task,您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建@Tasktest用户](#creating-a-halogen-software-test-user)**-让 Britta Simon 中存在对应@Taskthat链接到她的 Azure 广告表示。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录在您@Task应用程序。

**Azure AD 单一登录的配置@Task,执行以下步骤︰**

1. 在 Azure 的传统门户网站，在**@Task**应用程序集成页面上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][6] 

2. 在**如何您希望用户在登录@Task**页上，选择**Azure AD 单一登录**，然后再单击**下一步**。

    ![Azure 的广告单登录][7] 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置应用程序设置][8] 
 
     一。 在**符号上 URL**文本框中，键入您登录到用户使用的 URL 您@Task应用程序 (例如︰*https://<Tenant name>.attask ondemand.com*)。

     b。 单击**下一步**。

4. 在**配置单一登录处@Task**页上，单击**下载元数据**、 将保存在您的计算机上的本地元数据文件，然后单击**下一步**。

    ![什么是 Azure AD 连接][9] 



1. 登录到您@Task公司网站以管理员的身份。

2. 转到**单一登录配置**。


1. 在**单一登录**对话框中，请执行以下步骤

    ![配置单一登录][23]

    一。 作为**类型**，选择**SAML 2.0**。

    b。 选择**服务提供商 ID**。

    c。 在 Azure 的传统门户网站，复制的**远程登录 URL**，，然后将其粘贴到**登录门户 URL**文本框。

    d。 在 Azure 的传统门户网站，复制**单个 Sign-Out 服务 URL**，，然后将其粘贴到**Sign-Out URL**文本框。

    电子。 在 Azure 的传统门户网站，复制的**更改密码 URL**，，然后将其粘贴到文本框中**更改密码 URL** 。

    f。 单击**保存**。

6. 在 Azure 的传统门户网站，选择单一登录配置进行确认，，然后单击**下一步**。 

    ![什么是 Azure AD 连接][10]

7. 在**单一登录确认**页上，单击**完成**。  

    ![什么是 Azure AD 连接][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。  

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。 

    ![Azure 广告测试用户](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰ 

    ![Azure 广告测试用户](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ 

    ![Azure 广告测试用户](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。
    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    一。 记下**新密码**的值。

    b。 单击**完成**。   

  
 
### <a name="creating-an-task-test-user"></a>创建@Task测试用户

本节的目的是创建一个用户调用 Britta Simon @Task。


**要创建用户调用 Britta Simon@Task,执行以下步骤︰**

1. 登录到您@Task公司网站以管理员的身份。

2. 在菜单上，单击**用户**。

3. 单击**新用户**。 

4. 在新建用户对话框中，请执行以下步骤︰

    ![创建@Task测试用户][21] 

    一。 在**名字**文本框中，键入"Britta"。

    b。 在**姓氏**文本框中，键入"Simon"。

    c。 在**电子邮件地址**文本框中，键入 Britta Simon Azure Active Directory 中的电子邮件地址。

    d。 单击**添加用户**。




### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 通过授予其访问权限使用 Azure 单一登录@Task。

![分配用户][200] 

**要分配到 Britta Simon@Task,执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**@Task**。

    ![分配用户][202] 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。  
当您单击@Task平铺盖板，您应该获取自动签名对您@Task应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






