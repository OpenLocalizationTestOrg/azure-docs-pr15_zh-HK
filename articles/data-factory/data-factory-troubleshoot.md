<properties 
    pageTitle="Azure 数据工厂问题的疑难解答" 
    description="了解如何解决使用 Azure 数据工厂的问题。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>解决数据工厂的问题
使用 Azure 数据工厂时，这篇文章的问题提供故障排除技巧。 这篇文章没有列出所有可能出现的问题，在使用该服务，但它涵盖的一些问题和一般故障排除技巧。   

## <a name="troubleshooting-tips"></a>故障排除提示

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>错误︰ 该订阅未注册使用 Microsoft.DataFactory 的命名空间
如果您收到此错误，Azure 数据工厂资源提供程序尚未注册您的计算机上。 请执行以下操作︰ 

1. 启动 Azure PowerShell。 
2. 登录到您使用以下命令的 Azure 帐户。
        登录 AzureRmAccount 
3. 运行以下命令以注册 Azure 数据工厂提供程序。
        注册-AzureRmResourceProvider-ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>问题︰ 未经授权的错误时运行数据工厂 cmdlet
可能未使用 Azure PowerShell 使用右 Azure 帐户或订阅。 使用以下 cmdlet 选择右 Azure 帐户和使用 Azure PowerShell 的订阅。 

1. 登录 AzureRmAccount-使用适当的用户 ID 和密码
2. 获得 AzureRmSubscription-查看帐户的所有订阅。 
3. 选择 AzureRmSubscription&lt;订阅名称&gt;-选择适当的订阅。 使用同一个使用 Azure 的门户网站上创建的数据工厂。

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>问题︰ 无法从 Azure 门户启动数据管理网关速成版安装程序
速成版安装程序数据管理网关需要 Internet Explorer 或 Microsoft ClickOnce 兼容的 web 浏览器。 如果速成版安装程序无法启动，请执行以下任一操作︰ 

- 使用 Internet Explorer 或 Microsoft ClickOnce 兼容的 web 浏览器。

    如果您使用的铬，请转到[Chrome web 存储区](https://chrome.google.com/webstore/)，用"ClickOnce"关键字搜索，请选择一个 ClickOnce 扩展中，并将其安装。 
    
    执行相同的操作 （安装加载项） 的 firefox。 单击打开的菜单工具栏上的按钮 （三个水平行中的右上角），单击附件、"ClickOnce"关键字搜索、 选择一个 ClickOnce 扩展中，并安装。 

- 使用显示在门户中同一刀片式服务器上**手动安装**链接。 使用此方法下载安装文件并手动运行它。 成功安装后，您会看到数据管理网关配置对话框。 从门户屏幕复制**键**并使用配置管理器中手动注册该服务网关。  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>问题︰ 无法连接到 SQL Server 内部 
网关计算机上启动**数据管理网关配置管理器**，用于测试连接到 SQL Server 中的网关计算机**故障排除**选项卡。 请参阅故障排除连接/网关版的提示[疑难解答网关问题](data-factory-data-management-gateway.md#troubleshoot-gateway-issues)相关的问题。   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>问题︰ 输入片都在过等待状态

扇区可能处于**等待**状态，因为不同的原因。 常见的原因之一是**外部**属性未设置为**true**。 应使用**外部**属性标记的 Azure 数据工厂范围之外产生任何数据集。 此属性指示数据是外部和不只后盾数据工厂内任何管线。 数据切片标记为**就绪**后各自存储区中有可用的数据。 

请参阅下面的示例使用的**外部**属性。 您可以选择指定**externalData*** 当设置为 true 的外部。

有关此属性的更多详细信息请参见[数据集](data-factory-create-datasets.md)文章。
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

若要解决该错误，请向 JSON 定义输入表中添加**外部**属性和可选的**externalData**节并重新创建表。 

### <a name="problem-hybrid-copy-operation-fails"></a>问题︰ 混合复制操作失败
请参阅[网关问题的疑难解答](data-factory-data-management-gateway.md#troubleshoot-gateway-issues)步骤来解决复制到从内部数据存储区使用数据管理网关的问题。 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>问题︰ 按需 HDInsight 配置失败
当使用链接的类型 HDInsightOnDemand 的服务，您需要指定指向 Azure Blob 存储 linkedServiceName。 数据工厂服务使用此存储来存储日志和按需 HDInsight 群集的支持文件。  有时按需 HDInsight 群集的资源调配时失败，出现以下错误︰

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

此错误通常指示存储帐户在 linkedServiceName 中指定的位置不在同一个数据中心位置在何处发生 HDInsight 调配。 示例︰ 如果数据工厂位于美国西部和 Azure 存储是在美国东部，按需设置失败在西部美国。

此外，还有第二个的 JSON 属性 additionalLinkedServiceNames 可能在点播 HDInsight 中指定附加的存储账户。 这些额外的链接的存储帐户应该是在 HDInsight 群集中，相同的位置或它将失败，并出现同样的错误。

### <a name="problem-custom-net-activity-fails"></a>问题︰ 自定义.NET 活动失败
有关详细步骤，请参阅[调试自定义活动的管线](data-factory-use-custom-activities.md#debug-the-pipeline)。 

## <a name="use-azure-portal-to-troubleshoot"></a>使用 Azure 门户网站疑难解答 

### <a name="using-portal-blades"></a>使用门户刀片
步骤，请参阅[监视管道](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline)。 

### <a name="using-monitor-and-manage-app"></a>使用监视和管理应用程序
请参阅[显示器和管理数据工厂管线使用监视和管理应用程序](data-factory-monitor-manage-app.md)的详细信息。 

## <a name="use-azure-powershell-to-troubleshoot"></a>使用 Azure PowerShell 的疑难解答

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>使用 Azure PowerShell 解决错误  
详细信息，请参阅[使用 Azure PowerShell 的监视器数据工厂管线](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline)。 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 