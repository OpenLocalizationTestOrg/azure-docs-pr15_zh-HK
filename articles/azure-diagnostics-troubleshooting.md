<properties
    pageTitle="Azure 的诊断程序的疑难解答"
    description="在 Azure 云服务中，虚拟机使用 Azure 诊断时的疑难解答和 "
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
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure 诊断故障排除
故障排除与使用 Azure 诊断相关的信息。 Azure 的诊断程序的详细信息，请参阅[Azure 诊断程序概述](azure-diagnostics.md#cloud-services)。

## <a name="azure-diagnostics-is-not-starting"></a>无法启动 azure 的诊断

诊断程序由两个组件组成︰ 来宾代理插件和监视 agent。 您可以检查启动诊断程序失败原因的信息的日志文件**DiagnosticsPluginLauncher.log**和**DiagnosticsPlugin.log** 。  
  
在云服务角色中，来宾代理插件的日志文件位于︰ 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

在 Azure 虚拟机来宾代理插件的日志文件位于︰ 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
日志文件的最后一行将包含退出代码。  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

该插件将返回下面的退出代码︰

退出代码|说明
---|---
0|成功。
-1|一般性错误。
-2|无法加载 rcf 文件。<p>这是一个内部错误，应只发生这种情况如果来宾代理插件启动器手动调用，则不正确，在虚拟机上。
-3|无法加载诊断程序的配置文件。<p><p>解决方案︰ 这是不通过架构验证的配置文件的结果。 该解决方案旨在提供一个配置文件，将遵循与架构。
-4|监视代理诊断的另一个实例已经在使用本地资源目录。<p><p>解决方案︰ **LocalResourceDirectory**指定不同的值。
-6|访客代理插件启动程序试图用无效的命令行启动诊断程序。<p><p>这是一个内部错误，应只发生这种情况如果来宾代理插件启动器手动调用，则不正确，在虚拟机上。
-10|与未处理的异常退出诊断程序插件。
-11|访客代理无法创建该进程负责启动和监视监视 agent。<p><p>解决方案︰ 验证足够的系统资源可以用来启动新的进程。<p>
-101|无效参数时调用该诊断程序插件。<p><p>这是一个内部错误，应只发生这种情况如果来宾代理插件启动器手动调用，则不正确，在虚拟机上。
-102|该插件的过程不能初始化自身。<p><p>解决方案︰ 验证足够的系统资源可以用来启动新的进程。
-103|该插件的过程不能初始化自身。 具体而言，它是无法创建记录器对象。<p><p>解决方案︰ 验证足够的系统资源可以用来启动新的进程。
-104|无法加载来宾代理提供的 rcf 文件。<p><p>这是一个内部错误，应只发生这种情况如果来宾代理插件启动器手动调用，则不正确，在虚拟机上。
-105|诊断程序插件无法打开诊断程序的配置文件。<p><p>这是一个内部错误，应只发生这种情况如果诊断插件手动调用，则不正确，在虚拟机上。
-106|无法读取诊断程序的配置文件。<p><p>解决方案︰ 这是不通过架构验证的配置文件的结果。 因此解决方案是提供配置文件符合与架构。 您可以查找发送到文件夹*%SystemDrive%\WindowsAzure\Config*在 VM 中的诊断程序扩展名的 XML。 打开相应的 XML 文件和**Microsoft.Azure.Diagnostics**，然后**xmlCfg**字段搜索。 数据是 base64 编码，因此您需要对[解码](http://www.bing.com/search?q=base64+decoder)XML 加载诊断程序，请参阅。<p>
-107|资源目录传递到监视代理是无效的。<p><p>这是一个内部错误，应只发生这种情况如果监视代理手动调用，则不正确，在虚拟机上。</p>
-108    |无法将诊断程序配置文件转换到监视 agent 配置文件中。<p><p>这是一个内部错误，如果使用无效的配置文件手动调用该诊断程序插件应该只发生。
-110|常规诊断配置错误。<p><p>这是一个内部错误，如果使用无效的配置文件手动调用该诊断程序插件应该只发生。
-111|无法启动监视 agent。<p><p>解决方案︰ 验证足够的系统资源的可用时间。
-112|常规错误


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>诊断数据是 Azure 存储中未记录
Azure 诊断 Azure 存储中存储的所有数据。

丢失的事件数据的最常见原因是不正确地定义的存储帐户信息。

解决方案︰ 更正您诊断程序的配置文件，然后重新安装诊断程序。
如果重新安装诊断程序扩展，则可能需要调试后仍然出现该问题的进一步通过查看通过所有监视代理错误。 事件数据上载到您的存储帐户之前将它存储在 LocalResourceDirectory 中。

LocalResourceDirectory 是云服务角色︰

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

为虚拟机 LocalResourceDirectory 是︰

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

在 LocalResourceDirectory 文件夹中没有文件，如果不能启动监视 agent。 这通常是由无效的配置文件，应在 CommandExecution.log 中报告的事件引起的。

如果监视代理成功收集您会看到.tsf 文件在您的配置文件中定义的每个事件的事件数据。 监视代理记录文件 MaEventTable.tsf 中的错误。 若要检查此文件的内容可以使用 tabel2csv 应用程序将.tsf 文件转换为逗号分隔的 values(.csv) 文件︰

云服务角色︰

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

云服务角色*%系统驱动器 %*通常是 d:

在虚拟机︰

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

上述命令将生成日志文件*maeventtable.csv*，您可以打开并检查有错误消息。    


## <a name="diagnostics-data-tables-not-found"></a>诊断程序未找到表数据
下面的代码使用命名保存 Azure 诊断数据的 Azure 存储中的表︰

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

下面是一个示例︰

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

它将生成表 4:

事件|表名
---|---
提供程序 ="prov1"&lt;事件 id ="1"/&gt;|WADEvent + MD5("prov1") +"1"
提供程序 ="prov1"&lt;事件 id ="2"eventDestination ="dest1"/&gt;|WADdest1
提供程序 ="prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
提供程序 ="prov2" &lt;DefaultEvents eventDestination ="dest2"/&gt;|WADdest2
