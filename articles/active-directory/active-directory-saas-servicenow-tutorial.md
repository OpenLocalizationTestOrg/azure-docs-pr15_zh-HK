<properties
    pageTitle="教程︰ Azure Active Directory 集成与 ServiceNow 和 ServiceNow Express |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 ServiceNow 和 ServiceNow Express 之间。"
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


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>与 ServiceNow 和 ServiceNow Express 教程︰ Azure Active Directory 集成。


在本教程中，您将学习如何将 ServiceNow 和 ServiceNow Express 集成使用 Azure 活动目录 (AD Azure)。

将 ServiceNow 和 ServiceNow Express 集成使用 Azure 的广告提供了以下好处︰

- 您可以控制有权访问 ServiceNow 和 ServiceNow Express Azure AD 中
- 您可以使用户可以自动获取签名对 ServiceNow 和 ServiceNow Express （单一登录） 其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

若要配置与 ServiceNow 和 ServiceNow Express Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- ServiceNow、 实例或租户的 ServiceNow，卡尔加里版本或更高版本
- 为明确的 ServiceNow、 ServiceNow Express，赫尔辛基版本的实例或更高版本
- ServiceNow 租户必须启用[多个提供商单个符号上插件](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0)。 这可以通过提交在<https://hi.service-now.com/>的服务请求 


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。 在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 ServiceNow
2. 配置和测试 Azure AD 单一登录 ServiceNow 或 ServiceNow 高速芯片组


## <a name="adding-servicenow-from-the-gallery"></a>从库中添加 ServiceNow
若要配置的 ServiceNow 或 ServiceNow Express 集成到 Azure 的广告，您需要将 ServiceNow 从库添加到托管的 SaaS 应用程序的列表。 

**要从库添加 ServiceNow，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**ServiceNow**。

    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. 在结果窗格中，选择**ServiceNow**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本节中，您配置和使用 ServiceNow 或 ServiceNow Express Azure AD 单一登录测试基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 的广告需要知道对应的用户在 ServiceNow 中是什么给用户在 Azure 的广告。 换句话说，Azure AD 用户和 ServiceNow 中的相关的用户之间的链接关系需要建立。
此链接关系的建立在 Azure 广告作为**用户名**ServiceNow 中的值指定的**用户名**的值。 要配置和测试 Azure AD 单一登录使用 ServiceNow，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录的 ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** -若要使用户可以使用此功能。
2. **[Azure AD 配置单一登录的 ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** -若要使用户可以使用此功能。
3. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 ServiceNow 测试用户](#creating-a-servicenow-test-user)**的链接到她的 Azure 广告表示的 ServiceNow 中具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
6. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

> [AZURE.NOTE] 如果您想要配置 ServiceNow 跳过步骤 2。 同样，如果您想要配置 ServiceNow Express 忽略步骤 1。

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>针对 ServiceNow Azure 的广告单登录配置

1.  在 Azure AD 经典门户，在**ServiceNow**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749323.png "配置单一登录")

2.  在**您想怎样来登录到 ServiceNow 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749324.png "配置单一登录")

3.  在**配置应用程序设置**页中，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "配置应用程序的 URL")

    一。 在**ServiceNow 号上的 URL**文本框中，键入您登录到以下模式 ServiceNow 应用程序用户所使用的 URL: `https://<instance-name>.service-now.com`。

    b。 在**标识符**文本框中，键入您的 URL 由用户用来登录到 ServiceNow 应用程序遵循模式︰ `https://<instance-name>.service-now.com`。

    c。 单击**下一步**

4.  若要让自动基于 SAML 的身份验证配置 ServiceNow Azure AD，输入 ServiceNow 实例名称、 管理员用户名和管理员密码，在**自动配置单一登录**窗体中，单击*配置*。 请注意，管理员用户名提供必须具有**security_admin**角色分配在此的 ServiceNow 来处理。 否则为若要手动配置 ServiceNow Azure 广告作为 SAML 身份标识提供程序，请单击**手动配置为单一登录的应用程序**，然后单击**下一步**并完成以下步骤。

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "配置应用程序的 URL")



5.  在**配置单一登录 ServiceNow 在**页上，单击**下载证书**，保存在计算机上本地证书文件。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749325.png "配置单一登录")

1. 登录到您作为管理员的 ServiceNow 应用程序。
2. 按照下面的步骤来激活_集成的多个提供商单一登录安装_插件︰

    一。 在左侧导航窗格中，请转到**系统定义**的部分，然后单击**插件**。

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "激活插件")
    
    b。 _集成的多个提供商单个登录安装程序_搜索。

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "激活插件")

    c。 选择插件。 Rigth 单击，并选择**激活/升级**。
    
    d。 单击**激活**按钮。

2. 在左侧导航窗格中，单击**属性**。  

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "配置应用程序的 URL")


3. 在**多个提供程序 SSO 属性**对话框中，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "配置应用程序的 URL")

    一。 为**启用 SSO 的多个提供程序**，请选择**是**。

    b。 为**启用调试日志记录了 SSO 集成的多个提供程序**，选择**是**。

    c。 在**[用户] 字段表...**文本框中，键入**用户名**。

    d。 单击**保存**。



1. 在左侧导航窗格中，单击**x509 证书**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "配置单一登录")


1. 在**X.509 证书**对话框中，单击**新建**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "配置单一登录")


1. 在**X.509 证书**对话框中，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "配置单一登录")

    一。 单击**新建**。

    b。 在**名称**文本框中，键入您的配置的名称 (例如︰ **TestSAML2.0**)。

    c。 选择**活动**。

    d。 作为**格式**，请选择**PEM**。

    电子。 作为**类型**，选择**信任存储证书**。
    
    f。 在记事本中打开您的 Base64 编码证书，将其内容复制到剪贴板，然后将其粘贴到**PEM 证书**文本框。

    g。 单击**更新**。


1. 在左侧导航窗格中，单击**身份提供程序**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "配置单一登录")

1. 在**标识提供程序**对话框中，单击**新建**:

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "配置单一登录")


1. 在**身份提供程序**对话框中，单击**SAML2 Update1？**:

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "配置单一登录")


1. 在 SAML2 Update1 属性对话框中，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "配置单一登录")


    一。 在**名称**文本框中，键入您的配置的名称 (例如︰ **SAML 2.0**)。

    b。 中的**用户字段**文本框、 类型**的电子邮件**或**user_id**，具体取决于该字段用于唯一地标识 ServiceNow 部署中的用户。 
    
    > [AZURE.NOTE] Configue Azure 广告转到发出 SAML 令牌中的唯一标识符的 Azure AD 用户 ID （用户主体名称） 或电子邮件地址，您可以**ServiceNow > 属性 > 单一登录**Azure 的传统门户网站，将所需的字段映射到**nameidentifier**属性的部分。 在 Azure AD （例如用户主体名称） 中所选属性存储的值必须匹配的输入字段 (如 user_id) 存储在 ServiceNow 中的值

    c。 在 Azure AD 经典门户中，**标识提供程序 ID**值，复制，然后将其粘贴**标识提供程序 URL**文本框。

    d。 在 Azure AD 经典门户中，将**身份验证请求 URL**值，复制，然后将其粘贴到**身份标识提供程序的 AuthnRequest**文本框。

    电子。 在 Azure AD 经典门户中，复制的**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**身份标识提供程序的 SingleLogoutRequest**文本框。

    f。 在**ServiceNow 主页**文本框中，键入您的 ServiceNow 实例主页的 URL。

    > [AZURE.NOTE] ServiceNow 实例主页是您的**ServieNow 租户 URL**和**/navpage.do**的串联 (例如︰`https://fabrikam.service-now.com/navpage.do`)。
 

    g。 在**的实体标识 / 颁发者**文本框中，键入 ServiceNow 租户的 URL。

    h。 在**观众的 URL**文本框中，键入 ServiceNow 租户的 URL。 

    我。 在**IDP 的 SingleLogoutRequest 的协议绑定**的文本框中，键入**urn: oasis︰ 姓名︰ tc: SAML:2.0:bindings:HTTP-重定向**。

    j。 在 NameID 策略文本框中，键入**urn: oasis︰ 姓名︰ tc: SAML:1.1:nameid 的格式︰ 未指定**。

    k。 取消选择**创建 AuthnContextClass**。

    l。 在**AuthnContextClassRef 方法**中，键入`http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`。 这才需要使用如果您是云的唯一组织。 如果您正在使用内部部署 ADFS 或 MFA 用于身份验证，则不应该配置此值。 

    米。 在**时钟相位差**文本框中，键入**60**。

    n。 为**单个脚本签名上**，选择**MultiSSO_SAML2_Update1**。

    o。 作为**x509 证书**，选择上一步中创建的证书。

    p。 单击**提交**。 



6. 在 Azure AD 传统门户网站，选择单一登录配置进行确认，，然后单击**下一步**。 

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "配置单一登录")

7. 在**单一登录确认**页上，单击**完成**。
 
    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "配置单一登录")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>针对 ServiceNow Express Azure 的广告单登录配置

1.  在 Azure AD 经典门户，在**ServiceNow**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749323.png "配置单一登录")

2.  在**您想怎样来登录到 ServiceNow 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749324.png "配置单一登录")

3.  在**配置应用程序设置**页中，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "配置应用程序的 URL")

    一。 在**ServiceNow 号上的 URL**文本框中，键入您登录到以下模式 ServiceNow 应用程序用户所使用的 URL: `https://<instance-name>.service-now.com`。

    b。 在**颁发者 URL**文本框中，键入您的 URL 由用户用来登录到 ServiceNow 应用程序按照该模式`https://<instance-name>.service-now.com`。

    c。 单击**下一步**

4.  单击**手动配置为单一登录的应用程序**，然后单击**下一步**，请完成以下步骤。

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "配置应用程序的 URL")

5.  在**配置单一登录 ServiceNow 在**页上，单击**下载证书**，保存在您的计算机的本地证书文件，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749325.png "配置单一登录")

6. 登录到 ServiceNow Express 作为管理员应用程序。

7. 在左侧导航窗格中，单击**单一登录**。  

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "配置应用程序的 URL")


8. 在**单一登录**对话框中，单击右上方的配置图标并设置以下属性︰

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "配置应用程序的 URL")

    一。 切换到右边的**启用多个提供程序 SSO** 。

    b。 切换到右边的**启用调试日志记录的多个提供程序 SSO 集成**。

    c。 在**[用户] 字段表...**文本框中，键入**用户名**。


9. 在**单一登录**对话框中，单击**添加新证书**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "配置单一登录")



10. 在**X.509 证书**对话框中，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "配置单一登录")

    一。 在**名称**文本框中，键入您的配置的名称 (例如︰ **TestSAML2.0**)。

    b。 选择**活动**。

    c。 作为**格式**，请选择**PEM**。

    d。 作为**类型**，选择**信任存储证书**。

    电子。 创建您已下载的证书的 Base64 编码文件。
   
    > [AZURE.NOTE] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。
    
    f。 在记事本中打开您的 Base64 编码证书，将其内容复制到剪贴板，然后将其粘贴到**PEM 证书**文本框。

    g。 单击**更新**。


11. 在**单一登录**对话框中，单击**添加新 IdP**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "配置单一登录")


12. 在**添加新身份标识提供程序**对话框中，**配置标识提供程序**，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "配置单一登录")


    一。 在**名称**文本框中，键入您的配置的名称 (例如︰ **SAML 2.0**)。

    b。 在 Azure AD 经典门户中，**标识提供程序 ID**值，复制，然后将其粘贴**标识提供程序 URL**文本框。

    c。 在 Azure AD 经典门户中，将**身份验证请求 URL**值，复制，然后将其粘贴到**身份标识提供程序的 AuthnRequest**文本框。

    d。 在 Azure AD 经典门户中，复制的**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**身份标识提供程序的 SingleLogoutRequest**文本框。

    电子。 作为**标识提供者证书**，选择上一步中创建的证书。


13. 单击**高级设置**，并在**身份提供程序的其他属性**，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "配置单一登录")

    一。 在**IDP 的 SingleLogoutRequest 的协议绑定**的文本框中，键入**urn: oasis︰ 姓名︰ tc: SAML:2.0:bindings:HTTP-重定向**。

    b。 在**NameID 策略**文本框中，键入**urn: oasis︰ 姓名︰ tc: SAML:1.1:nameid 的格式︰ 未指定**。    

    c。 在**AuthnContextClassRef 方法**中，键入**http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**。
    
    d。 取消选择**创建 AuthnContextClass**。

14. 在**其他服务提供程序属性**，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "配置单一登录")

    一。 在**ServiceNow 主页**文本框中，键入您的 ServiceNow 实例主页的 URL。

    > [AZURE.NOTE] ServiceNow 实例主页是您的**ServieNow 租户 URL**和**/navpage.do**的串联 (例如︰ `https://fabrikam.service-now.com/navpage.do`)。

    b。 在**的实体标识 / 颁发者**文本框中，键入 ServiceNow 租户的 URL。

    c。 在**观众 URI**文本框中，键入 ServiceNow 租户的 URL。 

    d。 在**时钟相位差**文本框中，键入**60**。

    电子。 中的**用户字段**文本框、 类型**的电子邮件**或**user_id**，具体取决于该字段用于唯一地标识 ServiceNow 部署中的用户。
    
    > [AZURE.NOTE] Configue Azure 广告转到发出 SAML 令牌中的唯一标识符的 Azure AD 用户 ID （用户主体名称） 或电子邮件地址，您可以**ServiceNow > 属性 > 单一登录**Azure 的传统门户网站，将所需的字段映射到**nameidentifier**属性的部分。 在 Azure AD （例如用户主体名称） 中所选属性存储的值必须匹配的输入字段 (如 user_id) 存储在 ServiceNow 中的值

    f。 单击**保存**。 


15. 在 Azure AD 传统门户网站，选择单一登录配置进行确认，，然后单击**下一步**。 

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "配置单一登录")

16. 在**单一登录确认**页上，单击**完成**。
 
    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
本部分的目的是概述如何启用 Active Directory 用户帐户添加到 ServiceNow 的用户供应。


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1. 在 Azure 管理经典门户，在**ServiceNow**应用程序集成页上，单击**配置用户设置**。 

    ![用户资源调配](./media/active-directory-saas-servicenow-tutorial/IC769498.png "用户资源调配")


2. 在**输入 ServiceNow 启用自动用户提供的凭据**页提供以下配置设置︰ 配置用户的资源调配 

     一。 在**ServiceNow 实例名称**文本框中，键入 ServiceNow 实例名称。

     b。 在**ServiceNow 管理员用户名**文本框中，键入 ServiceNow 管理员帐户的名称。

     c。 在**ServiceNow 管理员密码**文本框中，键入此帐户的密码。

     d。 单击**验证**来验证您的配置。

     电子。 单击**下一步**按钮以打开**后续步骤**页。

     f。 如果您想要设置为此应用程序的所有用户，请选择"都**自动资源都调配到此应用程序目录中的所有用户帐户**"。 

    ![下一步行动](./media/active-directory-saas-servicenow-tutorial/IC698804.png "下一步行动")

     g。 在**接下来的步骤**页面上，单击**完成**以保存配置。

### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰
 
    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   


### <a name="creating-a-servicenow-test-user"></a>ServiceNow 测试用户

在本节中，您创建一个名 Britta Simon ServiceNow 中的用户。 在本节中，您创建一个名 Britta Simon ServiceNow 中的用户。 如果您不知道如何在 ServiceNow 或 ServiceNow Express 帐户中添加用户，请联系 ServiceNow 的支持团队。

### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon ServiceNow 授予她访问使用 Azure 单一登录。

![分配用户][200] 

**若要分配 Britta Simon ServiceNow，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**ServiceNow**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在所有用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。

单击访问权限面板中的 ServiceNow 平铺时，您应该获取自动签名对 ServiceNow 应用程序。

## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
