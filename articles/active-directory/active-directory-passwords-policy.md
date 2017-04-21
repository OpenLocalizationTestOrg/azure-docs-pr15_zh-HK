<properties
    pageTitle="密码策略和限制在 Azure Active Directory |Microsoft Azure"
    description="描述了适用于在 Azure Active Directory，包括允许的字符、 长度和过期的密码策略"
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
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>密码策略和 Azure Active Directory 中的限制

本文介绍了密码策略并与存储在 Azure 的广告目录中的用户帐户相关的复杂性要求。

> [AZURE.IMPORTANT] **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>应用于所有用户帐户的范围内策略

需要登录到 Azure AD 身份验证系统的每个用户帐户必须具有与该帐户相关的唯一的用户主体名称 (UPN) 特性值。 下表轮廓策略将应用于部署 Active Directory 源用户帐户 （同步到云） 和云专用的用户帐户。

|   属性           |     范围内的要求  |
|   ----------------------- |   ----------------------- |
|  允许的字符    |  <ul> <li>A – Z</li> <li>-z </li><li>0-9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  不允许使用的字符  | <ul> <li>任何'@'将域中的用户名不分开的字符。</li> <li>不能包含句点字符 '。 前面'@'符号</li></ul> |
| 长度限制  |       <ul> <li>总长度不能超过 113 个字符</li><li>前的 64 个字符‘@’符号</li><li>在之后的 48 个字符‘@’符号</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>仅适用于云用户帐户的密码策略

下表描述了可应用于用户帐户的创建和管理在 Azure AD 中的可用的密码策略设置。

|  属性       |    要求          |
|   ----------------------- |   ----------------------- |
|  允许的字符   |   <ul><li>A – Z</li><li>-z </li><li>0-9</li> <li>@# $ % ^ & * - _ ! + =] {} & #124;\ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  不允许使用的字符   |       <ul><li>Unicode 字符</li><li>空格</li><li> **强大的密码**︰ 不能包含一个点字符 '。 前面'@'符号</li></ul> |
|   密码限制 | <ul><li>8 个字符的最小和最大为 16 个字符</li><li>**强大的密码**︰ 从下面的 4 要求 3:<ul><li>小写字符</li><li>大写字符</li><li>数字 (0-9)</li><li>（请参见上面的密码限制） 的符号</li></ul></li></ul> |
| 密码到期期限      | <ul><li>默认值︰ **90**天 </li><li>值是可使用一组 MsolPasswordPolicy cmdlet 从 Azure 活动目录模块用于 Windows PowerShell 配置。</li></ul> |
| 密码过期通知 |  <ul><li>默认值︰ **14**天 （前密码过期）</li><li>值是可配置使用一组 MsolPasswordPolicy cmdlet。</li></ul> |
| 密码过期 |  <ul><li>默认值︰ **false**天 （指示是否启用该口令到期） </li><li>可以使用一组 MsolUser cmdlet 的单个用户帐户的配置值。 </li></ul> |
|  密码历史记录  | 最后一次密码不能再次使用。 |
|  密码历史记录持续时间 | 永远 |
|  帐户锁定 | 在 10 个不成功的登录尝试 （错误密码） 之后, 用户将被锁定为一分钟。 进一步不正确的登录尝试将锁定用户的增加持续时间。 |


## <a name="next-steps"></a>下一步行动

* **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。
* [从任何位置管理密码](active-directory-passwords.md)
* [密码管理的工作原理](active-directory-passwords-how-it-works.md)
* [密码管理入门](active-directory-passwords-getting-started.md)
* [自定义密码管理](active-directory-passwords-customize.md)
* [密码管理最佳做法](active-directory-passwords-best-practices.md)
* [如何获取运营洞察力与密码管理报告](active-directory-passwords-get-insights.md)
* [密码管理常见问题解答](active-directory-passwords-faq.md)
* [解决密码管理](active-directory-passwords-troubleshoot.md)
* [了解更多信息](active-directory-passwords-learn-more.md)
