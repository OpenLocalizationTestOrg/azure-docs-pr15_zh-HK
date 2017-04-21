<properties
    pageTitle="教程︰ Azure Active Directory 集成与 NetSuite |Microsoft Azure"
    description="了解如何使用 Azure Active Directory NetSuite 启用单一登录、 自动化资源调配，以及更多 ！"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>教程︰ 如何与 Azure Active Directory 集成 NetSuite

本教程将向您展示如何将 NetSuite 环境连接到 Azure 活动目录 (AD Azure)。 您将学习如何配置单一登录到 NetSuite、 如何启用自动的用户供应，并如何将用户有权访问 NetSuite 分配。 

##<a name="prerequisites"></a>系统必备组件

1. 若要通过[Azure 的传统门户网站](https://manage.windowsazure.com)访问 Azure Active Directory，首先必须有效 Azure 订购。

2. 必须具有管理员访问权限与[NetSuite](http://www.netsuite.com/portal/home.shtml)订阅。 对于这两个服务，您可以使用免费的试用帐户。

##<a name="step-1-add-netsuite-to-your-directory"></a>步骤 1︰ 添加到您的目录 NetSuite

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)，在左侧的导航窗格中，单击**活动目录**。

    ![从左侧的导航窗格中选择活动目录。][0]

2. 从**目录**列表中，选择您想要添加到 NetSuite 目录。

3. 在顶部的菜单中单击**应用程序**。

    ![单击应用程序。][1]

4. 单击页面底部的**添加**。

    ![单击添加以添加新的应用程序。][2]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![单击库中添加应用程序。][3]

6. 在**搜索框**中，键入**NetSuite**。 然后从结果中，选择**NetSuite**并单击**完成**添加该应用程序。

    ![添加 NetSuite。][4]

7. 您现在应该看到 NetSuite 快速启动页︰

    ![在 Azure 广告 NetSuite 的快速启动页][5]

##<a name="step-2-enable-single-sign-on"></a>步骤 2︰ 启用单一登录

1. 在 Azure 广告、 NetSuite，快速启动页上单击**配置单一登录**按钮。

    ![配置单一登录按钮][6]

2. 将会打开一个对话框，您会看到一个屏幕，询问"您想怎样用户能够登录到 NetSuite 吗？" **Azure AD Single Sign-on**，选择，然后单击**下一步**。

    ![选择 Azure 的广告单登录][7]

    > [AZURE.NOTE] 若要了解更多有关不同选项的有关单一登录方式进行登录，[请单击此处](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3. 在**配置应用程序设置**页上，将**回复 URL**字段中，键入 NetSuite 租户 URL 使用以下格式之一︰
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![在租户 URL 中键入][8]

4. 在**配置单一登录 NetSuite 在**页上，在**下载元数据**，请单击，然后将保存在您的计算机的本地证书文件。

    ![下载元数据。][9]

5. 在浏览器中，打开一个新的选项卡和 Netsuite 公司网站以管理员身份登录。

6. 在页面顶部的工具栏中，单击**设置**，然后单击**安装管理器**。

    ![转到安装管理器][10]

7. **设置任务**列表中，选择**集成**。

    ![转到集成][11]

8. 在**管理身份验证**部分中，单击**SAML 单一登录**。

    ![转到 SAML 单一登录][12]

9. 在**SAML 安装**页上，请执行以下步骤︰

    - Azure Active Directory 中的**远程登录 URL**值复制并粘贴 NetSuite 中的**身份提供程序的登录页**字段。

        ![SAML 设置页面。][13]

    - 在 NetSuite，选择**主要的身份验证方法**。

    - 对于标有**SAMLV2 的身份提供程序元数据**的字段，选择**上载 IDP 元数据文件**。 然后单击**浏览**上载您在步骤 4 中下载元数据文件。

        ![将元数据上载][16]

    - 单击**提交**。

9. 在 Azure 广告，选择单一登录配置确认复选框启用您上载到 NetSuite 证书。 然后单击**下一步**。

    ![检查确认复选框][14]

10. 在最后一页的对话框中，键入电子邮件地址如果您想要接收电子邮件通知错误和与这种单一登录配置维护相关的警告。 

    ![键入您的电子邮件地址。][15]

11. 单击**完成**关闭对话框。 接下来，单击顶部的页上的**属性**。

    ![单击属性。][17]

12. 单击**添加用户属性**。

    ![单击添加用户属性。][18]

13. 对于**属性名称**字段中，键入在`account`。 对于**属性值**字段中，键入在 NetSuite 帐户 id。 下文介绍了有关如何查找您的帐户 ID:

    ![添加 NetSuite 帐户 id。][19]

    - 在 NetSuite，从顶部导航菜单上单击**设置**。
    - 然后单击左边的导航菜单的**设置任务**部分下，选中**集成**部分中，单击**Web 服务首选项**。
    - 复制您 NetSuite 帐户 ID 和 Azure AD 中将其粘贴到**特性值**字段。

        ![获得您的帐户 ID][20]

14. 在 Azure 的广告，请单击**完成**以完成添加 SAML 特性。 然后单击底部菜单上的**应用更改**。

    ![保存您的更改。][21]

15. 用户可以执行单一登录 NetSuite 之前，他们必须首先分配 NetSuite 在适当的权限。 请按照下面的说明来分配这些权限。

    - 在顶部导航菜单中，单击**设置**，然后单击**安装管理器**。

        ![转到安装管理器][10]

    - 在左边的导航菜单中，选择**用户/角色**，然后单击**管理角色**。

        ![转到管理角色][22]

    - 单击**新角色**。

    - 键入一个**名称**为您新角色，并选择**单个登录仅**复选框。

        ![新角色的名称。][23]

    - 单击**保存**。

    - 在菜单上，单击**权限**。 然后单击**设置**。

        ![转到权限][24]

    - **设置了 SAM Single sign-on**，选择，然后单击**添加**。

    - 单击**保存**。

    - 在顶部导航菜单中，单击**设置**，然后单击**安装管理器**。

        ![转到安装管理器][10]

    - 在左边的导航菜单中，选择**用户/角色**，然后单击**管理用户**。

        ![转到要管理的用户][25]

    - 选择一个测试用户。 然后单击**编辑**。

        ![转到要管理的用户][26]

    - 在角色对话框中，选择您已创建并单击**添加**的角色。

        ![转到要管理的用户][27]

    - 单击**保存**。

19. 若要测试您的配置，请参阅标题[为用户分配 NetSuite](#step-4-assign-users-to-netsuite)下面的部分。

##<a name="step-3-enable-automated-user-provisioning"></a>第 3 步︰ 使用户自动化资源调配

> [AZURE.NOTE] 默认情况下，您已设置的用户将添加到根子公司 NetSuite 环境。

1. 在 Azure Active Directory NetSuite，快速启动页上单击**配置用户设置**。

    ![配置用户资源调配][28]

2. 在打开的对话框中，键入您的管理员凭据 NetSuite，然后单击**下一步**。

    ![键入您 NetSuite 管理员凭据。][29]

3. 在确认页上，键入您的电子邮件地址，以便接收的设置故障通知。

    ![请确认。][30]

4. 单击**完成**关闭对话框。

##<a name="step-4-assign-users-to-netsuite"></a>步骤 4︰ 将用户分配到 NetSuite

1. 若要测试您的配置，开始在该目录中创建新的测试帐户。

2. 在 NetSuite 快速起始页中，单击**分配用户**按钮。

    ![对指定用户单击][31]

3. 选择测试用户，然后单击**分配**按钮在屏幕的底部︰

 - 如果还没有启用自动的用户供应，然后您会看到以下提示︰

        ![Confirm the assignment.][32]

 - 如果您已经启用了资源调配的自动化的用户，您将看到提示定义用户应 NetSuite 在具有何种角色。 新调配的用户应 NetSuite 环境中出现几分钟的时间。

4. 要测试您的单一登录设置，请打开[https://myapps.microsoft.com](https://myapps.microsoft.com/)在访问权限面板，登录到测试帐户，单击**NetSuite**上。

##<a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [有关如何集成 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
