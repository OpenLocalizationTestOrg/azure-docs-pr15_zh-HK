<properties
    pageTitle="发布 HDInsight 应用程序 |Microsoft Azure"
    description="了解如何创建和发布 HDInsight 的应用程序。"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>发布到 Azure 市场的 HDInsight 应用程序

HDInsight 应用程序是用户可以在一个基于 Linux 的 HDInsight 群集安装应用程序。 这些应用程序可以通过 Microsoft，独立软件供应商 (ISV) 或自己开发。 在本文中，您将学习如何发布到 Azure 市场 HDInsight 应用程序。  有关发布到 Azure 市场的一般信息，请参阅[发布 Azure 市场优惠](../marketplace-publishing/marketplace-publishing-getting-started.md)。

HDInsight 应用程序使用*将您自己的许可证 (BYOL)*模型，其中应用程序提供程序负责许可给最终用户，应用程序和最终用户只收取 Azure 创建它们，如 HDInsight 群集和其虚拟机/节点的资源。 此时，应用程序本身的帐单不通过 Azure。

其他的 HDInsight 应用程序相关的文章︰

- [安装 HDInsight 应用程序](hdinsight-apps-install-applications.md)︰ 了解如何安装到群集应用程序 HDInsight。
- [安装自定义的 HDInsight 应用程序](hdinsight-apps-install-custom-applications.md)︰ 了解如何安装和测试自定义的 HDInsight 应用程序。

 
## <a name="prerequisites"></a>系统必备组件

为了提交到市场上自定义应用程序，您必须创建并测试您的自定义应用程序。 请参阅以下文章︰

- [安装自定义的 HDInsight 应用程序](hdinsight-apps-install-custom-applications.md)︰ 了解如何安装和测试自定义的 HDInsight 应用程序。

您还必须具有注册开发者帐户。 请参见[发布到 Azure 市场上的优惠](../marketplace-publishing/marketplace-publishing-getting-started.md)和[创建 Microsoft 开发人员帐户](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md)。

## <a name="define-application"></a>定义应用程序

包含用于发布应用程序到 Azure 市场有两个步骤。  首先要定义一个**createUiDef.json**文件来表明哪些群集应用程序兼容然后将从 Azure 门户模板发布。 下面是一个示例 createUiDef.json 文件。

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|字段  | 说明   | 可能的值|
|-------|---------------|----------------|
|类型  | 与兼容的应用程序的群集类型。    |Hadoop，HBase、 冲击、 触发，（或这些项的任意组合）|
|层  | 应用程序可以兼容与群集层。    |标准，高级 （或两者）|
|版本|  应用程序可以兼容与 HDInsight 群集类型。    |3.4|

## <a name="package-application"></a>封装应用程序

创建一个 zip 文件，其中包含有关安装 HDInsight 应用程序的所有必需的文件。 您将需要在[发布应用程序](#publish-application)中的 zip 文件。

- [createUiDefinition.json](#define-application)。
- mainTemplate.json。 在[安装自定义的 HDInsight 应用程序](hdinsight-apps-install-custom-applications.md)的示例，请参阅。

    >[AZURE.IMPORTANT] 应用程序的安装脚本名称的名称必须是唯一的与下面的格式特定群集。 此外，任何可以安装和卸载脚本操作应是幂等的意味着脚本可以调用重复而产生相同的结果。
    
    >   名称":"[concat (' 色调-安装 v0 '，'-'，uniquestring('applicationName')]"
        
    >请注意，有三个组成部分的脚本名称︰
        
    >   1. 脚本名称前缀，应包括应用程序名称或相关的应用程序的名称。
    >   2. 一个"-"以提高可读性。
    >   3. 应用程序名作为参数与一个唯一的字符串函数。

    >   例如，成为上述最终︰ 色调-安装-v0-4wkahss55hlas 保留的脚本操作列表中。 有关示例 JSON 负载，请参阅[https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json)。

- 所有必需的脚本。

> [AZURE.NOTE] 应用程序文件 （包括 web 应用程序文件，如果有任何） 可以位于任何可公开访问的终结点。

## <a name="publish-application"></a>发布应用程序

请按照以下步骤来发布 HDInsight 应用程序︰

1. 登录到[发布 Azure 门户](https://publish.windowsazure.com/)。
2. 从左边来创建新的解决方案模板，请单击**解决方案模板**。
3. 输入标题，然后单击**创建新的解决方案模板**。
3. 单击**创建帐户并加入 Azure 的程序的开发人员中心**注册您的公司，如果还没有这么做。  请参阅[创建 Microsoft 开发人员帐户](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md)。
4. 单击**定义某些拓扑结构着手**。 解决方案模板是"父"对所有其拓扑结构。 一个优惠中的解决方案模板中，可以定义多个拓扑。 当提供被推送到组配时，被按其拓扑结构的所有。 
4. 一个拓扑结构的名称，请输入，然后单击加号。
5. 输入一个新的版本，然后单击加号。
6. 在[包应用程序](#package-application)中准备的 zip 文件上载。  
7. 单击**申请证书**。 Microsoft 认证团队将查看这些文件，并保证拓扑结构。

## <a name="next-steps"></a>下一步行动

- [安装 HDInsight 应用程序](hdinsight-apps-install-applications.md)︰ 了解如何安装到群集应用程序 HDInsight。
- [安装自定义的 HDInsight 应用程序](hdinsight-apps-install-custom-applications.md)︰ 了解如何部署到 HDInsight 未发布的 HDInsight 应用程序。
- [基于自定义 Linux HDInsight 群集使用脚本操作](hdinsight-hadoop-customize-cluster-linux.md)︰ 了解如何使用脚本的操作来安装其他应用程序。
- [基于创建 Linux 的 Hadoop 群集中使用 Azure 资源管理器模板 HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md)︰ 了解如何调用资源管理器模板来创建 HDInsight 群集。
- [使用 HDInsight 中的空的边缘节点](hdinsight-apps-use-edge-node.md)︰ 了解如何使用一个空的边缘节点访问 HDInsight 群集、 测试 HDInsight 的应用程序和承载 HDInsight 应用程序。

