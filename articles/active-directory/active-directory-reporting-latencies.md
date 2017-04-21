<properties
   pageTitle="报告延迟的 azure Active Directory |Microsoft Azure"
   description="所需报告事件在 Azure 活动目录中显示的时间量"
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

# <a name="azure-active-directory-report-latencies"></a>Azure 的 Active Directory 报告延迟

*本文档是[Azure 活动目录报告指南](active-directory-reporting-guide.md)的一部分。*

报告                                                  | 最小值  | 平均值    | 最大值
------------------------------------------------------- | -------- | ---------- | ----------
**安全报告**                                    |          |            |
不规则的签到活动                              | 2 个小时  | 4 个小时    | 8 小时
来自未知源登录                           | 2 个小时  | 4 个小时    | 8 小时
在多次失败后登录                        | 2 个小时  | 4 个小时    | 8 小时
在多个地域的号接                      | 2 个小时  | 4 个小时    | 8 小时
从可疑活动的 IP 地址登录     | 2 个小时  | 4 个小时    | 8 小时
从可能是感染病毒的设备登录                 | 2 个小时  | 4 个小时    | 8 小时
用户登录活动异常                   | 2 个小时  | 4 个小时    | 8 小时
具有泄漏的凭据的用户                           | 2 个小时  | 4 个小时    | 8 小时
**应用程序报告**                                 |          |            |
资源调配活动的帐户                           | 2 个小时  | 4 个小时    | 8 小时
考虑资源调配错误                             | 2 个小时  | 4 个小时    | 8 小时
应用程序使用情况                                       | 2 个小时  | 4 个小时    | 8 小时
密码翻转状态                                | 2 个小时  | 4 个小时    | 8 小时
**审核和活动报告**                            |          |            |
审计报告                                            | 1 分钟 | 15 分钟 | 30 分钟
密码重置活动 (Azure AD)                      | 2 个小时  | 4 个小时    | 8 小时
密码重置活动 （标识管理器）              | 2 个小时  | 4 个小时    | 8 小时
密码重置注册的活动 (Azure AD)         | 2 个小时  | 4 个小时    | 8 小时
密码重置注册的活动 （标识管理器） | 2 个小时  | 4 个小时    | 8 小时
自助服务组活动 (Azure AD)                 | 2 个小时  | 4 个小时    | 8 小时
自助服务组活动 （标识管理器）         | 2 个小时  | 4 个小时    | 8 小时
**RMS 的报告**                                         |          |            |
最活跃的 RMS 用户                                   | 2 个小时  | 4 个小时    | 8 小时
RMS 使用情况                                               | 2 个小时  | 4 个小时    | 8 小时
RMS 设备用法                                        | 2 个小时  | 4 个小时    | 8 小时
启用 RMS 的应用程序使用情况                           | 2 个小时  | 4 个小时    | 8 小时
**专用预览报表**                             |          |            |
用户登录的所有活动                               | 2 个小时  | 4 个小时    | 8 小时
