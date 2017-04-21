<properties
    pageTitle="配置组设置 azure Active Directory cmdlet |Microsoft Azure"
    description="如何管理组使用 Azure Active Directory cmdlet 的设置。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>配置组设置 azure Active Directory cmdlet

可以在您的目录配置统一组的以下设置︰

1.  分类︰ 用户可以设置组的分类的列表，以逗号分隔。 示例将"分类"、"机密"和"最高机密"。

2.  使用准则网址︰ 指向用户的使用条款使用统一组，由您的组织定义一个 URL。 此 URL 将显示在用户界面中的用户使用组的位置。

3.  创建启用分组︰ 是否无，允许某些用户或所有用户创建统一的组。 设置为 on 时，所有用户都可以都创建组。 当设置为 off 时，任何用户都可以不创建组。 当关闭，您还可以指定安全组仍允许创建其用户组。

这些设置被配置使用一个设置和 SettingsTemplate 对象。 最初，不会看到在您的目录中的任何设置对象。 这意味着您的目录配置的默认设置。 若要更改默认设置，您将创建新的设置对象使用的设置的模板。 由 Microsoft 定义设置模板。

您可以下载包含对这些操作从[Microsoft 连接站点](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)使用的 cmdlet 的模块。

## <a name="create-settings-at-the-directory-level"></a>在目录级别创建设置

这些步骤创建设置在目录级别应用于目录中的所有 Office 组。

1. 如果您不知道要使用哪个 SettingTemplate，此 cmdlet 返回设置模板的列表︰

    `Get-MsolAllSettingTemplate`

    ![设置模板的列表](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. 若要添加使用准则 URL，首先您需要获取 SettingsTemplate 对象定义使用准则 URL 值;也就是说，Group.Unified 模板︰

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. 接下来，创建一个基于该模板的新设置对象︰

    `$setting = $template.CreateSettingsObject()`

4. 然后，更新该用法的准则值︰

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. 最后，应用设置︰

    `New-MsolSettings –SettingsObject $setting`

    ![添加使用准则 URL](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

以下是 Group.Unified SettingsTemplate 中定义的设置。

 **设置**                          | **说明**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>类型︰ 字符串<li>默认设置:""                  | 可以应用于统一组的有效分类值的逗号分隔列表。                
 <ul><li>EnableGroupCreation<li>类型︰ 布尔型<li>默认值︰ True              | 指示是否允许创建统一组目录中该标志。                               
 <ul><li>GroupCreationAllowedGroupId<li>类型︰ 字符串<li>默认设置:""         | 允许创建统一组安全组的 GUID 即使 EnableGroupCreation = = false。
 <ul><li>UsageGuidelinesUrl<li>类型︰ 字符串<li>默认设置:""                  | 到了组使用指南的链接。                                                                       

## <a name="read-settings-at-the-directory-level"></a>读取目录级别设置

这些步骤读取目录级别设置应用于目录中的所有 Office 组。

1. 读取目录的所有现有设置︰

    `Get-MsolAllSettings`

2. 阅读特定组的所有设置，请︰

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. 阅读特定目录的设置，使用设置 Id 的 GUID:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![设置标识的 GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>更新目录级别设置

下列步骤更新目录级别设置应用于目录中的所有 Office 组。

1. 获取现有设置对象︰

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. 获取要更新的值︰

    `$value = $Setting.GetSettingsValue()`

3. 更新的值︰

    `$value["AllowToAddGuests"] = "false"`

4. 更新的设置︰

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>删除目录级别设置

此步骤将删除目录级别设置应用于目录中的所有 Office 组。

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Cmdlet 语法引用

在[Azure 活动目录 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=808260)，您可以找到更多 Azure 活动目录 PowerShell 文档。

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>SettingsTemplate 对象引用 （Group.Unified SettingsTemplate 对象）

- "name":"EnableGroupCreation"，"类型":"System.Boolean"，"默认值":"真"、"说明":"一个布尔标志，指示上是否统一组创建功能。"

- "name":"GroupCreationAllowedGroupId"，"类型":"System.Guid"，"默认值":""，"说明":"安全组的白名单来创建统一组 GUID"。

- "name":"ClassificationList"，"类型":"System.String"，"默认值":""，"说明":"以逗号分隔的列表可以应用于同一组的有效分类值。"

- "name":"UsageGuidelinesUrl"，"类型":"System.String"，"默认值":""，"说明":"到了组使用指南的链接。"

名称 | 类型 | 默认值 | 说明
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "真正"  | "一个布尔标志，指示上是否统一组创建功能。"
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "安全组的白名单来创建统一组 GUID"。
"ClassificationList"  | "System.String"  | ""  | "以逗号分隔的列表可以应用于统一组的有效分类值。"
"UsageGuidelinesUrl"  | "System.String"  | ""  | "到了组使用指南的链接。"

## <a name="next-steps"></a>下一步行动

在[Azure 活动目录 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=808260)，您可以找到更多 Azure 活动目录 PowerShell 文档。

[Rob 的组博客](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad)提供了从 Microsoft 程序经理 Rob de Jong 的其他指令。

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
