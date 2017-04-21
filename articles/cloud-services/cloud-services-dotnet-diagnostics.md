<properties
    pageTitle="如何利用云服务使用 Azure 诊断程序 (.NET) |Microsoft Azure"
    description="使用 Azure 的诊断程序来收集从 Azure 的云服务的调试、 性能、 监控、 流量分析和多个测量数据。"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>启用在 Azure 的云服务的 Azure 诊断

Azure 诊断程序的背景信息，请参阅[Azure 诊断概述](../azure-diagnostics.md)。


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>如何启用辅助角色中的诊断

本演练介绍如何实现发出遥测数据使用.NET EventSource 类 Azure 的辅助角色。 Azure 诊断程序用于收集遥测数据并将其存储在 Azure 存储帐户。 创建辅助角色 Visual Studio 将自动启用诊断 1.0 Azure 的.NET 2.4 英寸及更早版本的 Sdk 中的解决方案的一部分。 下面的说明描述了创建辅助角色中，解决方案部署诊断 1.2 和 1.3 诊断 1.0 禁止对辅助角色的过程。

### <a name="pre-requisites"></a>必备组件
本文假定 Azure 的订购和使用 Visual Studio 2013 Azure SDK。 如果您没有 Azure 的订阅，您可以注册[免费试用版][]。 请确保[安装和配置 Azure PowerShell 版本 0.8.7 或更高版本][]。

### <a name="step-1-create-a-worker-role"></a>步骤 1︰ 创建辅助角色
1.  启动**Visual Studio 2013年**。
2.  从**云**模板面向.NET Framework 4.5 创建一个新的**Azure 云服务**项目。  命名为"WadExample"的项目，然后单击确定。
3.  选择**辅助角色**，然后单击确定。 将创建项目。
4.  在**解决方案资源管理器**中双击**WorkerRole1**属性文件。
5.  在**配置**选项卡中取消选中**启用诊断程序**来禁用诊断 1.0 （Azure SDK 2.4 和 eariler）。
6.  生成解决方案以验证您有任何错误。

### <a name="step-2-instrument-your-code"></a>步骤 2︰ 检测代码
WorkerRole.cs 中的内容替换为以下代码。 从[EventSource 类][]继承的类 SampleEventSourceWriter，实现了四种日志记录方法︰ **SendEnums**、 **MessageMethod**、 **SetOther**和**HighFreq**。 **WriteEvent**方法的第一个参数定义为各自的事件 ID。 Run 方法实现无限循环调用每种日志记录方法在**SampleEventSourceWriter**类中实现每隔 10 秒。

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>第 3 步︰ 部署您的工作角色
1.  从**生成**菜单中选择**WadExample**项目在解决方案资源管理器然后**发布**到 Visual Studio 中从 Azure 部署辅助角色。
2.  选择您的订购。
3.  在**Microsoft Azure 发布设置**对话框中，选择**新建...**。
4.  在**创建云服务和存储帐户**对话框中输入一个**名称**(例如，"WadExample")，并选择地区或相关性组。
5.  将**环境**设置为**转移**。
6.  任何其他**设置**根据需要修改，单击**发布**。
7.  部署完成后验证 Azure 的经典门户中云服务处于**运行**状态。

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>步骤 4︰ 创建诊断程序配置文件和安装扩展
1.  通过执行以下 PowerShell 命令下载公用配置文件架构定义︰
2.
        (Get AzureServiceAvailableExtension-ExtensionName 'PaaSDiagnostics'-ProviderNamespace 'Microsoft.Azure.Diagnostics')。PublicConfigurationSchema |输出文件的编码的 utf8 FilePath 'WadConfig.xsd'

2.  通过右击**WorkerRole1**项目添加到**WorkerRole1**项目的 XML 文件并选择**添加** -> **新项...** ->  **C# 项目** -> **数据** -> **的 XML 文件**。 命名为"WadExample.xml"的文件。

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  将 WadConfig.xsd 与配置文件相关联。 请确保 WadExample.xml 编辑器窗口为活动窗口。 按**F4**打开**属性**窗口。 单击**属性**窗口中对**架构**属性。 单击**...** 在**架构**属性。 单击**添加...** 按钮，导航到保存该 XSD 文件的位置，选择该文件 WadConfig.xsd。 单击**确定**。
4.  WadExample.xml 配置文件的内容替换为下面的 XML 并保存该文件。 此配置文件定义了几个要收集的性能计数器︰ 一个用于 CPU 利用率和内存使用率。 然后配置定义 SampleEventSourceWriter 类中的方法所对应的四个事件。

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>步骤 5︰ 在辅助角色上安装诊断程序
在 web 或辅助角色管理诊断 PowerShell cmdlet 都︰ 集 AzureServiceDiagnosticsExtension，获取-AzureServiceDiagnosticsExtension，并删除 AzureServiceDiagnosticsExtension。

1.  打开 Azure PowerShell。
2.  执行脚本以在您的工作角色 （用 wadexample 存储帐户存储帐户密钥替换*StorageAccountKey* ） 上安装诊断程序︰

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>步骤 6︰ 查看遥测数据
在 Visual Studio 的**服务器资源管理器**导航到的 wadexample 存储帐户。 在云后服务已进行的 5 分钟内应该看到**WADEnumsTable**， **WADHighFreqTable**， **WADMessageTable**、 **WADPerformanceCountersTable**和**WADSetOtherTable**的表。 双击其中一个表以查看已收集的遥测数据。
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>配置文件架构

诊断程序的配置文件定义了用于诊断代理启动时初始化诊断配置设置的值。 请参阅[最新架构引用](https://msdn.microsoft.com/library/azure/mt634524.aspx)的有效值和示例。

## <a name="troubleshooting"></a>故障排除

如果您有问题，请参见[疑难解答 Azure 诊断](../azure-diagnostics-troubleshooting.md)常见问题的帮助信息

## <a name="next-steps"></a>下一步行动
[请参阅虚拟机的列表相关 Azure 诊断文章](azure-diagnostics.md#cloud-services)来更改您所收集的数据解决问题或一般情况了解诊断的详细信息。


[EventSource 类]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[免费试用版]: http://azure.microsoft.com/pricing/free-trial/
[安装和配置 Azure PowerShell 版本 0.8.7 或更高版本]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
