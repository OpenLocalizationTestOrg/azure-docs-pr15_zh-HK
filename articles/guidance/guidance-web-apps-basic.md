<properties
   pageTitle="基本的 web 应用程序 |Azure 的参考体系结构 |Microsoft Azure"
   description="建议在 Microsoft Azure 中运行一个基本的 web 应用程序的体系结构。"
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="mwasson"/>

# <a name="azure-reference-architecture-basic-web-application"></a>Azure 的参考体系结构︰ 基本的 web 应用程序

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍 Microsoft Azure 中的建议对于基本的 web 应用程序的体系结构。 该体系结构实现使用[Azure 应用程序服务]web 前端[app-service]，与[SQL Azure 数据库][sql-db]数据库上。 本系列的后续文章建立在这种基本结构，添加组件，如缓存和 CDN。 

> [AZURE.NOTE] 这篇文章不专注于应用程序的开发，并不会承担任何特定的应用程序框架。 相反，目标是要了解如何各种 Azure 服务协同在此应用程序体系结构中。

## <a name="architecture-diagram"></a>体系结构关系图

![[0]][0]

该体系结构具有以下组件︰

- **资源组**。 [资源组][resource-group]是 Azure 资源的逻辑容器。 

- **应用程序服务的应用程序**。 [Azure 应用程序服务][app-service]是一个完全托管的平台，用于创建和部署云应用程序。     

- **应用程序服务计划**。 [应用程序服务计划][app-service-plans]提供托管的虚拟机 (Vm) 承载您的应用程序。 与计划相关联的所有应用程序运行在同一个 VM 实例。 

- **部署的插槽。**  一个[部署插槽][deployment-slots]允许您部署阶段，并进行生产部署，则换它。 这样一来，可以避免直接到生产环境中部署。 请参阅具体建议[可管理性](#manageability-considerations)部分。   

- **IP 地址。** 应用程序服务的应用程序具有一个公共 IP 地址和域名。 域名是一个子域的`azurewebsites.net`，如`contoso.azurewebsites.net`。 若要使用自定义的域名，如`contoso.com`，创建的自定义域名称映射到 IP 地址的 DNS 记录。 有关详细信息，请参阅[配置 Azure 应用程序服务中的自定义域名][custom-domain-name]。

- **SQL azure 数据库**。 [SQL 数据库][sql-db]是关系数据库为-服务在云中。 

- **逻辑服务器。** 在 Azure SQL 数据库中，逻辑服务器承载您的数据库。 您可以创建多个数据库，每个逻辑服务器。 

- **Azure 存储。** 创建带有 blob 容器，以存储诊断日志 Azure 存储帐户。 

- **Azure 的活动目录**(Azure AD)。 使用 Azure 广告或另一种身份标识提供程序进行身份验证。

## <a name="recommendations"></a>建议

### <a name="app-service-plan"></a>应用程序服务计划

使用标准或特优层中，因为它们支持自动缩放和 SSL，出诸多功能的比例。 每一层支持多个*实例的大小*，这相差的内核和内存数。 创建一个计划后，您可以更改层或实例的大小。 有关应用程序服务计划的详细信息，请参阅[应用程序服务定价][app-service-plans-tiers]。

您负责的实例在应用程序服务计划中，即使应用程序已停止。 请确保删除您不使用 （例如，测试部署） 的计划。

### <a name="sql-database"></a>SQL 数据库

使用[V12 版本][sql-db-v12]的 SQL 数据库。 SQL 数据库支持基本、 标准和特优[服务层][sql-db-service-tiers]，与每一层中的多个性能级别，以[数据库事务单位 (DTUs)][sql-dtu]。 执行容量规划和选择层和性能级别，以满足您的需求。

### <a name="region"></a>地区

提供应用程序服务计划和 SQL 数据库在同一个地区，若要最小化网络延迟。 通常情况下，选择最接近您的用户的区域。 

资源组也有一个区域，指定部署元数据的存储位置。 将资源组和资源放在同一个地区。 如果在某些 Azure 数据中心中的问题，这可以在部署期间，提高可用性。  


## <a name="scalability-considerations"></a>可伸缩性注意事项

### <a name="scaling-the-app-service-app"></a>扩展的应用程序服务的应用程序

有两种方法来扩展应用程序服务的应用程序︰

- *扩大规模*，这意味着更改实例的大小。 实例的大小确定内存的内核，并在每个虚拟机实例的存储数量。 您可以放大手动更改实例的大小或计划层。  

- *向外的扩展*，这意味着添加实例以处理增加的负载。 每个定价层具有的最大实例数。 可以通过更改的实例计数，手动扩展或使用[自动缩放][web-app-autoscale]具有 Azure 自动添加或删除实例，根据计划和/或性能指标。

    - 自动缩放比例配置文件定义最小值和最大实例数。 配置文件可以安排。 例如，您可以为工作日和周末创建单独的配置文件。 

    - （可选） 配置文件时添加或删除实例，在配置文件的最小值和大范围定义具有规则。 例如︰ 如果 CPU 使用率超过 70%的 5 分钟添加两个实例。 每个缩放操作快速反应&mdash;通常在秒钟之内。
    
    - 自动缩放比例规则包括*冷却*阶段，即后缩放操作之前执行其他缩放操作要等待的时间间隔。 冷却期使系统达到稳定状态前再次缩放。
 
这里是我们扩展 web 应用程序的一些建议︰

- 尽可能多地，避免缩放向上和向下，因为它可能会触发应用程序重启。 选择层和大小，满足您的性能要求，在典型负载，然后扩展的实例来处理流量的变化。    

- 启用自动缩放。 如果您的应用程序具有可预测性、 正则的工作负荷，创建配置文件以安排提前的实例计数。 如果工作负荷是不可预测的，使用基于规则的自动缩放发生时对负载变化做出反应。 您可以结合使用这两种方法。 

- 通常，CPU 是自动缩放比例规则的一个良好指标。 但是，您应该加载测试您的应用程序，识别潜在的瓶颈，并基于该数据自动缩放比例规则。  

- 设置用于添加实例和删除实例的再冷却期较短的冷却周期。 例如，设置 5 分钟添加一个实例，但 60 分钟删除实例。 突然在负载下，最好是若要快速添加新的实例，以处理通信，然后逐渐扩展回。
    
### <a name="scaling-sql-database"></a>扩展的 SQL 数据库

如果您需要更高的服务层或性能级别为 SQL 数据库，您可以缩放建立单独的数据库，避免应用程序宕机。 有关详细信息，请参阅[更改 SQL 数据库服务层和性能级别][sql-db-scale]。 

## <a name="availability-considerations"></a>可用性考虑事项

在书写时，应用程序服务的 SLA 为 99.95%，和 SQL 数据库的 SLA 是基本、 标准和高级层的 99.99%。 

> [AZURE.NOTE] 应用程序服务 SLA 适用于单个和多个实例。  

### <a name="backups"></a>备份

SQL 数据库提供的时间点还原和地理还原。 这些功能在所有层中可用，并将自动启用。 您不需要安排或管理备份。 

- 使用时间点还原到[恢复的人为错误][sql-human-error]。 及时向早些时候返回您的数据库。

- 使用地理-还原到[恢复服务中断的][sql-outage-recovery]。 它从地理冗余备份还原数据库。

有关这些选项的详细信息，请参阅[云业务连续性和 SQL 数据库的数据库灾难恢复][sql-backup]。 

应用程序服务提供[备份和还原][web-app-backup]功能。 但是，请注意备份文件以纯文本形式包含应用程序设置。 它们中可能包含机密信息，如连接字符串。 避免使用应用程序服务备份功能来备份您的 SQL 数据库，因为它将数据库导出到 SQL.bacpac 文件，它使用[DTUs][sql-dtu]。 相反，使用 SQL 数据库的时间点还原，上文所述。 

## <a name="manageability-considerations"></a>可管理性注意事项

创建单独的资源组用于生产、 开发和测试环境。 这使得更容易地管理部署、 删除测试部署和分配访问权限。

将资源分配给资源组，请考虑下列情况︰

- 生命周期。 一般情况下，将具有相同的生命周期的资源放入同一个资源组。 

- 访问。 您可以使用[基于角色的访问控制][ rbac] (RBAC)，以应用于组中的资源的访问策略。

- 帐单。 您可以查看汇总的成本资源组。  

有关详细信息，请参阅[Azure 资源管理器概述][resource-group]。

### <a name="deployment"></a>部署

部署包括两个步骤︰

- 资源调配的 Azure 的资源。 我们建议使用[Azure 资源管理器模板][arm-template]的这一步。 模板方便地自动通过 PowerShell 或 Azure CLI 的部署。 

- 部署应用程序 （代码、 二进制文件和内容文件）。 有几个选项，包括部署从本地 Git 存储库，使用 Visual Studio 或连续从基于云的源代码管理部署。 [部署您的应用程序到 Azure 应用程序服务]，请参阅[deploy]。  
 
应用程序服务的应用程序始终有一个名为的部署插槽`production`，表示实际生产站点。 我们建议您创建一个临时部署更新的插槽。 使用临时插槽的好处包括︰

- 您可以验证以前换到生产部署成功。

- 部署到临时插槽可确保所有实例都取暖被交换到生产环境之前。 许多应用程序有大量的预热和冷启动时间。 

我们还建议您创建第三个插槽用于保存上次已知的正确部署。 换临时和生产后，进入最后一个已知良好的插槽 （即现在暂存） 以前生产部署。 这样一来，如果发现问题，以后，您可以快速恢复到上次已知的正确版本。 

![[1]][1]

如果还原到以前的版本，请确保数据库架构的任何更改都向后兼容。

不要使用插槽上生产部署测试，因为相同的应用程序服务计划中的所有应用程序都共享同一个 VM 实例。 例如，负载测试可能会降低实际生产站点。 相反，创建单独的应用程序服务计划，以生产和测试。 通过将测试部署到不同的计划，您的生产版本将它们隔离。 

### <a name="configuration"></a>配置

将配置设置存储[的应用程序设置]为[app-settings]。 在资源管理器模板，或使用 PowerShell 定义应用程序设置。 在运行时，应用程序设置应用程序可以使用环境变量一样。 

永远不签入源代码管理密码，访问键或连接字符串。 相反，将它们作为参数传递给一个部署脚本，将这些值存储为应用程序设置。 

当交换部署插槽时，请应用程序设置默认情况下交换。 如果生产和转移需要不同的设置，您可以创建应用程序设置，"粘"在一个插槽并不会交换。 

### <a name="diagnostics-and-monitoring"></a>诊断和监视

启用[诊断日志记录][diagnostic-logs]，包括日志记录应用程序和 web 服务器日志记录。 配置日志使用 blob 存储。 出于性能原因，为创建一个单独的存储帐户诊断日志。 不要使用相同的存储帐户日志和应用程序数据。 有关更多详细日志记录的指导，请参阅[监视和诊断指导][monitoring-guidance]。 

使用服务，如[新的 Relic] [new-relic]或[应用程序的见解][app-insights]监控应用程序性能和负载下的行为。 请注意[数据速率限制]的[app-insights-data-rate]的应用程序的见解。

执行负载测试时，使用诸如[Visual Studio 团队服务]之类的工具[vsts]。 在云应用程序中的性能分析的一般概述，请参阅[性能分析入门][perf-analysis]。

有关您的应用程序的故障排除提示︰

- 使用[疑难解答刀片式服务器][troubleshoot-blade]在 Azure 门户为标识的常见问题的解决方案。

- 启用[日志流][web-app-log-stream]以接近实时的日志记录信息，请参阅。 

- [仪表板 Kudu] [kudu]有几个工具用于监视和调试您的应用程序。 有关详细信息，请参阅[Azure 网站在线工具，您应了解有关][ kudu] （博客文章）。 您可以从 Azure 门户到达 Kudu 仪表板。 打开刀片式服务器为您的应用程序并单击**工具**，然后单击**Kudu**。

- 如果您使用 Visual Studio，请参阅[疑难解答中 Azure 应用程序服务 web 应用程序使用的 Visual Studio]文章[troubleshoot-web-app]用于调试和故障排除技巧。


## <a name="security-considerations"></a>安全注意事项

> [AZURE.NOTE] 本部分指出了一些特定于本文中所描述的 Azure 服务的安全注意事项。 该列表并不完整的安全最佳做法。 一些额外的安全注意事项，请参见[安全在 Azure 应用程序服务的应用程序][app-service-security]。

**SQL 数据库的审核**。 审核可以帮助您保持法规遵从性，并深入矛盾和异常情况可能表示的业务问题或怀疑有违反安全的行为。 [开始使用 SQL 数据库的审核]，请参阅[sql-audit]。

**部署的插槽**。 部署的每个插槽都有一个公用 IP 地址。 安全使用[Azure Active Directory 登录名]的非生产插槽[aad-auth]，以便只有您开发和 DevOps 团队的成员可以访问这些终结点。 

**日志记录**。 日志应该永远不会记录用户的密码或其他信息，可以用来进行身份欺诈。 清理前将其从数据的详细信息。   

**SSL**。 应用程序服务的应用程序包括的子域上的 SSL 端点`azurewebsites.net`在不增加成本，使用通配符证书的`*.azurewebsites.net`域。     如果您使用自定义的域的名称，您必须提供自定义的域匹配的证书。 最简单的方法是购买直接通过 Azure 门户的证书。 您还可以从其他证书颁发机构导入证书。 有关详细信息，请参阅[购买和配置 SSL 证书 Azure 应用程序服务的][ssl-cert]。 

作为安全性的最佳操作，您的应用程序应强制 HTTPS 通过重定向的 HTTP 请求。 您可以在您的应用程序内实现这或述[Azure 应用程序服务中的应用程序启用 HTTPS]使用 URL 重写规则[ssl-redirect]。

### <a name="authentication"></a>身份验证

我们建议通过进行身份验证的身份提供程序 (IDP) Azure 的广告，如 Facebook，Google，或者使用 Twitter，使用 OAuth 2 或 OpenID 连接 (OIDC) 身份验证的流。 

Azure 的广告使您能够管理用户和组、 创建应用程序角色、 集成您的内部标识和使用 Office 365 和 Skype 等业务的后端服务。

避免应用程序管理用户登录帐户和凭据直接造成较大的潜在攻击面。 至少，您需要有电子邮件确认、 密码恢复和多因素身份验证;验证密码强度;并安全存储密码哈希值。 大标识提供程序处理所有这些事情，并会不断监控和提高安全措施。 

请考虑使用[应用程序服务的身份验证][app-service-auth]来实现 OAuth/OIDC 身份验证流程。 应用程序服务的身份验证的优点包括︰

- 易于配置。
    
- 对于简单的身份验证方案，没有代码是必需的。
    
- 支持委派授权;也就是说，使用 OAuth 的访问令牌代表用户消耗的资源。
    
- 提供内置的令牌缓存。

应用程序服务的身份验证的一些限制︰  

- 有限的自定义选项。 

- 委派的授权仅限于每个登录会话的一个后端资源。
    
- 如果您使用多个 IDP，没有家庭领域发现的内置机制。

- 多租户的情况下，应用程序必须实现的逻辑以验证令牌颁发者。



## <a name="deploying-the-sample-solution"></a>部署示例解决方案

此体系结构资源管理器模板示例出现在 GitHub 上。 下载[以下][paas-basic-arm-template]。

若要部署使用 PowerShell 的模板，请运行以下命令︰

```
New-AzureRmResourceGroup -Name <resource-group-name> -Location "West US"

$parameters = @{"appName"="<app-name>";"environment"="dev";"locationShort"="uw";"databaseName"="app-db";"administratorLogin"="<admin>";"administratorLoginPassword"="<password>"}
    
New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile .\PaaS-Basic.json -TemplateParameterObject  $parameters
```

有关详细信息，请参阅[使用 Azure 资源管理器模板部署资源][deploy-arm-template]。


## <a name="next-steps"></a>下一步行动

- 可以通过添加功能，如缓存，CDN，改善可伸缩性和性能和后台处理长时间运行的任务。 请参阅[Web 应用程序提供改进的可扩展性](guidance-web-apps-scalability.md)。

<!-- links -->

[aad-auth]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-data-rate]: ../application-insights/app-insights-pricing.md
[app-service]: https://azure.microsoft.com/en-us/documentation/services/app-service/
[app-service-auth]: ../app-service-api/app-service-api-authentication.md
[app-service-plans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[app-service-plans-tiers]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[app-service-security]: ../app-service-web/web-sites-security.md
[app-settings]: ../app-service-web/web-sites-configure.md
[arm-template]: ../resource-group-overview.md
[custom-domain-name]: ../app-service-web/web-sites-custom-domain-name.md
[deploy]: ../app-service-web/web-sites-deploy.md
[deploy-arm-template]: ../resource-group-template-deploy.md
[deployment-slots]: ../app-service-web/web-sites-staged-publishing.md
[diagnostic-logs]: ../app-service-web/web-sites-enable-diagnostic-log.md
[kudu]: https://azure.microsoft.com/en-us/blog/windows-azure-websites-online-tools-you-should-know-about/
[monitoring-guidance]: ../best-practices-monitoring.md
[new-relic]: http://newrelic.com/
[paas-basic-arm-template]: https://github.com/mspnp/blueprints/tree/master/paas-basic/Paas-Basic/Templates
[perf-analysis]: https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md
[rbac]: ../active-directory/role-based-access-control-what-is.md
[resource-group]: ../resource-group-overview.md
[sla]: https://azure.microsoft.com/en-us/support/legal/sla/
[sql-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-backup]: ../sql-database/sql-database-business-continuity.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-db-scale]: ../sql-database/sql-database-scale-up-powershell.md
[sql-db-service-tiers]: ../sql-database/sql-database-service-tiers.md
[sql-db-v12]: ../sql-database/sql-database-v12-whats-new.md
[sql-dtu]: ../sql-database/sql-database-service-tiers.md#understanding-dtus
[sql-human-error]: ../sql-database/sql-database-business-continuity.md#recover-a-database-after-a-user-or-application-error
[sql-outage-recovery]: ../sql-database/sql-database-business-continuity.md#recover-a-database-to-another-region-from-an-azure-regional-data-center-outage
[ssl-redirect]: ../app-service-web/web-sites-configure-ssl-certificate.md#4-enforce-https-on-your-app
[sql-resource-limits]: ../sql-database/sql-database-resource-limits.md
[ssl-cert]: ../app-service-web/web-sites-purchase-ssl-web-site.md
[troubleshoot-blade]: https://azure.microsoft.com/en-us/updates/self-service-troubleshooting-for-app-service-web-apps-customers/
[troubleshoot-web-app]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[vsts]: https://www.visualstudio.com/en-us/features/vso-cloud-load-testing-vs.aspx
[web-app-autoscale]: ../app-service-web/web-sites-scale.md#scaling-to-standard-or-premium-mode
[web-app-backup]: ../app-service-web/web-sites-backup.md
[web-app-log-stream]: ../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs
[0]: ./media/blueprints/paas-basic-web-app.png "Azure 的基本 web 应用程序的体系结构"
[1]: ./media/blueprints/paas-basic-web-app-staging-slots.png "在换用插槽用于生产和转移部署"