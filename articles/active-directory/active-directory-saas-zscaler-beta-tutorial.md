<properties 
    pageTitle="教程︰ 与 Zscaler 测试版的 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Zscaler 测试版启用单一登录、 自动化资源调配，和更多。" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>教程︰ 与 Zscaler 测试版的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 ZScaler 测试版的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   ZScaler 测试版单一登录启用了订阅
  
学完本教程后, 已分配给 ZScaler 测试版的 Azure AD 用户将能够到 ZScaler Beta 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的 ZScaler 测试版
2.  配置单一登录
3.  配置代理服务器设置
4.  配置用户设置
5.  分配用户

![方案](./media/active-directory-saas-zscaler-beta-tutorial/IC800223.png "方案")

##<a name="enabling-the-application-integration-for-zscaler-beta"></a>启用应用程序集成的 ZScaler 测试版
  
本部分的目的是概述如何启用 ZScaler 测试版的应用程序集成。

###<a name="to-enable-the-application-integration-for-zscaler-beta-perform-the-following-steps"></a>若要启用应用程序集成的 ZScaler 测试版，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-zscaler-beta-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-zscaler-beta-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-zscaler-beta-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-zscaler-beta-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**ZScaler 测试版**。

    ![应用程序库](./media/active-directory-saas-zscaler-beta-tutorial/IC800224.png "应用程序库")

7.  在结果窗格中，选择**ZScaler 测试版**，，然后单击**完成**添加应用程序。

    ![ZScaler 一](./media/active-directory-saas-zscaler-beta-tutorial/IC800216.png "ZScaler 一")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ZScaler 测试版。  
作为此过程的一部分，您都需要将 base 64 编码的证书上传到 ZScaler Beta 租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**ZScaler 测试版**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-zscaler-beta-tutorial/IC800225.png "配置单一登录")

2.  在**您希望如何登录到 ZScaler 测试版的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-zscaler-beta-tutorial/IC800226.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**ZScaler 测试版号在 URL**文本框中，键入您登录到 ZScaler 测试版应用程序，用户使用的 URL，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-zscaler-beta-tutorial/IC800227.png "配置应用程序的 URL")

    >[AZURE.NOTE] 您可以实际值为您的环境的 ZScaler 测试版的支持团队如果您需要它。

4.  在**配置单一登录 ZScaler 测试版在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-zscaler-beta-tutorial/IC800228.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 ZScaler Beta 公司网站。

6.  在菜单上，单击**管理**。

    ![管理](./media/active-directory-saas-zscaler-beta-tutorial/IC800206.png "管理")

7.  下**管理管理员和角色**，单击**管理用户和身份验证**。

    ![管理用户和身份验证](./media/active-directory-saas-zscaler-beta-tutorial/IC800207.png "管理用户和身份验证")

8.  在**选择您的组织的身份验证选项**部分中，执行以下步骤︰

    ![身份验证](./media/active-directory-saas-zscaler-beta-tutorial/IC800208.png "身份验证")

    1.  选择**身份验证使用 SAML 单一登录**。
    2.  单击**配置 SAML 单一登录参数**。

9.  **配置 SAML 单一登录参数**对话框页上，执行下面的步骤，然后单击**完成**:

    ![单一登录](./media/active-directory-saas-zscaler-beta-tutorial/IC800209.png "单一登录")

    1.  在 Azure 经典门户中，在**配置单一登录 ZScaler 测试版在**对话框页上，将**身份验证请求 URL**值，复制，然后将其粘贴到**进行身份验证的用户发送到 SAML 门户网站的 URL**文本框。
    2.  在**属性包含登录名**文本框中，键入**NameID**。
    3.  若要上载您已下载的证书，请单击**Zscaler pem**。
    4.  选择**启用 SAML 自动资源调配**。

10. 在**配置用户身份验证**对话框页面上，请执行以下步骤︰

    ![管理](./media/active-directory-saas-zscaler-beta-tutorial/IC800210.png "管理")

    1.  单击**保存**。
    2.  单击**立即激活**。

11. 在 Azure 经典门户中，在**配置单一登录 ZScaler 测试版在**对话框页面上，选择单一登录配置进行确认，，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-zscaler-beta-tutorial/IC800229.png "配置单一登录")

##<a name="configuring-proxy-settings"></a>配置代理服务器设置

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>要在 Internet Explorer 中配置代理服务器设置

1.  启动**Internet Explorer**。

2.  从**工具**菜单上打开**Internet 选项**对话框中选择**Internet 选项**。

    ![Internet 选项](./media/active-directory-saas-zscaler-beta-tutorial/IC769492.png "Internet 选项")

3.  单击**连接**选项卡。

    ![连接](./media/active-directory-saas-zscaler-beta-tutorial/IC769493.png "连接")

4.  单击**局域网设置**以打开**局域网设置**对话框。

5.  在代理服务器部分中，执行以下步骤︰

    ![代理服务器](./media/active-directory-saas-zscaler-beta-tutorial/IC769494.png "代理服务器")

    1.  选择为 LAN 使用代理服务器。
    2.  在地址文本框中，键入**gateway.zscalerBeta.net**。
    3.  在端口文本框中，键入**80**。
    4.  选择**对于本地地址不使用代理服务器**。
    5.  单击**确定**以关闭**局域网 (LAN) 设置**对话框。

6.  单击**确定**以关闭**Internet 选项**对话框。

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 ZScaler 测试版，他们必须到 ZScaler 测试版提供。  
在 ZScaler 测试版资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  登录到**Zscaler**租户。

2.  单击**管理**。

    ![管理](./media/active-directory-saas-zscaler-beta-tutorial/IC781035.png "管理")

3.  单击**用户管理**。

    ![添加](./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "添加")

4.  在**用户**选项卡上，单击**添加**。

    ![添加](./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "添加")

5.  在添加用户部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-zscaler-beta-tutorial/IC781038.png "添加用户")

    1.  键入**用户 Id**、**显示用户名**、**密码**、**确认密码**，然后选择**组**和您希望提供一个有效的 AAD 帐户**部门**。
    2.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 ZScaler 测试版用户帐户创建工具或 Api 提供的 ZScaler 测试版来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-zscaler-beta-perform-the-following-steps"></a>若要将用户分配到 ZScaler 测试版，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**ZScaler 测试版**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-zscaler-beta-tutorial/IC800230.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-zscaler-beta-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。