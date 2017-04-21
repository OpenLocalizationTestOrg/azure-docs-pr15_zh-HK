<properties
    pageTitle="在 HDInsight 中使用空的边缘节点 |Microsoft Azure"
    description="如何将 ampty 边节点添加到 HDInsight 群集，可以用作一个客户端，以及如何测试/主机 HDInsight 应用程序。"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>在 HDInsight 中使用空的边缘节点

了解如何添加到基于 Linux 的 HDInsight 群集空边节点。 空边节点是一个 Linux 虚拟机具有相同客户端工具的安装和配置如下所示的 headnodes，但是没有正在运行的 hadoop 服务。 可以使用边缘节点访问群集、 测试客户端应用程序和托管客户端应用程序。 

向现有 HDInsight 群集，到一个新的群集创建群集时，您可以添加空边节点。 添加一个空的边缘节点是使用 Azure 资源管理器模板。  下面的示例演示完成的如何使用模板︰

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

如示例中所示，可以选择调用[脚本操作](hdinsight-hadoop-customize-cluster-linux.md)来执行其他配置，如在边缘节点中安装[Apache 色调](hdinsight-hadoop-hue-linux.md)。

您已经创建了一个边缘节点后，可以连接到使用 SSH，在边缘节点并运行客户端工具来访问 HDInsight 在 Hadoop 群集。

## <a name="add-an-edge-node-to-an-existing-cluster"></a>将边缘节点添加到一个现有的群集

在本节中，您使用资源管理器模板将边缘节点添加到现有的 HDInsight 群集。  在[GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode)中找不到资源管理器模板。 资源管理器模板调用脚本操作脚本位于 https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh。 该脚本不会执行任何操作。  这是演示从资源管理器模板调用脚本操作。

**若要将空边节点添加到一个现有的群集**

1. 如果您还没有，创建一个 HDInsight 的群集。  请参阅[Hadoop 教程︰ 开始在 HDInsight 中使用基于 Linux 的 Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md)。
2. 请单击以下图像中登录到 Azure 并在 Azure 的门户打开 Azure 资源管理器的模板。 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. 配置以下属性︰

    - 群集名称︰ 输入现有的 HDInsight 群集的名称。
    - EDGENODESIZE︰ 选择一个 VM 大小。
    - EDGENODEPREFIX︰ 为**新**的默认值。  边缘节点名称使用的默认值，则**新 edgenode**。  您可以自定义门户的前缀。 您还可以自定义模板中的完整名称。


4. 单击**确定**以保存所做的更改。
5. 在**资源组**中，选择资源组。
6. **审查法律条款**，请单击，然后单击**购买**。
7. 选择**附到仪表板**，然后再单击**创建**。

## <a name="add-an-edge-node-when-creating-a-cluster"></a>创建群集时添加边缘节点

在本节中，您使用资源管理器模板边缘节点创建 HDInsight 群集。  公共[Azure Blob 存储容器](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json)中找不到资源管理器模板。 资源管理器模板调用脚本操作脚本位于 https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh。 该脚本不会执行任何操作。  这是演示从资源管理器模板调用脚本操作。

**若要将空边节点添加到一个现有的群集**

1. 如果您还没有，创建一个 HDInsight 的群集。  请参阅[Hadoop 教程︰ 开始在 HDInsight 中使用基于 Linux 的 Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md)。
2. 请单击以下图像中登录到 Azure 并在 Azure 的门户打开 Azure 资源管理器的模板。 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. 配置以下属性︰
        
    - 群集名称︰ 输入创建新群集的名称。
    - CLUSTERLOGINUSERNAME︰ 输入 Hadoop HTTP 用户名称。  默认名称是**管理员**。
    - CLUSTERLOGINPASSWORD︰ 输入 Hadoop HTTP 用户密码。
    - SSHUSERNAME︰ 输入 SSH 用户名称。 默认名称是**sshuser**。
    - SSHPASSWORD︰ 输入 SSH 用户密码。
    - 位置︰ 输入资源组的名称、 群集，以及默认的存储帐户的位置。
    - CLUSTERTYPE: 默认值为**hadoop**。
    - CLUSTERWORKERNODECOUNT︰ 默认值为 2。
    - EDGENODESIZE︰ 选择一个 VM 大小。
    - EDGENODEPREFIX︰ 为**新**的默认值。  边缘节点名称使用的默认值，则**新 edgenode**。  您可以自定义门户的前缀。 您还可以自定义模板中的完整名称。

4. 单击**确定**以保存所做的更改。
5. 在**资源组**中，输入新的资源组名称。
6. **审查法律条款**，请单击，然后单击**购买**。
7. 选择**附到仪表板**，然后再单击**创建**。 


## <a name="access-an-edge-node"></a>访问边缘节点

边缘节点 ssh 终结点是<EdgeNodeName>。<ClusterName>-ssh.azurehdinsight.net:22。  例如，新的 edgenode.myedgenode0914-ssh.azurehdinsight.net:22。

边缘节点显示为 Azure 的门户网站上的应用程序。  门户网站可以访问使用 SSH 的边缘节点的信息。

**要验证的边缘节点 SSH 终结点**

1. 登录到[Azure 的门户](https://portal.azure.com)。
2. 用边缘节点打开 HDInsight 群集。
3. 从群集刀片式服务器，请单击**应用程序**。 您应看到边缘节点。  默认名称是**新 edgenode**。
4. 单击边缘节点。 您应看到 SSH 终结点。

**若要使用在边缘节点上配置单元**

5. 使用 SSH 连接到边缘节点。  请参阅[使用 HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)或[使用 HDInsight 从 Windows 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)。
6. 您连接到使用 SSH 在边缘节点后，使用下面的命令以打开配置单元控制台︰

        hive
7. 运行以下命令，以显示在群集中配置单元的表︰

        show tables;

## <a name="delete-an-edge-node"></a>删除边缘节点

您可以从 Azure 门户删除边缘节点。

**访问边缘节点**

1. 登录到[Azure 的门户](https://portal.azure.com)。
2. 用边缘节点打开 HDInsight 群集。
3. 从群集刀片式服务器，请单击**应用程序**。 您应看到边缘节点的列表。  
4. 用鼠标右键单击想要删除的边缘节点，然后单击**删除**。
5. 单击**是**确认。

## <a name="next-steps"></a>下一步行动

在本文中，您学习了如何添加边缘节点以及如何访问边缘节点。 若要了解详细信息，请参阅下列文章︰

- [安装 HDInsight 应用程序](hdinsight-apps-install-applications.md)︰ 了解如何安装到群集应用程序 HDInsight。
- [安装自定义的 HDInsight 应用程序](hdinsight-apps-install-custom-applications.md)︰ 了解如何部署到 HDInsight 的 HDInsight 应用程序发布。
- [发布的 HDInsight 应用程序](hdinsight-apps-publish-applications.md)︰ 了解如何发布您的自定义 HDInsight 应用程序到 Azure 市场。
- [MSDN︰ 安装应用程序 HDInsight](https://msdn.microsoft.com/library/mt706515.aspx)︰ 了解如何定义 HDInsight 应用程序。
- [基于自定义 Linux HDInsight 群集使用脚本操作](hdinsight-hadoop-customize-cluster-linux.md)︰ 了解如何使用脚本的操作来安装其他应用程序。
- [基于创建 Linux 的 Hadoop 群集中使用资源管理器模板 HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md)︰ 了解如何调用资源管理器模板来创建 HDInsight 群集。

