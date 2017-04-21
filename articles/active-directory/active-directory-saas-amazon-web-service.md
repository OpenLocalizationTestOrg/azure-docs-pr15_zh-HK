<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Amazon Web 服务 (AWS) |Microsoft Azure"
    description="了解如何使用 Azure Active Directory Amazon Web 服务 (AWS) 启用单一登录、 自动化资源调配，以及更多 ！"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>教程︰ Azure Active Directory 集成与 Amazon Web 服务 (AWS)

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 Amazon Web 服务 (AWS)。  
与 Azure AD 集成 Amazon Web 服务 (AWS) 提供了以下好处︰ 

- 您可以控制有权向 Amazon Web 服务 (AWS) Azure AD 中 
- 您可以其 Azure 的广告帐户使用户可以自动获取签名上到 Amazon Web 服务 (AWS) （单一登录）
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件 

要配置 Azure AD 集成与 Amazon Web 服务 (AWS)，您需要以下各项︰

- Azure 广告订阅
- Amazon Web 服务 (AWS) 单点登录启用订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。 

 
## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。  
在本教程中介绍的方案由三个主要的构造块组成︰

1. 从库中添加 Amazon Web 服务 (AWS) 
2. 配置和测试 Azure AD 单一登录


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>从库中添加 Amazon Web 服务 (AWS)
若要配置 Azure AD Amazon Web 服务 (AWS) 集成，您需要将 Amazon Web 服务 (AWS) 从库添加到托管的 SaaS 应用程序的列表。

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>要从库添加 Amazon Web 服务 (AWS)，请执行以下步骤︰

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1] 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。 
   
    ![应用程序][2]

4. 单击页面底部的**添加**。 
   
    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。 
   
    ![应用程序][4]

6. 在搜索框中，键入**Amazon Web 服务 (AWS)**。
   
    ![应用程序][5]

7. 在结果窗格中，选择**Amazon Web 服务 (AWS)**，，然后单击**完成**添加应用程序。

    ![应用程序][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和 Azure AD 单一登录测试与 Amazon Web 服务 (AWS) 根据一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道在 Azure AD 用户对应的用户在 Amazon Web 服务 (AWS)。 换句话说，Azure AD 用户和相关的用户在 Amazon Web 服务 (AWS) 之间的链接关系需要建立。  
此链接关系的建立在 Azure 广告作为**用户名**在 Amazon Web 服务 (AWS) 的值分配的**用户名**的值。
 
要配置和测试 Azure AD 单一登录 Amazon Web 服务 (AWS)，您需要完成以下构造块︰

1. **[Azure AD 配置一个 Single Sign-on](#configuring-azure-ad-single-single-sign-on)** -若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 Amazon Web 服务 (AWS) 测试用户](#creating-a-halogen-software-test-user)**-在 Amazon Web 服务 (AWS) 链接到她的 Azure 广告表示具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure 的广告单单一登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户启用并配置单一登录 Amazon Web 服务 (AWS) 应用程序中。  
Amazon Web 服务 (AWS) 应用程序需要以特定的格式，这就需要你对**saml 令牌属性**配置中添加自定义属性映射 SAML 断言。 下面的屏幕快照显示此示例。


![配置单一登录][27]

**Azure AD 单一登录配置与 Amazon Web 服务 (AWS)，请执行以下步骤︰**

1. 在 Azure 传统门户网站，在**Amazon Web 服务 (AWS)**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][7]

2. 在**您想怎样用户能够登录到 Amazon Web 服务 (AWS)**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录][8]

3. 在**配置应用程序设置**对话框页面上，单击下一步。 

    ![配置应用程序设置][9]
 
4. 在**配置单一登录在 Amazon Web 服务 (AWS)**页上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。

    ![配置单一登录][10]

5. 在不同的浏览器窗口中，登录到 Amazon Web 服务 (AWS) 公司网站以管理员的身份。

6. 单击**控制台主页**。

    ![配置单一登录][11]

7. 单击**标识和访问管理**。 

    ![配置单一登录][12]

8. **标识提供程序**，请单击，然后单击**创建提供程序**。 

    ![配置单一登录][13]

9. 在**配置提供程序**对话框页面上，请执行以下步骤︰ 

    ![配置单一登录][14]

     一。 作为**提供程序类型**，选择**SAML**。

     b。 在**提供程序名称**文本框中，键入提供程序的名称 (例如︰ *WAAD*)。

     c。 若要上载下载元数据文件，请单击**选择文件**。

     d。 单击**下一步**。


10. 在**验证提供程序信息**对话框页面上，单击**创建**。 

    ![配置单一登录][15]

11. 单击**角色**，然后单击**创建新角色**。 

    ![配置单一登录][16]

12. 在**设置角色名称**对话框中，请执行以下步骤︰ 

    ![配置单一登录][17]

    一。 在**角色名称**文本框中，键入角色名称 (例如︰ *TestUser*)。

    b。 单击**下一步**。

13. 在**选择角色类型**对话框中，请执行以下步骤︰ 

    ![配置单一登录][18]

    一。 选择**用于标识提供程序访问的角色**。

    b。 在**授予 Web 单一登录 (WebSSO) 对 SAML 提供程序的访问**部分中，单击**选择**。


14. 在**建立信任**对话框中，请执行以下步骤︰  

    ![配置单一登录][19]
     
     一。 作为 SAML 提供程序，请选择您已经创建了 previousley SAML 提供 (例如︰ *WAAD*) 

     b。 单击**下一步**。


15. 在**验证信任角色**对话框中，单击**下一步**。 

    ![配置单一登录][32]


16. 在**附加策略**对话框中，单击**下一步**。  

    ![配置单一登录][33]


17. 在**审阅**对话框中，请执行以下步骤︰   

    ![配置单一登录][34]

     一。 复制**角色 ARN**值。

     b。 复制的**受信任实体**ARN 值。

     c。 单击**创建角色**。 

18. 在 Azure 的传统门户网站，选择单一登录配置进行确认，，然后单击**下一步**。

    ![什么是 Azure AD 连接][20]

19. 在**单一登录确认**页上，单击**完成**关闭**配置单一登录**对话框。

    ![什么是 Azure AD 连接][22]


20. 在顶部菜单中，单击**属性**以打开**SAML 令牌的属性**对话框。 

    ![配置单一登录][21]

21. 单击**添加用户属性**。 

    ![配置单一登录][23]

22. 在添加用户属性对话框中，请执行以下步骤。 

    ![配置单一登录][24] 

    一。 在**属性名称**文本框中，键入**https://aws.amazon.com/SAML/Attributes/Role**。

    b。 在**属性值**文本框中，键入**[角色 ARN 值]，[受信任实体 ARN 值]**。

    >[AZURE.TIP] 这些是在您创建了您的角色已经从审阅对话框中复制的值。 

    c。 单击**完成**关闭**添加用户属性**对话框。

23. 单击**添加用户属性**。 

    ![配置单一登录][23]


24. 在添加用户属性对话框中，请执行以下步骤。 

    ![配置单一登录][25]


     一。 在**属性名称**文本框中，键入**https://aws.amazon.com/SAML/Attributes/RoleSessionName**。

     b。 在**属性值**文本框中，键入或从下拉列表中选择**user.userprincipalname** 。
     
    ![配置单一登录][35]
    

     c。 单击**完成**关闭**添加用户属性**对话框。


25. 单击**应用更改**。 

    ![配置单一登录][26]





### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户

本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。

![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。 

    ![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰ 

    ![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. 作为用户类型，选择您的组织中的新用户。
  2. 在用户名**文本框**中，键入**BrittaSimon**。
  3. 单击下一步。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ 

    ![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**最后一名**txtbox，类型， **Simon**。
  
    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。
  
    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    一。 记下**新密码**的值。
  
    b。 单击**完成**。   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Amazon Web 服务 (AWS) 测试用户

本部分的目的是创建用户调用 Britta Simon 在 Amazon Web 服务 (AWS)。

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>若要创建一个名为 Britta Simon 在 Amazon Web 服务 (AWS) 用户，请执行以下步骤︰

1. 以管理员身份登录到**Amazon Web 服务 (AWS)**公司网站。

2. 单击**控制台主页**图标。 

    ![配置单一登录][11]

3. 单击标识和访问权限管理。 

    ![配置单一登录][28]

4. 在仪表板中，单击用户，然后单击创建新用户。 

    ![配置单一登录][29]

5. 在创建用户对话框中，请执行以下步骤︰ 

    ![配置单一登录][30]

     一。 在**输入用户名**文本框，Azure AD 中键入 Brita Simon 的用户名 （范围内）。

     b。 单击**创建**。




### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 使用 Azure 单一登录通过授予其访问权限到 Amazon Web 服务 (AWS)。

![分配用户][31]

**若要分配 Britta Simon CloudPassage，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][26]

2. 在应用程序列表中，选择**Amazon Web 服务 (AWS)**。

    ![分配用户][27]

1. 在菜单上，单击**用户**。

    ![分配用户][25]

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][29]

### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。  
Amazon Web 服务 (AWS) 拼贴访问权限面板中的单击时，您应该获取自动签名对 Amazon Web 服务 (AWS) 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















