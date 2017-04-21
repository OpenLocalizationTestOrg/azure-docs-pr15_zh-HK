<properties 
    pageTitle="教程︰ Azure Active Directory 集成与工作日 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 工作日启用单一登录、 自动化资源调配，和更多。" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>与工作日的教程︰ Azure Active Directory 集成
  
本教程的目的是显示 Azure 和工作日的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在工作日中一个租户
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的工作日
2.  配置单一登录
3.  配置用户设置
4.  配置用户设置

![方案](./media/active-directory-saas-workday-tutorial/IC782919.png "方案")

##<a name="enabling-the-application-integration-for-workday"></a>启用应用程序集成的工作日
  
本部分的目的是概述如何启用应用程序集成的队伍。

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>若要启用应用程序集成为工作日，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-workday-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-workday-tutorial/IC700994.png "应用程序")

4.  若要打开**应用程序库**，**添加应用程序**，请单击，然后单击**添加我的组织使用的应用程序**。

    ![要做什么？](./media/active-directory-saas-workday-tutorial/IC700995.png "要做什么？")

5.  在**搜索框**中，键入**工作日**。

    ![工作日](./media/active-directory-saas-workday-tutorial/IC701021.png "工作日")

6.  在结果窗格中，选择**工作日**，，然后单击**完成**添加应用程序。

    ![工作日](./media/active-directory-saas-workday-tutorial/IC701022.png "工作日")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够验证到工作日使用他们的帐户在 Azure 使用基于 SAML 协议联盟的广告。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在**工作日**应用程序集成页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782920.png "配置单一登录")

2.  在**您想怎样来登录到工作日的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782921.png "配置单一登录")

3.  在**配置应用程序 URL**页上，执行下面的步骤，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-workday-tutorial/IC782957.png "配置应用程序的 URL")

    一。 在**符号上 URL**文本框中，键入用户用于登录到使用以下模式的工作日的 URL:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b。  在**工作日答复 URL**文本框中，键入工作日答复 URL 使用以下模式︰`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]您回复的 URL 必须使用子域名 (例如︰ www，wd2，wd3，wd3 impl wd5、 wd5 实现)。 
    >使用类似于"*http://www.myworkday.com*"的工作，但是"*http://myworkday.com*"不。 
 
4.  在**配置单一登录在工作日**页中，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782922.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您工作日公司网站。

6.  转到**菜单\>工作台**。

    ![工作台](./media/active-directory-saas-workday-tutorial/IC782923.png "工作台")

7.  转到**帐户管理**。

    ![帐户管理](./media/active-directory-saas-workday-tutorial/IC782924.png "帐户管理")

8.  转到**编辑租户安装 – 安全**。

    ![编辑租户安全](./media/active-directory-saas-workday-tutorial/IC782925.png "编辑租户安全")

9.  在**重定向 Url**部分中，执行以下步骤︰

    ![重定向 Url](./media/active-directory-saas-workday-tutorial/IC7829581.png "重定向 Url")

    一。 单击**添加行**。

    b。 在**登录重定向 URL**文本框和**移动重定向 URL**文本框中，键入您 Azure 的传统门户网站的**配置应用程序 URL**页输入的**工作日租户 URL** 。
    
    c。 在 Azure 经典门户中，在**配置单一登录在工作日**对话框页复制**单个 Sign-Out 服务 URL**，，然后将其粘贴到**注销重定向 URL**文本框。

    d。  在**环境**文本框中，键入环境名称。  


    >[AZURE.NOTE] 环境属性的值与租户 URL 的值︰
    >
    >-   如果工作日租户的域名 URL 从开始实现 (如︰ *https://impl.workday.com/\<租户\>/login-saml2.htmld*)，**环境**属性必须设置为实现。
    >-   如果以停下手中的开头的域名，您需要联系工作日来获得匹配的**环境**价值。

10. 在**SAML 安装**部分中，执行以下步骤︰

    ![SAML 安装程序](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML 安装程序")

    一。  选择**启用 SAML 验证**。

    b。  单击**添加行**。

11. 在 SAML 身份提供程序部分中，执行以下步骤︰

    ![SAML 身份提供程序](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML 身份提供程序")

    一。 在身份提供程序名称文本框中，键入提供程序的名称 (例如︰ *SPInitiatedSSO*)。

    b。 在 Azure 经典门户中，在**配置单一登录在工作日**对话框页的**标识提供程序 ID**值，复制，然后将其粘贴到**颁发者**文本框。

    c。 选择**启用工作日 Initialted 注销**。

    d。 在 Azure 经典门户中，在**配置单一登录在工作日**对话框页复制**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**注销请求 URL**文本框。


    电子。 **公用密钥证书的标识提供程序**，请单击，然后单击**创建**。 

    ![创建](./media/active-directory-saas-workday-tutorial/IC782928.png "创建")

    f。 单击**创建 x509 公钥**。 
        
    ![创建](./media/active-directory-saas-workday-tutorial/IC782929.png "创建")


1. 在**查看 x509 公用密钥**部分中，执行以下步骤︰ 

    ![查看 x509 公共密钥](./media/active-directory-saas-workday-tutorial/IC782930.png "查看 x509 公共密钥") 

    一。 在**名称**文本框中，键入证书的名称 (例如︰ *PPE\_SP*)。
        
    b。 在**有效期自**文本框中，键入有效的从您的证书的属性值。
    
    c。  中的**失效日期**文本框中，键入您的证书的属性值无效。
        
    >[AZURE.NOTE] 您可以从日期和有效日期从下载证书通过双击它以获得有效。 在**详细信息**选项卡列出了日期。

    d。 创建您已下载的证书的**Base 64 编码**的文件。  

    >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    电子。  在记事本中打开您 base 64 编码的证书，然后复制其内容。
    
    f。  在**证书**文本框中，粘贴剪贴板中的内容。
    
    g。  单击**确定**。

12.  执行以下步骤︰ 

    ![SSO 配置](./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO 配置")

    一。  启用**x509 私钥对**。

    b。  在**服务提供商 ID**文本框中，键入**http://www.workday.com**。

    c。  选择**启用 SP 启动 SAML 验证**。

    d。  Azure 传统门户网站，在**配置单一登录在工作日**对话框页中的**单一登录服务 URL**值，复制，然后将其粘贴到**IdP SSO 服务 URL**文本框。
     
    电子。 选择**不地道 SP 启动身份验证请求**。

    f。 作为**身份验证请求签名方法**，选择**SHA256**。 
        
    ![身份验证请求签名方法](./media/active-directory-saas-workday-tutorial/IC782932.png "身份验证请求签名方法") 
 
    g。 单击**确定**。 
        
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. 在 Azure 经典门户中，**配置单一登录在工作日**在页上，单击**下一步**。 

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782934.png "配置单一登录")

13. 在**单一登录确认**页上，单击**完成**。 

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782935111.png "配置单一登录")



##<a name="configuring-user-provisioning"></a>配置用户设置
  
若要获取工作日中置备一个测试用户，您需要与工作日支持团队联系。  
工作日的支持团队将为您创建该用户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>若要将用户分配到工作日，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**工作日**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-workday-tutorial/IC782935.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-workday-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。