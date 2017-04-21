<properties
   pageTitle="使用 Azure 活动目录 B2B 协作预览的详细的演练 |Microsoft Azure"
   description="Azure 的活动目录 B2B 协作支持您跨公司的关系通过有选择地访问企业应用程序的业务合作伙伴"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Azure AD B2B 协作预览︰ 详细的演练

本演练介绍如何使用 Azure AD B2B 协作。 作为 Contoso 的 IT 管理员，我们想要从三个合作伙伴公司的员工与共享应用程序。 无伙伴公司需要具有 Azure 的广告。

- 刘爱琳从简单的伙伴组织
- 博波，从媒体伙伴组织，需要一套应用程序访问
- 刘英，从复杂的伙伴组织，需要一套应用程序和在 Contoso 的组中的成员访问

邀请被发送给合作伙伴用户之后，我们可以在 Azure 的广告来授予访问权限的应用程序和通过 Azure 门户网站组的成员身份配置它们。 让我们先将刘爱琳添加。

## <a name="adding-alice-to-the-contoso-directory"></a>将刘爱琳添加到 Contoso 目录
1. 如所示，只有 Alice 的**电子邮件**，**显示名称**和**InviteContactUsUrl**填充带有标题创建.csv 文件。 **显示名称**是在邀请中，显示的名称以及在 Contoso Azure 的广告目录中显示的名称。 **InviteContactUsUrl**是为刘爱琳联系 Contoso 的方法。 在以下示例中，InviteContactUsUrl 指定 Contoso 的 LinkedIn 配置文件。 请务必拼写中严格按指定[CSV 文件格式参考](active-directory-b2b-references-csv-file-format.md)中的.csv 文件的第一行的标签。  
![示例小红 CSV 文件](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. 在 Azure 的门户中，将用户添加到 Contoso 目录 (Active Directory > Contoso > 用户 > 添加用户)。 在"用户类型"下拉列表中，选择"合作伙伴公司中的用户"。 上传的.csv 文件。 请确保该.csv 文件已上载之前。  
![小红为 CSV 文件上传](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice 表示为 Contoso Azure 的广告目录中的外部用户。  
![刘爱琳已列入 Azure 的广告](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. 刘爱琳将收到下面的电子邮件。  
![对于小红的邀请电子邮件](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice 单击链接，并且她接受邀请，用她工作凭据登录提示。 如果 Alice 不是 Azure 的广告目录，Alice 是提示您注册。  
![注册之后 Alice 的邀请](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. 刘爱琳被重定向到应用程序接入面板，空，直到她被授予访问应用程序。  
![小红的接入面板](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

此过程使最简单的 B2B 协作形式。 Contoso Azure 的广告目录中的用户，Alice 可以被授予访问应用程序和通过 Azure 门户组。 现在让我们添加 Bob，需要为 Moodle 和销售队伍的应用程序的访问权限。

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Contoso 目录添加 Bob 以及授予对应用程序的访问
1. 使用 Windows PowerShell Azure AD 模块安装来查找应用程序 Id 的 Moodle 和销售队伍。 可以使用 cmdlet 检索 Id:`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`这将打开 Contoso 和其 AppPrincialIds 中的所有可用应用程序列表。  
![小明为检索 Id](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. 创建一个包含王俊元的电子邮件并显示名称、 **InviteAppID**、 **InviteAppResources**和 InviteContactUsUrl 的.csv 文件。 填充**InviteAppResources**与 AppPrincipalIds 的 Moodle 和发现从 PowerShell，由空格分隔的队伍。 填充**InviteAppId**具有相同 AppPrincipalId 的 Moodle 品牌电子邮件并在页中使用 Moodle 徽标签名。  
![Bob 的示例 CSV 文件](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. 就像它已经为刘爱琳上传通过 Azure 门户的.csv 文件。 Bob 现 Contoso Azure 的广告目录中的外部用户。

4. Bob 接收以下电子邮件。  
![Bob 的邀请电子邮件](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Bob 单击该链接，并提示用户接受邀请。 他签名后，他将直接转到访问权限面板中，已经可以使用 Moodle 和销售队伍。  
![Bob 的接入面板](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

我们将添加刘英接下来，需要访问的应用程序以及与 Contoso 目录中的组成员关系。

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Contoso 目录中添加刘英、 授予应用程序访问权限和授予组成员资格

1. 要查找的应用程序 Id 和组 Id Contoso 内部使用 Azure AD 模块使用 Windows PowerShell 安装。
 - 检索使用 cmdlet 的 AppPrincipalId `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`、 与 Bob 相同
 - 检索组使用 cmdlet 的 ObjectId `Get-MsolGroup | fl DisplayName, ObjectId`。 这将打开 Contoso 和其 ObjectIds 中的所有组的列表。 组 Id 还检索到 Azure 门户中的组的属性选项卡中的对象 id。  
![刘英的检索 Id 和组](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. 创建.csv 文件，填充刘英的电子邮件、 显示名称、 InviteAppID、 InviteAppResources、 **InviteGroupResources**和 InviteContactUsUrl。 **InviteGroupResources**是由 MyGroup1 和由空格分隔的外部对象的组的 ObjectIds 填充的。  
![刘英的示例 CSV 文件](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. 通过 Azure 门户的.csv 文件上载。

4. 刘英是 Contoso 目录中的用户，也是 MyGroup1 和外部对象，组的成员，在 Azure 门户中看到。  
![刘英在 Azure AD 组中列出](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. 刘英收到一封电子邮件，其中包含的链接接受邀请。 她登陆后，她被重定向到应用程序访问面板访问 Moodle 和销售队伍。  

这就是全部就是添加用户可向 Azure AD B2B 合作的伙伴企业。 本演练演示了如何添加用户 Alice 和 Bob，刘英到 Contoso 目录使用三个单独的.csv 文件。 此过程可更容易通过冷凝成一个文件的单独的.csv 文件。  
![Alice 和 Bob，刘英的示例 CSV 文件](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>相关的文章
浏览我们 Azure AD B2B 协作的其他文章︰

- [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
- [它的工作原理](active-directory-b2b-how-it-works.md)
- [CSV 文件格式参考](active-directory-b2b-references-csv-file-format.md)
- [外部用户令牌格式](active-directory-b2b-references-external-user-token-format.md)
- [外部用户对象属性的更改](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [当前预览限制](active-directory-b2b-current-preview-limitations.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
