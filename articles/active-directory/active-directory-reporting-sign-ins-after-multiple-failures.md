<properties
    pageTitle="在多次失败后签名宏"
    description="指示用户已成功登录之后多个连续失败登录尝试中报告。"
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>在多次失败后登录
此报表指明用户已成功登录之后多个连续失败登录尝试。 可能的原因包括︰

- 用户已经忘记了他们的密码</li><li>用户是密码的成功猜测方面暴力的受害者强制攻击

此报表中的结果将显示的数连续失败的登录尝试成功登录前创建和与第一个成功登录相关联的时间戳。

**报告设置**︰ 您可以配置的最低数量的连续失败的登录尝试，它可以显示在报表中之前，必须执行。 对此设置进行更改时，必须注意到单元当前在现有报表中显示的任何现有的失败签名不应用这些更改。 但是，它们将应用于所有未来号接。 更改此报表仅可通过授权的管理员。


![在多次失败后签名宏](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)
