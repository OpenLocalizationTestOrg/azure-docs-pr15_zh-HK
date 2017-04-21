<properties
    pageTitle="确保在 Azure 应用程序服务的应用程序的安全"
    description="了解如何保护 web 应用程序、 移动应用程序的后端或 API 在 Azure 应用程序服务的应用程序的安全。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>确保在 Azure 应用程序服务的应用程序的安全

这篇文章可以帮助您保护您的 web 应用程序、 移动应用程序的后端或在 Azure 应用程序服务的 API 应用程序开始。 

在 Azure 应用程序服务的安全性有两个级别︰ 

- **基础设施和平台安全**-信任 Azure 能够实际运行在云环境中安全地操作所需的服务。
- **应用程序安全**-您需要安全地设计应用程序本身。 这包括与 Azure Active Directory 集成方式、 如何管理证书，以及如何可以确保您可以安全地谈到不同的服务。 

#### <a name="infrastructure-and-platform-security"></a>基础设施和平台的安全性
因为应用程序服务维护 Azure Vm，存储、 网络连接、 web 框架、 管理和集成功能和更多，主动保护并强化和强有力的法规遵从性所经历和检查，以确保持续不断地︰

- 您的应用程序服务的应用程序被隔离，从这两个互联网和其他客户的 Azure 资源。
- 通讯，您的应用程序服务的应用程序和资源组中其他 Azure 的资源 （如 SQL 数据库） 之间的机密信息 （如连接字符串） 停留在 Azure 并不跨所有网络边界。 总是加密机密信息。
- 您的应用程序服务的应用程序和外部资源，如 PowerShell 管理、 命令行界面，Azure 的 Sdk，REST Api 和混合连接之间的所有通信进行正确都加密。
- 24 小时威胁管理保护的应用程序服务资源从恶意软件，分布式拒绝服务 (DDoS)--拦截 (MITM) 和其他威胁。 

基础设施和平台 Azure 中的安全的详细信息，请参阅[Azure 信任中心](/support/trust-center/security/)。

#### <a name="application-security"></a>应用程序的安全性

负责保护基础架构和应用程序运行的平台 Azure 时，是以保护应用程序本身的责任。 换句话说，您需要开发、 部署和管理应用程序代码和内容以安全的方式。 没有它，您的应用程序代码或内容仍然容易受到威胁，如︰

- SQL 注入
- 会话劫持
- 跨站点脚本攻击
- 应用程序级 MITM
- 应用程序级 DDoS

基于 web 的应用程序的安全注意事项的完整讨论已超出了本文的讨论范围。 为进一步指导保护您的应用程序的起始点，请参见[打开 Web 应用程序安全项目 (OWASP)](https://www.owasp.org/index.php/Main_Page)，特别是[十大项目。](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)，其中列出了当前的顶 10 个关键的 web 应用程序安全漏洞，由 OWASP 成员

## <a name="perform-penetration-testing-on-your-app"></a>执行渗透测试您的应用程序

一个最简单的方法来测试您的应用程序服务的应用程序上的漏洞与尚未开始是用[锡箔安全与集成](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)来执行一次单击漏洞扫描您的应用程序。 可以查看测试结果，在一个易于理解的报告，并了解如何解决每个漏洞的分步指导。

如果您希望执行渗透测试或者想要使用其他扫描程序套件或提供程序，必须遵循[Azure 渗透测试的审批流程](https://security-forms.azure.com/penetration-testing/terms)并获得事先批准才能执行所需的渗透测试。

##<a name="https"></a>确保与客户的通信安全

如果使用**\*。 azurewebsites.net**域名创建为您的应用程序服务的应用程序，您可以立即使用 HTTPS，SSL 证书提供所有**\*。 azurewebsites.net**域名。 如果您的站点使用一个[自定义的域的名称](web-sites-custom-domain-name.md)，您可以为自定义的域[启用 HTTPS](web-sites-configure-ssl-certificate.md)上载 SSL 证书。

启用[HTTPS](https://en.wikipedia.org/wiki/HTTPS)可以帮助防止对您的应用程序及其用户之间的通信 MITM 攻击。

## <a name="secure-data-tier"></a>安全的数据层

应用程序服务高度集成与 SQL 数据库中，以便所有连接字符串纵观全局加密并且仅被解密应用程序仅在应用程序运行时*以及*运行的 VM 上。 此外，SQL Azure 数据库包含许多安全功能来帮助您保护您的应用程序数据从网络威胁，包括[静态加密](https://msdn.microsoft.com/library/dn948096.aspx)，[始终将其加密](https://msdn.microsoft.com/library/mt163865.aspx)，[动态数据屏蔽](../sql-database/sql-database-dynamic-data-masking-get-started.md)和[威胁检测](../sql-database/sql-database-threat-detection-get-started.md)。 如果您的敏感数据或法规遵从性要求，请参阅[SQL 数据库设置安全性](../sql-database/sql-database-security.md)如何保护您的数据的详细信息。

如果您使用第三方数据库提供程序，如 ClearDB，您应该咨询直接的安全最佳做法的提供程序的文档。  

##<a name="develop"></a>安全开发和部署

### <a name="publishing-profiles-and-publish-settings"></a>发布配置文件和发布设置

在开发应用程序、 执行管理任务，或者使用**Visual Studio**、 **Web 矩阵**， **Azure PowerShell**和**Azure 命令行界面 (Azure CLI)**，实用程序自动完成任务时可以使用*发布设置*文件或*发布配置文件*。 这两种文件类型与 Azure，验证您的身份，应该得到保护，以防止未经授权的访问。

* **发布设置**文件包含

    * Azure 的订阅 ID

    * 管理证书，您可以执行管理任务，为您的订阅*而无需提供用户名或密码*。

* **发布配置**文件包含

    * 发布到您的应用程序的信息

如果您使用一个实用工具，使用发布设置文件或发布配置文件，导入文件包含的发布设置或配置文件的实用程序，然后**删除**该文件。 如果您必须保留该文件，与为例，在该项目上工作的其他人共享存储在安全的位置，如*加密*目录具有受限制权限。

此外，应确保导入的凭据的安全。 例如， **Azure PowerShell**和**Azure 命令行界面 (Azure CLI)**都将导入的信息存储在**主目录**中 (*~*在 Linux 或 OS X 系统，在 Windows 系统上的*/users/yourusername* 。)为额外的安全性，您可能希望进行**加密**您的操作系统使用可用的加密工具这些位置。

### <a name="configuration-settings-and-connection-strings"></a>配置设置和连接字符串
它是常见的做法在配置文件中存储连接字符串、 身份验证凭据和其他敏感信息。 遗憾的是，可能会在您的网站上公开这些文件或将其登记到一个公用的存储库，将公开此信息。 例如，在[GitHub](https://github.com)，简单搜索可以发现无数的配置文件与公用存储库中公开的秘密。

最好的办法是从您的应用程序配置文件中保留此信息。 应用程序服务允许您为**应用程序设置**和**连接字符串**配置信息存储运行时环境的一部分。 向您的应用程序在运行时*环境变量*对于大多数编程语言通过公开值。 对于.NET 应用程序中，这些值注入到您的.NET 配置在运行时。 除了这些情况下，这些配置设置将保持加密状态除非查看或配置使用[Azure 门户](https://portal.azure.com)或 PowerShell 和 Azure CLI 的实用程序。 

将配置信息存储在应用程序服务，使应用程序的管理员可以锁定用于生产应用程序的敏感信息。 开发人员可以使用一组单独的配置设置为应用程序开发和设置，则可以自动取代由应用程序服务中配置的设置。 开发人员甚至不需要知道为生产应用程序配置的机密。 配置应用程序服务中的应用程序设置和连接字符串的详细信息，请参阅[配置 web 应用程序](web-sites-configure.md)。

### <a name="ftps"></a>提取

Azure 应用程序服务提供了安全的文件系统为您的应用程序通过**FTPS**的 FTP 访问。 这使您可以安全地访问 web 应用程序以及诊断上的应用程序代码日志。 建议您始终使用 FTPS 而不是 FTP。 

FTPS 链接为您的应用程序可以找到下列步骤︰

1. 打开[Azure 的门户](https://portal.azure.com)。
2. 选择**浏览所有**。
3. 从**浏览**刀片式服务器，请选择**应用程序服务**。
4. 从**应用程序服务**刀片式服务器，请选择所需的应用程序。
5. 从应用程序的刀片式服务器，请选择**所有设置**。
6. 从**设置**刀片式服务器，选择**属性**。
7. 提供**设置**刀片式服务器上的 FTP 和 FTPS 链接。 

FTPS 的详细信息，请参阅[文件传输协议](http://en.wikipedia.org/wiki/File_Transfer_Protocol)。

## <a name="next-steps"></a>下一步行动

Azure 平台，报告**安全事故或使用不当**，或以通知 Microsoft，您将执行**渗透测试**您的网站，信息的安全性的详细信息，请参阅[微软 Azure 信任中心](https://azure.microsoft.com/support/trust-center/security/)的安全部分。

在应用程序服务应用程序的**web.config**或**applicationhost.config**文件的详细信息，请参阅[配置选项解锁 Azure 应用程序服务 web 应用程序中](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)。

用于应用程序服务的应用程序的日志记录信息的信息，这可能会在检测攻击很有用，请参阅[启用诊断日志记录](web-sites-enable-diagnostic-log.md)。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的起始应用程序的应用程序服务中。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化

* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
