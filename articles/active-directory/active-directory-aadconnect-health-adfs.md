
<properties
    pageTitle="使用 Azure 广告健康与 AD FS |Microsoft Azure"
    description="这是 Azure AD 连接状况页面如何监视您在内部 AD FS 基础结构。"
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>与 AD FS 使用 Azure AD 连接运行状况
以下文档是特定的 AD FS 基础结构使用 Azure AD 连接运行状况监视。 监视使用 Azure AD 连接健康的 Azure AD 连接 （同步） 的信息，请参阅[使用 Azure AD 同步连接状况](active-directory-aadconnect-health-sync.md)。 此外，有关监视 Azure AD 连接运行状况与活动目录域服务的信息，请参阅[使用 Azure AD 连接健康 AD DS](active-directory-aadconnect-health-adds.md)。

## <a name="alerts-for-ad-fs"></a>AD fs 的警报
Azure AD 连接的运行状况警报部分提供活动通知的列表。 每个通知包含相关的信息、 解决方法步骤和相关文档的链接。

您可以双击活动或已解决警报，可使用的其他信息，您可以采取的步骤来解决警报，并链接到相关文档打开新刀片。 您还可以查看历史数据，在过去已解决的警报。

![Azure AD 连接健康门户](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>AD fs 使用情况分析
Azure 的 AD 连接健康使用率分析分析联合身份验证服务器的身份验证通迅流。 您可以双击用法分析中，若要打开显示几个衡量标准和分组使用状况分析刀片式服务器。

>[AZURE.NOTE] 若要使用 AD FS 使用情况分析，必须确保启用 AD FS 的审核。 有关详细信息，请参阅[启用 AD FS 的审核](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs)。

![Azure AD 连接健康门户](./media/active-directory-aadconnect-health/report1.png)

若要选择其他度量，请指定一个时间范围，或更改分组，使用率分析图表上右键单击，并选择编辑图表。 然后可以指定的时间范围，选择不同的指标，并更改分组。 可以查看基于不同的"标准"身份验证流量的分布和分组使用下表中描述的相关"分组依据"参数每个跃点计数︰

| 跃点计数 | 分组依据 | 意味着什么分组和为什么它很有用？ |
| ------ | -------- | -------------------------------------------- |
| 总请求︰ 总请求数的联合身份验证服务处理 | 所有 | 说明如果不进行分组的请求总数的计数。 |
|  | 应用程序 | 分组基于目标依赖当事方的请求总数。 这种分组可用于了解哪些应用程序正在接收多少百分比的总通信量。 |
|  | 服务器 | 分组根据处理请求的服务器的请求总数。 这种分组可用于了解总的流量的负载分布。 |
|  | 工作区的连接 | 根据其是否来自于加入的工作区的设备总请求 （已知） 进行分组。 这种分组可用于了解是否使用未知的身份基础结构的设备访问您的资源。 |
|  | 身份验证方法 | 组用于身份验证的身份验证方法所根据的请求总数。 这种分组可用于了解公共获取用于身份验证的身份验证方法。 以下是可能的身份验证方法 <ol> <li>Windows 集成身份验证 (Windows)</li> <li>基于表单的身份验证 （窗体）</li> <li>SSO （单一登录）</li> <li>X509 证书身份验证 （证书）</li> <br>如果联合身份验证服务器收到请求 SSO 的 cookie，则该请求被视为 SSO （单一登录）。 在这种情况下，如果该 cookie 是有效的用户不要求提供凭据并获取对应用程序的无缝访问。 这种现象很常见的如果您有多个联合服务器所受的信赖方。 |
|  | 网络位置 | 总请求基于网络位置的用户进行分组。 它可以是任何一个企业内部网或外部网。 这种分组很有用知道百分之多少的流量来自企业内部网与外部网。 |
| 失败的请求总数︰ 失败联合身份验证服务处理的请求总数。 <br> （该指标只具有 Windows Server 2012 R2 的 AD FS）| 错误类型 | 显示基于预定义的错误类型的错误数。 这种分组可用于理解错误的常见类型。 <ul><li>不正确的用户名或密码︰ 由于错误的用户名或密码错误。</li> <li>"外部网锁定": 从外部网已被锁定的用户接收到的请求引起的故障 </li><li> "过期密码": 由于故障而导致用户登录已过期的密码。</li><li>"禁用帐户": 由于用户使用已禁用的帐户登录失败。</li><li>"身份验证设备": 由于故障而导致无法使用设备的身份验证进行身份验证的用户。</li><li>"用户证书身份验证": 用户无法验证证书无效原因引起的故障。</li><li>"MFA": 由于用户不使用多因素身份验证进行身份验证失败。</li><li>"其他凭据":"颁发授权": 授权失败引起的故障。</li><li>"颁发委派": 颁发委派错误引起的故障。</li><li>"标记验收": 由于 ADFS 拒绝第三方身份标识提供程序中的令牌失败。</li><li>"协议": 由于协议错误而失败。</li><li>"未知": 全部捕获。 任何其他失败不符合定义的类别。</li> |
|  | 服务器 | 基于服务器的错误的组。 这种分组可用于了解跨服务器错误分发。 分配不均，可能是服务器处于故障状态的指示器。 |
|  | 网络位置 | 组的请求 （内部网与外部网） 的网络位置的错误。 这种分组可用于了解失败的请求的类型。 |
|  | 应用程序 | 分组的目标应用程序 （信赖方） 的故障。 这种分组可用于了解哪个目标应用程序所看到的错误最多。 |
| 用户计数︰ 平均的系统中的活动的唯一用户数 | 所有 | 此统计数据提供了用户在选定的时间段中使用联合身份验证服务的平均数目。 未分组的用户。 <br>平均值取决于所选的时间片。 |
|  | 应用程序 | 分组的平均 （信赖方） 的目标应用程序的用户数。 这种分组可用于了解多少用户正在使用的应用程序。 |


## <a name="performance-monitoring-for-ad-fs"></a>AD fs 监视性能
Azure AD 连接健康性能监控提供了监视信息的度量标准。 选择监视框中，打开新的刀片具有标准的详细信息。


![Azure AD 连接健康门户](./media/active-directory-aadconnect-health/perf1.png)


通过选择刀片式服务器顶部的筛选器选项，您可以通过筛选服务器以查看单个服务器的规格。 更改度量值，请在下监视刀片式服务器的监视图表上右击并选择编辑图表。 然后，从开辟新刀片，可以从下拉列表中选择其他度量并指定查看性能数据的时间范围。

## <a name="reports-for-ad-fs"></a>AD fs 报告
Azure AD 连接运行状况提供了关于活动和 AD FS 的性能报告。 这些报告可帮助管理员能深入了解其 AD FS 服务器上的活动。

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>前 50 名用户的用户名/密码登录失败

AD FS 服务器上请求身份验证失败的常见原因之一是使用无效的凭据，就是使用错误的用户名或密码的请求。 通常会发生由于复杂的密码、 忘记的密码，或者键入错误的用户。

但也有其他原因，可能会导致意外的 AD FS 服务器，如处理的请求数︰ 应用程序的缓存用户凭据和凭据过期或恶意用户尝试登录到具有一系列众所周知的密码的帐户。 这两个示例都可能会导致请求的峰值的正当理由。

Azure AD 连接的运行状况 ADFS 提供有关失败的登录尝试无效的用户名或密码的前 50 个用户的报告。 此报告被通过处理在服务器场中的所有 AD FS 服务器都生成的审核事件

![Azure AD 连接健康门户](./media/active-directory-aadconnect-health-adfs/report1a.png)

在此报表中的您可以方便地访问信息的以下部分︰

- 失败的请求使用在过去 30 天中错误的用户名/密码的总人数
- 平均每天一个错误的用户名/密码登录失败的用户数。


单击此部分可将您带到主报表刀片式服务器，提供更多详细信息。 此刀片包括与趋势的信息，以帮助建立有关请求使用错误的用户名或密码的基准图。 此外，它提供了前 50 用户列表失败尝试的次数。

该图提供了以下信息︰

- 登录失败，由于错误的用户名/密码在每日的基础上的总人数。
- 失败的登录在每日的基础上的唯一用户的总人数。
- 客户端 IP 地址的最后一个请求

![Azure AD 连接健康门户](./media/active-directory-aadconnect-health-adfs/report3a.png)

该报告提供以下信息︰

| 报表项 | 说明
| ------ | -------- |
|用户 ID| 显示所使用的用户 ID。 此值是用户键入的内容，这在某些情况下是错误的用户 ID 的使用。|
|失败的尝试| 显示失败的尝试的总人数为该特定的用户 id。 表的排序方式最失败尝试次数降序排序。|
|上次失败| 上一次故障发生时显示的时间戳。
|上次失败 IP | 显示最新的错误请求的客户端 IP 地址。|



>[AZURE.NOTE] 在这段时间内收集新信息每隔两个小时后，此报表将自动更新。 因此，过去两个小时内的登录尝试可能不包括在报表中。



## <a name="related-links"></a>相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接健康代理安装](active-directory-aadconnect-health-agent-install.md)
* [Azure AD 连接健康操作](active-directory-aadconnect-health-operations.md)
* [对于同步使用 Azure AD 连接运行状况](active-directory-aadconnect-health-sync.md)
* [与 AD DS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adds.md)
* [Azure AD 连接健康常见问题解答](active-directory-aadconnect-health-faq.md)
* [Azure AD 连接健康版本历史记录](active-directory-aadconnect-health-version-history.md)
