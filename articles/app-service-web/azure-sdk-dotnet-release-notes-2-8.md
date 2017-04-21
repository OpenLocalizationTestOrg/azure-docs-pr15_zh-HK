
<properties 
   pageTitle="用于.NET 2.8 azure SDK 发行说明" 
   description="用于.NET 2.8 azure SDK 发行说明" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>用于.NET 2.8、 2.8.1 和 2.8.2 azure SDK

##<a name="overview"></a>概述
 
本文包含的发行说明 （包括已知的问题和重大更改） 的 Azure sdk.NET 2.8，2.8.1 和 2.8.2 释放。 

新功能以及在此版本中所做的更新的完整列表，请参阅[Visual Studio 2013年和 2015 Visual Studio Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)公告。 

##  <a name="azure-sdk-for-net-28"></a>使.NET 2.8 的 azure SDK

### <a name="download-azure-sdk-for-net-28"></a>下载 SDK Azure.net 2.8

[.NET 2.8 对 Visual Studio 2015 的 azure SDK](http://go.microsoft.com/fwlink/?LinkId=699285) 

[对于.NET 2.8 对 Visual Studio 2013 azure SDK](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>.NET 4.5.2 支持 

####<a name="known-issues"></a>已知的问题

Azure.NET SDK 2.8 使您得以创建.NET 4.5.2 云服务包。 但是.NET 4.5.2 框架将不能安装在直到 1 月 2016 来宾 OS 的来宾 OS 映像版本的默认值。 前 4.5.2，.NET 框架可通过单独的来宾操作系统发布版本 – 11 月 2015年-02。 请参阅[Azure 来宾 OS 版本和 SDK 兼容性列表](../cloud-services/cloud-services-guestos-update-matrix.md)页后，可以跟踪图像将被释放。  11 月 2015年-02 图像被释放后，您可以通过更新您的云服务配置文件 (.cscfg) 文件中使用该图像。 在服务配置文件的服务配置元素的 osVersion 属性设置为字符串"华盛顿-来宾-OS-4.26_201511-02"。 如果选择选择要将此图像，然后将不再得到来宾操作系统的自动更新。 要获取自动更新 osVersion 必须设置为"*"和.NET 4.5.2 仅可以通过自动更新在 2016 年 1 月。

###<a name="azure-data-factory"></a>Azure 数据工厂

####<a name="known-issues"></a>已知的问题 

**数据工厂模板**在创建项目期间涉及的示例数据，azure 电源外壳脚本可能会失败，如果在计算机上安装的 azure 电源外壳版本后 0.9.8。

要成功地创建这种类型的项目，您必须安装[azure 电源外壳版本 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)。


### <a name="azure-resource-manager-tools"></a>Azure 的资源管理器工具 

####<a name="breaking-changes"></a>重大更改

在此版本中使用新的 Azure PowerShell cmdlet 1.0 版已更新 PowerShell 脚本提供的 Azure 资源组项目。  此新的脚本不会从使用 Visual Studio 在使用之前 2.8 SDK 版本时。  

从 SDK 的早期版本中创建的项目将不运行脚本从 Visual Studio 内使用 2.8 SDK 时。  所有脚本将都继续使用 Visual Studio 之外 Azure PowerShell cmdlet 的相应版本。  

2.8 SDK 要求 Azure PowerShell cmdlet 的 1.0 版。  所有其他版本的 SDK 要求版本 0.9.8 的 Azure PowerShell cmdlet。  有关详细信息，请参见[以下](http://go.microsoft.com/fwlink/?LinkID=623011)博客。

###<a name="web-tools-extensions"></a>Web 工具扩展

####<a name="known-issues"></a>已知的问题

以下已知的问题将在以下版本中得到解决。

- 有关应用程序服务的云和服务器资源管理器不能用于非生产环境 （如 Azure 中国或 Azure 堆栈的客户） 的笔势。 对于在这些受影响的区域的客户，从 Azure 的门户网站下载的发布配置文件将启用发布功能。 未来的版本将 Azure 中国和堆栈客户修复如"连接调试器"和"查看流式传输日志"的手势。 
- 在创建应用程序理解到他们部署实例时处于东亚美国以外地区的应用程序服务，客户可能会看到错误。 在这些情况下，在门户网站中创建的应用程序服务和下载的发布配置文件将使发布方案。 

###<a name="azure-hdinsight-tools"></a>Azure HDInsight 工具

####<a name="new-updates"></a>新的更新

- 可以与几乎任何开销通过 HiveServer2 群集中执行配置单元查询并查看作业日志实时。
- 使用新配置单元任务执行视图您可以深入了解更深入工作，找到更多详细信息，并发现潜在的问题。

信息，请参见[为 Visual Studio 2013年和 2015 Visual Studio Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)。 

## <a name="azure-sdk-for-net-281"></a>使.NET 2.8.1 的 azure SDK

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013年和 2015 Visual Studio 的已知的问题
 
1. 触发的 WebJob 插槽将发行放映和错误，将不会设置一个计划，但它将推 WebJob 到 Azure。 计划作业需要的客户可以使用 Azure 门户设置 WebJob 的计划。 
2. Python 的客户可能会遇到调试程序问题。 服务团队为此推出了修补程序，但如果客户受到影响，请让 Microsoft 知道在论坛或者发布博客上或松开注释备注节。 
3. 在某些地区 （如印度南部） 的客户会遇到资源调配错误的应用程序服务。 这是与门户，保持一致，遇到此问题的客户可以使用 Azure 门户请求发布到这些地理区域的访问权限。 一旦他们请求访问这些地区使用 Azure 的门户资源调配应工作。 

##<a name="azure-sdk-for-net-282"></a>使.NET 2.8.2 的 azure SDK

按照 2.8.2 的安装工具，客户可能会遇到以下问题。         

- 如果使用的 Windows 10 并且未安装 Internet Explorer，可能会收到"找不到 Internet Explorer"错误。
若要解决此问题，请安装 Internet Explorer 使用添加/删除 Windows 组件对话框。

如果您注意到此问题，使用发送一个微笑功能报告。

有关详细信息，请参阅[此](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/)过帐。
##<a name="other-updates"></a>其他更新程序

其他更新程序，请参阅[发布 Azure SDK 2.8 公告](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)。

##<a name="also-see"></a>另请参阅

[Azure SDK 2.8 发布公告](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[支持和 Azure SDK 以及.NET Api 的退休信息](https://msdn.microsoft.com/library/azure/dn479282.aspx)

