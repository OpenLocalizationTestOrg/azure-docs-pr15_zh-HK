<properties
    pageTitle="Internet Explorer 的疑难解答访问控制面板扩展名 |Microsoft Azure"
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

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Internet Explorer 的疑难解答访问控制面板扩展

这篇文章将帮助您解决以下问题︰

- 你不能在使用 Internet Explorer 时通过我的应用程序门户访问您的应用程序。
- 即使您已经安装该软件，您将看到"安装软件"消息。

如果您是管理员，请参阅︰[如何部署使用组策略的 Internet Explorer 访问控制面板扩展](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>运行诊断工具

您可以通过下载并运行该接入面板诊断工具诊断访问控制面板扩展的安装问题︰

1. [单击此处下载该诊断工具。](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. 打开该文件，然后按**提取所有**按钮。

    ![按提取所有](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. 然后按**提取**按钮以继续。

    ![按提取](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. 若要运行该工具，请右键单击该文件命名为**AccessPanelExtensionDiagnosticTool**，然后选择**用打开 > Microsoft Windows 基于脚本宿主**。

    ![打开与 > 基于 Microsoft Windows 的脚本宿主](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. 然后，您会看到以下诊断窗口，该窗口描述什么可能是错误的与您的安装。

    ![样本的诊断窗口](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. 单击"****"要让程序解决已经发现的问题。

7. 若要保存这些更改，关闭每个 Internet Explorer 窗口，，然后重新打开 Internet Explorer。<br />如果仍然无法访问您的应用程序，请尝试执行以下步骤。

##<a name="check-that-the-access-panel-extension-is-enabled"></a>检查启用了访问控制面板扩展

若要验证在 Internet Explorer 中启用了访问控制面板扩展名︰

1. 在 Internet Explorer 中单击窗口右上角中的**齿轮图标**。 然后选择**Internet 选项**。<br />(旧版本的 Internet Explorer 中您可以找到在**工具 > Internet 选项**。

    ![请转到工具 > Internet 选项](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. 在**程序**选项卡上单击，然后单击**管理加载项**按钮。

    ![单击管理加载项](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. 在此对话框中，选择**访问控制面板扩展名**，然后单击**启用**按钮。

    ![单击启用](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. 若要保存这些更改，关闭每个 Internet Explorer 窗口，然后重新打开 Internet Explorer。

##<a name="enable-extensions-for-inprivate-browsing"></a>启用扩展为 InPrivate 浏览

如果您使用的 InPrivate 浏览模式︰

1. 在 Internet Explorer 中单击窗口右上角中的**齿轮图标**。 然后选择**Internet 选项**。<br />(旧版本的 Internet Explorer 中您可以找到在**工具 > Internet 选项**。

    ![样本的诊断窗口](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. 转到**隐私**选项卡，然后**取消选中**复选框标记为**禁用工具栏和扩展 InPrivate 浏览启动时**</p>

    ![当 InPrivate 浏览开始取消选中禁用工具栏和扩展](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. 若要保存这些更改，关闭每个 Internet Explorer 窗口，然后重新打开 Internet Explorer。

##<a name="uninstall-the-access-panel-extension"></a>卸载访问控制面板扩展

若要从计算机上卸载访问控制面板扩展名︰

1. 在键盘上按**Windows 键**打开 「 开始 」 菜单。 当菜单打开时，您可以键入任何要执行的搜索。 键入"控制面板"，然后打开**控制面板**，它出现在搜索结果中。

    ![控制面板搜索](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. 在控制面板的右上角，将**通过视图**选项更改**大图标**。 然后查找并单击**程序和功能**按钮。

    ![激活视图以显示大图标](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. 从列表中，选择**访问控制面板扩展名**，然后单击**卸载**按钮上。

    ![单击卸载](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. 然后，您可以尝试安装扩展后，再次以查看问题是否已解决。

如果您遇到问题卸载扩展，您还可以删除使用[Microsoft 修复它](https://go.microsoft.com/?linkid=9779673)的工具。

## <a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [应用程序访问权限和单一登录使用 Azure 活动目录](active-directory-appssoaccess-whatis.md)
- [如何为 Internet Explorer 使用组策略部署访问控制面板扩展](active-directory-saas-ie-group-policy.md)
