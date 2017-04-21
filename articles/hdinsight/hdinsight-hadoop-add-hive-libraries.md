<properties
pageTitle="将配置单元库 HDInsight 群集创建过程中添加 |Azure"
description="了解如何添加配置单元库 （jar 文件），到 HDInsight 群集，在群集创建过程。"
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>在 HDInsight 群集创建过程中添加配置单元库

如果您经常使用在 HDInsight 上的配置单元的库，本文包含有关使用脚本操作在群集创建过程中预加载的库。 这将使库全局可在配置单元 （无需使用[添加 jar/文件夹](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli)来将其加载。）

##<a name="how-it-works"></a>它的工作原理

在创建群集时，您可以选择指定在群集节点运行的脚本，而他们正在创建一个脚本操作。 本文档中的脚本接受单个参数，即包含要预加载的库 （存储为 jar 文件） 的 WASB 位置。

在群集创建过程中脚本枚举的文件，将它们添加到`/usr/lib/customhivelibs/`目录上头部和辅助节点，然后将它们添加到`hive.aux.jars.path`在`core-site.xml`文件。 在基于 Linux 的群集，它还更新`hive-env.sh`扩展名的文件的位置。

> [AZURE.NOTE] 使用本文中的脚本操作使库可在以下情况下︰
>
> * __基于 linux * 的 HDInsight__ -当使用__命令行配置单元__、 __WebHCat__ (Templeton) 和__HiveServer2__。
> * __基于 Windows 的 HDInsight__ -使用__命令行配置单元__和__WebHCat__ (Templeton) 时。

##<a name="the-script"></a>该脚本

__脚本位置__

对于__基于 Linux 的群集__︰ [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

对于__基于 Windows 群集__︰ [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__要求__

* 脚本必须应用到的__头节点__和__辅助节点__。

* 您想要安装 jar 必须在__单个容器__中存储在 Azure Blob 存储。 

* 在创建过程中包含的库中的 jar 文件__必须__存储帐户链接到 HDInsight 群集。 这可以通过两种方式之一来完成︰

    * 通过在群集的默认存储帐户的容器中。
    
    * 通过一个链接的存储容器，在容器中。 例如，在门户网站中可以使用__可选配置__，__链接存储帐户__添加附加存储空间。

* 作为脚本操作的参数必须指定容器的 WASB 路径。 例如，如果 jar 存储在名为__库__上名为__mystorage__的存储帐户的容器中，参数应为__wasbs://libs@mystorage.blob.core.windows.net/__。

    > [AZURE.NOTE] 本文档假定您已经具有创建存储帐户，blob 容器，并向其上载文件。 
    >
    > 如果您没有创建存储帐户，你可以通过[Azure 的门户](https://portal.azure.com)。 然后可以使用[Azure 存储浏览器](http://storageexplorer.com/)之类的实用程序在帐户中创建一个新的容器和将文件上传到它。

##<a name="create-a-cluster-using-the-script"></a>创建群集使用的脚本

> [AZURE.NOTE] 以下步骤将创建一个新的基于 Linux 的 HDInsight 群集。 若要创建一个新的基于 Windows 的群集时创建的群集中，, 为群集操作系统选择__Windows__ ，而不是 bash 脚本使用 Windows (PowerShell) 的脚本。
> 
> 您还可以使用 Azure PowerShell 或 HDInsight.NET SDK 创建群集使用此脚本。 使用这些方法的详细信息，请参阅[自定义 HDInsight 群集使用脚本的操作](hdinsight-hadoop-customize-cluster-linux.md)。

1. 开始调配通过步骤在[Linux 上的设置 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md#portal)中，群集，但未完成资源调配。

2. **可选配置**刀片式服务器，选择**脚本操作**，并提供信息，如下所示︰

    * __名称__︰ 输入脚本操作的友好名称。
    * __脚本的 URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __头部__︰ 选中该选项
    * __工作人员__︰ 选中此选项。
    * __ZOOKEEPER__︰ 将其留空。
    * __参数__︰ WASB 地址输入到包含 jar 的容器和存储帐户。 例如， __wasbs://libs@mystorage.blob.core.windows.net/__。

3. 底部的**脚本操作**，使用**选择**按钮以保存配置。

4. **可选配置**刀片式服务器，选择__链接的存储帐户__并选择__添加存储密钥__链接。 选择包含 jar，存储帐户，然后使用__选择__按钮来保存设置并返回__可选配置__刀片式服务器。

5. 使用**可选配置**刀片底部的**选择**按钮保存可选配置信息。

6. 继续[在 Linux 上的设置 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md#portal)中所述配置群集。

一旦群集创建完成后，您将能够使用 jar 添加到此脚本配置单元而无需使用`ADD JAR`语句。

##<a name="next-steps"></a>下一步行动

本文档中您已了解如何添加到 HDInsight 群集的 jar 文件中包含在群集创建过程中的配置单元库。 使用配置单元的详细信息，请参阅[使用配置单元与 HDInsight](hdinsight-use-hive.md)
