<properties
    pageTitle="Azure 诊断程序版本历史记录"
    description="作为发运的 Azure 诊断不同 Microsoft Azure Sdk 版本的不同版本中的更改的说明。"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Microsoft Azure 诊断程序版本历史记录

新到 Azure 的诊断？ 请参阅[Azure 诊断程序概述](azure-diagnostics.md)。

每个版本的 SDK Azure 通常附带 Azure 诊断程序的新版本。 下表描述了与 SDK 版本相关的 Azure SDK 和 Azure 诊断程序版本。



Azure SDK 版本 | Azure 诊断程序版本 | 模型
--- | --- | ---
1.x      | 1.0 | 插件
2.0 到 2.4| 1.0 | "
2.5      | 1.2 | 扩展
2.6      | 1.3 | "
2.7      | 1.4 | "
2.8      | 1.5 | "


最新版本是 1.5，附带 Azure SDK 2.8。 随 SDK 的 Azure 诊断程序扩展的版本仅用于本地仿真方案。 而不考虑其版本的 SDK 应用程序使用 Azure，在运行时，任何已部署的应用程序自动使用最新版本。 但是，除非您安装最新的 Azure SDK，您可能不具有所有的工具，可帮助您利用这些新功能。

各种功能并更改如下所述。

## <a name="azure-sdk-28"></a>Azure SDK 2.8
Azure SDK 2.8 添加诊断数据发送到[应用程序的见解](./application-insights/app-insights-cloudservices.md)更方便地在您的应用程序，以及系统和基础结构级别诊断问题的能力。

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 诊断更改

在 Visual Studio 中的 Azure SDK 2.6 云服务项目，进行以下更改。 （这些更改也应用于更高版本的 Azure SDK。）

- 本地的仿真程序现在支持诊断。 这意味着您可以收集诊断数据并确保您的应用程序而正在开发并测试在 Visual Studio 中创建适当的跟踪。 连接字符串`UseDevelopmentStorage=true`在 Visual Studio 中使用 Azure 存储仿真器运行云服务项目的同时，使诊断数据收集。 所有诊断数据都收集在 （开发存储） 存储帐户。

- 诊断程序存储帐户连接字符串 (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) 再一次存储服务 (.cscfg) 在配置文件中。 在 Azure SDK 2.5 diagnostics.wadcfgx 文件中指定诊断存储帐户。

有一些在 Azure SDK 2.4 英寸及更早版本连接字符串的工作方式和原理在 Azure SDK 2.6 或更高版本之间的显著差异。

- 在 Azure SDK 2.4 和更早版本，连接字符串被用作运行时诊断插件获取转移诊断日志的存储帐户信息。

- 在 Azure SDK 2.6 或更高版本，诊断连接字符串用于通过 Visual Studio 配置诊断扩展用适当的存储帐户信息发布。 连接字符串，可以定义不同的服务配置发布时，Visual Studio 将使用不同的存储帐户。 但是，诊断插件 （后 Azure SDK 2.5) 已不再可用，因为本身的.cscfg 文件不能启用诊断程序扩展。 您必须启用分别通过 Visual Studio 或 PowerShell 等工具扩展。

- 为了简化配置使用 PowerShell 诊断程序扩展的过程，包输出从 Visual Studio 还包含公用配置 XML 为诊断程序扩展为每个角色。 Visual Studio 使用诊断连接字符串填充中的公用配置的存储帐户信息。 公用配置文件扩展文件夹中创建并按照 PaaSDiagnostics 的模式。<RoleName>.PubConfig.xml。 任何基于 PowerShell 部署可以使用此模式映射到某个角色的每一种配置。

- Azure 门户还用.cscfg 文件中的连接字符串来访问诊断数据，因此它可以显示在**监视**选项卡。 连接字符串需要配置的服务，以在门户中显示详细的监控数据。

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>迁移项目到 Azure SDK 2.6 及更高版本

当从 Azure SDK 2.5 到 Azure SDK 2.6 或更高版本，如果您有一个诊断存储客户，.wadcfgx 文件中指定迁移，然后它将保持存在。 要利用不同的存储配置为使用不同的存储帐户的灵活性，您必须手动向项目中添加的连接字符串。 如果您正在迁移项目从 Azure SDK 2.4 或更早到 Azure SDK 2.6，诊断程序的连接字符串将被保留。 但是，请注意如何连接字符串被视为在 Azure SDK 2.6 指定在上一节中所做的更改。

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Visual Studio 如何确定诊断存储帐户

- 如果.cscfg 文件中指定诊断连接字符串，则 Visual Studio 将使用它来配置诊断程序扩展，并在发布时，在打包过程中生成的公用配置 xml 文件时。

- 如果没有诊断程序连接字符串指定的.cscfg 文件中，然后 Visual Studio 回退到使用.wadcfgx 文件中指定的存储帐户来配置诊断扩展时发布，并在包装时生成的公用配置 xml 文件。

- .Cscfg 文件中的诊断程序连接字符串将优先于在.wadcfgx 文件中的存储帐户。 如果.cscfg 文件中指定诊断连接字符串，则 Visual Studio 使用的而忽略中.wadcfgx 的存储帐户。

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>做"更新开发存储连接字符串..." 复选框？

**诊断和使用 Microsoft Azure 存储帐户凭据发布到 Microsoft Azure 时，缓存的更新开发存储连接字符串**的复选框可以方便地更新任何开发存储帐户连接字符串使用 Azure 存储帐户指定发布过程。

例如，假设您选中此复选框，并诊断连接字符串指定`UseDevelopmentStorage=true`。 到 Azure 发布项目时，Visual Studio 将自动使用发布向导中指定的存储帐户更新诊断连接字符串。 但是，如果为诊断程序的连接字符串指定了实际存储帐户，则改为使用该帐户。

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>诊断程序 Azure SDK 2.4 和早期和 Azure SDK 2.5 或更高版本的功能差异

如果您要升级您的项目从 Azure SDK 2.4 到 Azure SDK 2.5 或更高版本，您应当记住下面的诊断功能差异。

- **不建议使用配置 Api** – 诊断程序以编程方式配置可以在 Azure SDK 2.4 和早期版本中，但在 Azure SDK 2.5 或更高版本中不再。 如果诊断配置当前在代码中定义的您需要重新配置这些设置迁移项目中，用于诊断中从头要继续工作。 Azure SDK 2.4 诊断配置文件是 diagnostics.wadcfg 和 diagnostics.wadcfgx Azure SDK 2.5 及更高版本。

- **对于云服务应用程序的诊断只能在角色级别，不能在实例级别配置。**

- **每次部署您的应用程序更新诊断配置**– 如果您从服务器资源管理器中更改您的诊断程序配置，然后重新部署您的应用程序，这可能会导致奇偶校验问题。

- **在 Azure SDK 2.5 和更高版本，崩溃转储配置在诊断程序配置文件中，不在代码中**– 如果有故障转储配置代码中，您需要手动将配置从代码转移到配置文件中，因为在迁移过程中，故障转储不转移到 Azure SDK 2.6。
