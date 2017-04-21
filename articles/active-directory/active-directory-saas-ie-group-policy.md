<properties
    pageTitle="如何为 Internet Explorer 使用组策略部署访问控制面板扩展名 |Microsoft Azure"
    description="如何使用组策略来部署我的应用程序门户的 Internet Explorer 加载项。"
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>如何为 Internet Explorer 使用组策略部署访问控制面板扩展

本教程展示如何使用组策略可远程安装接入面板扩展为 Internet Explorer 在用户的计算机上。 此扩展程序需要 Internet Explorer 用户需要登录到应用程序配置为使用[基于密码的单一登录](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)。

建议管理员自动执行此扩展的部署。 否则，用户必须下载并安装该扩展自己，这很容易导致用户错误和需要管理员权限。 本教程介绍了自动化通过使用组策略部署软件的一种方法。 [了解有关组策略的详细信息。](https://technet.microsoft.com/windowsserver/bb310732.aspx)

访问控制面板扩展还有[铬](https://go.microsoft.com/fwLink/?LinkID=311859)和[Firefox](https://go.microsoft.com/fwLink/?LinkID=626998)，都不需要管理员权限才能安装。

##<a name="prerequisites"></a>系统必备组件

- 您已经安装了[Active Directory 域服务](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)，并且您已加入到域的用户的计算机。
- 您必须具有"编辑设置"权限才能编辑组策略对象 (Gpo)。 默认情况下，下列安全组的成员具有此权限︰ 域管理员、 企业管理员和组策略创建者所有者。 [了解更多信息。](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>步骤 1︰ 创建分发点

首先，必须从所有您想要扩展远程安装的计算机可以访问的网络位置上放置的安装程序包。 若要执行此操作，请执行以下步骤︰

1. 以管理员身份登录到服务器

2. 在**服务器管理器**窗口中，转到**文件和存储服务**。

    ![打开的文件和存储服务](./media/active-directory-saas-ie-group-policy/files-services.png)

3. 请转到**共享**选项卡。 然后单击**任务**上的 > **新共享...**

    ![打开的文件和存储服务](./media/active-directory-saas-ie-group-policy/shares.png)

4. 完成**新加共享向导**并设置权限，以确保其可以从用户的计算机进行访问。 [了解有关共享的详细信息。](https://technet.microsoft.com/library/cc753175.aspx)

5. 下载以下 Microsoft Windows Installer 程序包 （.msi 文件）︰[访问面板 Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. 将安装包复制到共享上所需的位置。

    ![复制到您的.msi 文件已经共享。](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. 验证客户端计算机可以从共享位置访问安装程序包。 

##<a name="step-2-create-the-group-policy-object"></a>步骤 2︰ 创建组策略对象

1. 登录到承载 Active Directory 域服务 (AD DS) 安装的服务器上。

2. 在服务器管理器中，请转到**工具** > **组策略管理**。

    ![请转到工具 > 组策略管理](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. 在**组策略管理**窗口的左窗格中，查看您的组织单位 (OU) 层次结构，并确定在哪些范围内您想要应用组策略。 例如，您可能希望选择一个小型的 OU 部署到几个用户进行测试，或可能会选取顶级 OU 部署到整个组织。

    > [AZURE.NOTE] 如果您想要创建或编辑您的组织单位 (Ou) 切换回发到服务器管理器，请转到**工具** > **Active Directory 用户和计算机**。

4. 一旦选择了一个 OU，右键单击它并选择**创建此域中的 GPO 和此处链接...**

    ![创建新的 GPO](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. 在**新的 GPO**的提示时，键入新的组策略对象的名称。

    ![命名新的 GPO](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. 右击刚创建的组策略对象并选择**编辑**。

    ![编辑新的 GPO](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>步骤 3︰ 指定安装软件包

1. 确定是否要部署基于**计算机配置**或**用户配置**的扩展。 使用[计算机配置](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx)时，将不考虑哪个用户登录到该计算机上安装扩展。 另一方面，使用[用户配置](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx)，用户将拥有他们登录到哪些计算机无论安装扩展。

2. 在**组策略管理编辑器**窗口的左窗格中，转到以下文件夹路径，具体取决于哪种类型的配置，您选择的任何一个︰
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. 用鼠标右键单击**软件安装**，然后选择**新建** > **包...**

    ![创建一个新的软件安装包](./media/active-directory-saas-ie-group-policy/new-package.png)

4. 转到包含安装程序包的共享文件夹[步骤 1︰ 创建分发点](#step-1-create-the-distribution-point)，选择.msi 文件，然后单击**打开**。

    > [AZURE.IMPORTANT] 如果共享此同一台服务器上，请验证访问.msi 文件通过网络文件路径，而不是本地文件路径。

    ![从共享文件夹中选择安装包。](./media/active-directory-saas-ie-group-policy/select-package.png)

5. **部署软件**提示时，选择您的部署方法**分配**。 然后单击**确定**。

    ![选择分配，然后单击确定。](./media/active-directory-saas-ie-group-policy/deployment-method.png)

该扩展现在部署到您选择的 OU。 [了解有关组策略软件安装的详细信息。](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>步骤 4︰ 自动启用 Internet Explorer 的扩展名 

除了运行安装程序，必须显式启用 Internet Explorer 的每个扩展才能使用它。 请按照下面的步骤来启用访问面板扩展使用组策略︰

1. 在**组策略管理编辑器**窗口中，转到下列路径，具体取决于哪种类型的配置您选择中任一[第 3 步︰ 指定安装软件包](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. **加载项列表**中，右键单击并选择**编辑**。
    ![编辑加载项列表。](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. 在**加载项列表**窗口中，选择**已启用**。 然后，在**选项**部分中，单击**显示...**。

    ![单击启用，然后单击显示。.](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. 在**显示内容**窗口中，请执行以下步骤︰

    1. 第一列 （**值名称**字段），复制并粘贴下面的类 ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. 第二列 （**值**字段），键入以下值︰`1`

    3. 单击**确定**以关闭**显示内容**窗口中。

    ![如同上面所指定的值填充。](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. 单击**确定**以应用所做的更改并关闭**加载项列表**窗口。

现在扩展选定 OU 中的计算机已启用。 [了解有关使用组策略来启用或禁用 Internet Explorer 加载项的详细信息。](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>第 5 步 （可选）︰ 禁用"记住密码"提示

当用户登录到网站使用访问控制面板扩展时，Internet Explorer 可能会显示以下提示询问"要保存您的密码吗？"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

如果您希望防止用户看到此提示，然后按照下面的步骤来防止从记忆密码的自动完成︰

1. 在**组策略管理编辑器**窗口中，请转到下面列出的路径。 请注意，此配置设置仅可在**用户配置**下。
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. 查找名为**开启用户名称和密码在窗体上的自动完成功能**的设置。

    > [AZURE.NOTE] Active Directory 的以前版本可能会列出**不允许自动完成保存密码**的名称与此设置。 该设置的配置不同于在本教程中描述的设置。

    ![请记住在用户设置下查找此。](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. 上述设置，请右键单击并选择**编辑**。

4. 在名为**用户名和密码在窗体上的自动完成功能打开**的窗口，选择**已禁用**。

    ![选择禁用](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. 单击**确定**以应用这些更改并关闭窗口。

用户不能保存他们的凭据或使用自动完成访问以前存储的凭据。 但是，该策略允许用户继续为其他类型的表单域，例如搜索字段中使用自动完成。

> [AZURE.WARNING] 如果此策略被启用后用户选择用来存储某些凭据，该策略将*不*清除已存储的凭据。

##<a name="step-6-testing-the-deployment"></a>步骤 6︰ 测试部署

请按照下面的步骤来验证扩展部署是否成功︰

1. 如果您部署了使用**计算机配置**，登录到客户端计算机所属的 OU，您在中选择[步骤 2︰ 创建组策略对象](#step-2-create-the-group-policy-object)。 如果您部署了使用**用户配置**，请确保以属于该 OU 的用户身份登录。

2. 它可能需要几个单元的组策略更改为完全更新的符号与这台机器。 强制进行更新，打开一个**命令提示**窗口并运行以下命令︰`gpupdate /force`

3. 您将需要重新启动计算机以进行安装。 启动时可能需要更多的时间，比平常时扩展安装。

4. 重新启动后，打开**Internet Explorer**。 在窗口的右上角，**工具**（齿轮图标），请单击，然后选择**管理加载项**。

    ![请转到工具 > 管理加载项](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. 在**管理加载项**窗口中，验证已安装**访问面板扩展**程序已被其**状态**设置为**已启用**。

    ![验证安装并启用访问控制面板扩展名。](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [应用程序访问权限和单一登录使用 Azure 活动目录](active-directory-appssoaccess-whatis.md)
- [Internet Explorer 的疑难解答访问控制面板扩展](active-directory-saas-ie-troubleshooting.md)
