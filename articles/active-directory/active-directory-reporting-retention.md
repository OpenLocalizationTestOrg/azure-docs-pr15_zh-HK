<properties
    pageTitle="Azure Active Directory 报告保留策略 |Microsoft Azure"
    description="有关将 Azure Active Directory 中的报告数据的保留策略"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-retention-policies"></a>Azure Active Directory 报告保留策略

*本文档是[Azure 活动目录报告指南](active-directory-reporting-guide.md)的一部分。*

Azure 的活动目录 (AD Azure) 报表将数据保留一定数量的天数，在此处指出。

报告                                                  | 说明
------------------------------------------------------- | -----------
来自未知源登录                           | 30 天
在多次失败后登录                        | 30 天
在多个地域的号接                      | 30 天
从可疑活动的 IP 地址登录     | 30 天
从可能是感染病毒的设备登录                 | 30 天
不规则的签到活动                              | 30 天
用户登录活动异常                   | 30 天
具有泄漏的凭据的用户                           | 30 天
审计报告                                            | 180 天
密码重置活动 (Azure AD)                      | 30 天
密码重置活动 （标识管理器）              | 30 天
密码重置注册的活动 (Azure AD)         | 30 天
密码重置注册的活动 （标识管理器） | 30 天
自助服务组活动 (Azure AD)                 | 30 天
自助服务组活动 （标识管理器）         | 30 天
应用程序使用情况                                       | 30 天
资源调配活动的帐户                           | 30 天
密码翻转状态                                | 30 天
考虑资源调配错误                             | 30 天
RMS 使用情况                                               | 30 天
最活跃的 RMS 用户                                   | 30 天
RMS 设备用法                                        | 30 天
启用 RMS 的应用程序使用情况                           | 30 天
