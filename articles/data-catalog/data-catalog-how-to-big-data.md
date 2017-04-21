<properties
   pageTitle="如何使用大数据数据源 |Microsoft Azure"
   description="如何文章突出显示与大数据的数据源，包括 Azure Blob 存储、 Azure 数据湖和 Hadoop HDFS 使用 Azure 数据目录的模式。"
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
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>如何使用 Azure 数据目录中的重要数据源

## <a name="introduction"></a>介绍
**Microsoft Azure 数据目录**是查询的一种完全托管的云服务，作为注册的系统和企业数据源的系统。 换句话说， **Azure 数据目录**是所有关于帮助人们发现、 了解和使用数据源，并帮助企业从现有的数据源，包括大数据获取更大的价值。

**Azure 数据目录**支持注册 Azure 博客存储 blob 和目录，以及 Hadoop HDFS 的文件和目录。 这些数据源的半结构化的性质提供了极大的灵活性，但这也意味着用户必须考虑数据源以获取最大价值从**Azure 数据目录**中注册它们的组织方式。

## <a name="directories-as-logical-data-sets"></a>作为逻辑数据集目录

组织大型的数据源的一种常见的模式是将视为逻辑数据集的目录。 顶层目录用于定义数据集，而子文件夹定义分区，以及它们所包含的文件存储数据本身。

可能此模式的示例︰

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

在此示例中，vehicle_maintenance_events 和 location_tracking_events 表示逻辑数据集。 其中每个文件夹包含数据文件按年和月组织到子文件夹。 其中每个文件夹可能包含成百上千个文件。

在此模式中，可能使用**Azure 数据目录**中注册单独的文件没有意义。 相反，应注册表示将会对用户使用的数据有意义的数据集的目录。

## <a name="reference-data-files"></a>引用数据文件

互为补充的模式是将引用数据设置存储为单个文件。 这些数据集可能会被看作是大数据的 '小' 一侧，而且通常类似于分析数据模型中的维度。 引用数据文件包含用于提供上下文的批量大的数据存储区中存储在其他位置的数据文件的记录。

可能此模式的示例︰

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

当分析师或数据的科学家正在与更大的目录结构中包含的数据时，这些引用文件中的数据可以用于提供更详细的信息的实体的引用仅按名称或 ID 中较大的数据集。

在此模式中，它将意义**Azure 数据目录**中注册的个别的引用数据文件。 每个文件代表一个数据集，而且每个可以批注和单独发现。

## <a name="alternate-patterns"></a>替换模式

上面描述的模式只是两种可行的方法可能会组织一个大型的数据存储区，但每个实现都不相同。 无论数据源构建方式，注册**Azure 数据目录**大数据源时，重点放在注册的文件和目录，表示将会对其他人在您的组织中有价值的数据集。 注册的所有文件和目录可以使混乱的目录，使用户查找他们需要的东西更难。

## <a name="summary"></a>摘要
**Azure 数据目录**中注册的数据源将使它们更容易地发现和了解。 通过注册和大数据文件和表示逻辑数据组的目录添加注释，可以帮助用户查找和使用所需的重要数据源。
