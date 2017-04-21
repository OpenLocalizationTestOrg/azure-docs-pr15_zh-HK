<properties
   pageTitle="Azure 活动目录 B2B 协作预览的 CSV 文件格式 |Microsoft Azure"
   description="Azure 的活动目录 B2B 支持您跨公司的关系通过有选择地访问企业应用程序的业务合作伙伴"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure AD B2B 协作预览︰ CSV 文件的格式

Azure AD B2B 协作的预览版本要求指定合作伙伴的用户信息，通过 Azure 广告门户上载 CSV 文件。 CSV 文件应包含所需的标签下面，并根据需要的可选字段。 修改示例 CSV 文件中的 （见下文），而无需更改的第一行中的标签的拼写正确。

>[AZURE.NOTE] 标签 （如电子邮件、 显示名称，等等） 的第一行是成功分析该 CSV 文件的必要。 拼写检查必须匹配以下指定的字段。 这些标签标识内容之下。 对于不需要的可选字段，可以从 CSV 文件删除它们的标签。 整列可以留空。

## <a name="required-fields-br"></a>必填的字段︰ <br/>
**电子邮件︰**被邀请的用户的电子邮件地址。 <br/>
**显示名称︰**所邀请的用户 （通常情况下，第一个和最后一个名称） 的显示名称。<br/>


## <a name="optional-fields-br"></a>可选字段︰ <br/>

**InvitationText:**在 app 品牌之后和之前兑换链接自定义邀请电子邮件文本。<br/>
**InvitedToApplications:**企业应用程序将用户分配到 AppIDs。 AppIDs 是继续检索，则可以通过调用`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:**若要将用户添加到组的 ObjectIDs。 ObjectIDs 是继续检索，则可以通过调用`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:**在接受邀请后直接邀请的用户的 URL。 这应该是一个特定于公司的 URL （如[*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/))。 如果未指定此可选字段，所邀请的用户将被定向到用户可以导航到您选定的企业应用程序的应用程序接入面板中。 应用程序访问面板 URL 的形式`https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`。<br/>
**CcEmailAddress**︰ 电子邮件地址将通过电子邮件发送的邀请。 如果使用 CcEmailAddress 字段，则此邀请不能用于电子邮件验证用户或客户端创建。<br/>
**语言︰**邀请电子邮件和兑换经验，用"en"（英语） 时未指定的默认语言。 其他 10 支持的语言代码是︰<br/>
1. 特拉华州︰ 德语<br/>
2. es︰ 西班牙语<br/>
3. fr︰ 法语<br/>
4. 它︰ 意大利语<br/>
5. ja︰ 日语<br/>
6. ko︰ 朝鲜语<br/>
7. PT-BR︰ 葡萄牙语 （巴西）<br/>
8. ru︰ 俄语<br/>
9. zh HANS︰ 简体中文<br/>
10. 此不同︰ 中文 （繁体)<br/>

## <a name="sample-csv-file"></a>CSV 文件示例
下面是一个示例可以修改的 CSV。

>[AZURE.NOTE] 复制并粘贴到记事本，用.csv 文件扩展名保存它。 然后在 Excel 中编辑此。 它将组织成多个表中第一行的标签。

> 通过指定的标签和填充下面各列在 Excel 中添加多个可选字段。

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>相关的文章
浏览我们 Azure AD B2B 协作的其他文章

- [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
- [它的工作原理](active-directory-b2b-how-it-works.md)
- [详细的演练](active-directory-b2b-detailed-walkthrough.md)
- [外部用户令牌格式](active-directory-b2b-references-external-user-token-format.md)
- [外部用户对象属性的更改](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [当前预览限制](active-directory-b2b-current-preview-limitations.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
