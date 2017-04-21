<properties
   pageTitle="Azure 数据目录发行说明 |Microsoft Azure"
   description="Azure 数据目录的发行说明。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure 数据目录发行说明

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>11 月 20，2015年的说明发布 Azure 数据目录

### <a name="opening-data-sources-in-power-bi-desktop"></a>在电源双桌面打开数据源

使用从**Azure 数据目录**入口"打开中电源双桌面"选项时，用户可能会遇到一个电源双桌面应用程序中的两个问题︰

- 显示了标题为"找不到打开的文档"对话框
- 打开电源双桌面应用程序，但该文件似乎为空

对于每种情况下，可以通过从下载和安装最新版本的电源双桌面[PowerBI.com](https://powerbi.com)解决问题。

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>11 月，于 2015 年 13 说明发布 Azure 数据目录

### <a name="registering-and-connecting-to-teradata"></a>注册并连接到 Teradata

当连接到所用的软件位数 （32 位或 64 位） 匹配的 Teradata 数据源用户必须安装正确的 Teradata ODBC 驱动程序。

此日期 ADC 版本，最新的[windows （版本 15.10） Teradata ODBC 驱动程序](http://downloads.teradata.com/download/connectivity/odbc-driver/windows)是否兼容 Office 2013，但不是与 Office 2016。

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>7 月，于 2015 年 13 说明发布 Azure 数据目录

### <a name="registering-and-connecting-to-oracle-database"></a>注册并连接到 Oracle 数据库

当连接到 Oracle 数据库的数据源的用户必须已安装了正确的 Oracle 驱动程序相匹配所用的软件的位 （32 位或 64 位）。

-   注册时运行 32 位 Windows 的计算机上的 Oracle 数据源，请将使用 32 位 Oracle 驱动程序
-   注册时运行 64 位 Windows 的计算机上的 Oracle 数据源，请将使用 64 位 Oracle 驱动程序
-   连接到 Oracle 数据源运行的 Microsoft Office 的 32 位版本的计算机上使用 Excel 时，包括在 64 位 Windows 上的 32 位 Oracle 驱动程序将使用
-   64 位 Oracle 驱动程序连接到 Oracle 数据源运行的 Microsoft Office 的 64 位版本的计算机上使用 Excel 时，将使用

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>注册和连接到 SQL Server 报告服务

对 SQL Server 报表服务 (SSRS) 数据源支持是当前限制为本机模式的服务器。 将在以后的版本中添加 SSRS 支持 SharePoint 模式。

### <a name="opening-data-assets-in-excel"></a>在 Excel 中打开数据资产

当在 Microsoft Excel 中打开数据资产，从**Azure 数据目录**入口， **Microsoft Excel 安全通知**对话框中可能会提示用户。 这是标准的预期的行为，并且用户可以选择**启用**以继续。

有关详细信息，请参阅[启用或禁用有关链接和文件来自可疑网站的安全警报](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE)。

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>代理和策略配置和数据源注册

用户可能会遇到他们从何处可以登录到 Azure 数据目录门户，但在尝试登录到数据源注册工具遇到错误消息，可以阻止登录时的情况。

有这种问题行为的两个可能的原因︰

**原因 1: Active Directory 联合身份验证服务配置**数据源注册工具使用窗体身份验证来验证对 Active Directory 用户登录。 对于成功的登录窗体身份验证必须在全局身份验证策略中启用 Active Directory 管理员。

在某些情况下，只有当用户是在公司网络上，或仅从公司网络外部连接的用户，可能发生这种错误行为。 全局身份验证策略允许分别为企业内部网和外部网连接启用的身份验证方法。 如果未启用窗体身份验证的用户进行连接的网络，则可能会出现登录错误。

有关详细信息，请参阅[配置身份验证策略](https://technet.microsoft.com/library/dn486781.aspx)。

**原因 2︰ 网络代理配置**如果公司的网络使用代理服务器，注册工具可能无法通过代理服务器连接到 Azure 活动目录。 用户可以确保注册工具编辑工具的配置文件，将这一节添加到文件︰


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


为找到 RegistrationTool.exe.config 文件，启动注册工具，然后打开 Windows 任务管理器实用程序。 在任务管理器详细信息选项卡上，在 RegistrationTool.exe 上右击并从弹出式菜单中选择打开文件的位置。
