<properties
    pageTitle="团队数据科学过程中的操作︰ 使用 SQL Server |Microsoft Azure"
    description="高级分析功能流程和操作技术"  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>团队数据科学过程中的操作︰ 使用 SQL Server

在本教程，您演练中生成并部署使用 SQL Server 和公开数据集-[纽约出租车上行程](http://www.andresmh.com/nyctaxitrips/)数据集的机器学习模型。 该过程执行以下标准数据科学工作流︰ 摄取和研究数据，工程师可以促进学习，然后生成和部署模型的特点。


## <a name="dataset"></a>纽约出租车上遍历数据集描述

纽约出租车上行程数据大约 20 GB 的压缩 CSV 文件 (压缩后 ~ 48 GB)，其中包含超过 173 万个人行程和可支付的每一行程。 旅行中的每个记录包括挑选和放置位置和时间、 anonymized 抗攻击 （驱动） 许可证编号和 medallion (taxi 的唯一 id) 号。 数据在 2013 年中涵盖所有的行程和中提供了以下两个数据集每个月︰

1. Trip_data CSV 包含行程的详细信息，例如数名乘客、 装货和 dropoff 点、 行程时间和行程的长度。 下面是几个示例记录︰

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Trip_fare CSV 包含每个行程，付款类型、 票费金额、 附加费和增值税、 提示和工具，例如支付票费以及支付的总金额的详细信息。 下面是几个示例记录︰

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

要加入行程的唯一键\_数据和行程\_票费由字段组成︰ medallion，攻击\_许可证和拾取\_日期时间。

## <a name="mltasks"></a>预测任务的示例

我们将阐明三种预测问题，基于*提示\_量*，即︰

1. 二元分类︰ 预测是否提示已支付的行程，即*提示\_量*更大比 0 美元是一个积极的例子，而*提示\_量*0 美元是一个负面的例子。

2. Multiclass 分类︰ 预测提示为旅行支付的范围。 我们除*提示\_量*到五箱或多个类︰

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. 回归分析任务︰ 预测的提示支付的旅行量。  


## <a name="setup"></a>设置了 Azure 数据科学环境的高级分析功能

您[计划您的环境](machine-learning-data-science-plan-your-environment.md)指南 》 中可以看到，有几个选项可以使用 Azure 在纽约出租车上行程数据集︰

- 使用 Azure blob 中的数据，然后在 Azure 机器学习模型
- 将数据加载到 SQL Server 数据库，然后在 Azure 机器学习模型

在本教程中，我们将演示并行批量导入的数据复制到 SQL Server，数据研究、 功能设计和下采样使用 SQL Server 管理 Studio 以及使用 IPython 笔记本。 在 GitHub 共享[示例脚本](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)和[IPython 笔记本](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)。 在相同位置还有示例 IPython 笔记本使用 Azure blob 中的数据。

若要设置 Azure 数据科学环境︰

1. [创建存储帐户。](../storage/storage-create-storage-account.md)

2. [创建一个 Azure 机器学习工作区](machine-learning-create-workspace.md)

3. [提供数据科学虚拟机](machine-learning-data-science-setup-sql-server-virtual-machine.md)，它可作为 SQL Server 也 IPython 笔记本服务器。

    > [AZURE.NOTE] 示例脚本和 IPython 笔记本将下载到您的数据科学虚拟机安装过程。 当 VM 安装后脚本执行完毕时，样本将在虚拟机的文档库︰  
    > - 示例脚本︰`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - 示例 IPython 笔记本︰`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > 其中`<user_name>`是虚拟机的 Windows 登录名。 我们将参考**示例脚本**和**示例 IPython 笔记本**作为示例文件夹。


基于数据集大小、 数据源位置和选定的 Azure 目标环境，这种情况是类似于[方案\#5︰ 大型数据集在本地文件中，目标 SQL Server Azure VM 中的](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb)。

## <a name="getdata"></a>从公共源获取数据

为了从其公用位置获得[纽约出租车上行程](http://www.andresmh.com/nyctaxitrips/)数据集，您可以使用任何[移动数据和 Azure Blob 存储](machine-learning-data-science-move-azure-blob.md)在所述的方法将数据复制到新的虚拟机。

使用 AzCopy 将数据复制︰

1. 登录到您的虚拟机 (VM)

2. 在虚拟机的数据磁盘中创建一个新目录 (注意︰ 不要使用临时磁盘与数据磁盘作为 VM)。

3. 在命令提示符窗口中，运行下面的 Azcopy 命令行，< path_to_data_folder > 替换为您在 (2) 中创建的数据文件夹︰

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    当 AzCopy 完成时，总共 24 个压缩 CSV 文件 (行程 12\_数据和行程 12\_票费) 应数据文件夹中。

4. 将下载的文件解压缩。 请注意解压缩后的文件所在的文件夹。 此文件夹将被称为 < 路径\_到\_数据\_文件\>。

## <a name="dbload"></a>批量导入到 SQL Server 数据库的数据

通过使用_分区表和视图_，可以提高加载/传输大量的数据复制到一个 SQL 数据库和后续查询的性能。 在本节中，我们将按照[并行批量数据导入使用 SQL 分区表](machine-learning-data-science-parallel-load-sql-partitioned-tables.md)创建一个新数据库并将数据加载到已分区表并行中描述的说明。

1. 登录到您的 VM，则启动**SQL Server 管理 Studio**。

2. 使用 Windows 身份验证进行连接。

    ![SSMS 连接][12]

3. 如果您有尚未更改 SQL Server 身份验证模式，创建新的 SQL 登录用户打开脚本文件命名为**更改\_auth.sql** **示例脚本**文件夹中。 更改默认的用户名和密码。 单击**！执行**在工具栏中运行脚本。

    ![执行脚本][13]

4. 验证和/或更改的 SQL Server 默认数据库和日志文件夹以确保新创建的数据库将存储在数据磁盘。 对于 datawarehousing 装载优化 SQL Server 虚拟机映像是预配置数据和日志的磁盘配置。 如果 VM 不包括数据磁盘和虚拟机安装过程中添加新的虚拟硬盘，更改默认文件夹，如下所示︰

    - 用鼠标右键单击左窗格中的 SQL Server 名称并单击**属性**。

        ![SQL Server 属性][14]

    - 向左**选择一个页面**列表中选择**数据库设置**。

    - 验证和/或将**数据库的默认位置**更改为您选择的**数据磁盘**位置。 这是如果用默认位置设置创建新的数据库驻留的位置。

        ![SQL 数据库默认值][15]  

5. 若要创建新的数据库和一组包含分区的表的文件组，打开示例脚本**创建\_db\_default.sql**。 该脚本将创建新的数据库名为**TaxiNYC**和 12 文件组是默认的数据位置。 每个文件组将包含一个月的行程\_数据和行程\_不乏其数据。 如果需要，请修改数据库名。 单击**！执行**以运行该脚本。

6. 接下来，创建两个分区表，一个用于旅游\_数据，另一个为旅行\_票费。 打开示例脚本**创建\_分区\_table.sql**，它将︰

    - 创建分区函数将数据拆分按月。
    - 创建分区方案每个月的数据映射到不同的文件组。
    - 创建两个分区的表映射到分区方案︰ **nyctaxi\_行程**将保留行程\_数据和**nyctaxi\_票费**将保留行程\_不乏其数据。

    单击**！执行**以运行该脚本并创建已分区的表。

7. 在**示例脚本**文件夹中，有两个示例 PowerShell 脚本提供以演示并行的批量导入到 SQL Server 表的数据。

    - **bcp\_并行\_generic.ps1**是一个通用脚本到并行的批量导入数据到表中。 修改此脚本，在脚本的注释行中所述设置输入和目标变量。
    - **bcp\_并行\_nyctaxi.ps1**是通用脚本的预先配置的版本，可用于为加载这两个表的纽约出租车上行程数据。  

8. 用鼠标右键单击**bcp\_并行\_nyctaxi.ps1**脚本名称并单击**编辑**以打开它在 PowerShell。 查看预设的变量并修改根据您选定的数据库名称、 输入的数据文件夹、 目标日志文件夹、 和路径的示例格式文件**nyctaxi_trip.xml**和**nyctaxi\_fare.xml** （提供的**示例脚本**文件夹中）。

    ![批量导入数据][16]

    您还可以选择身份验证模式下，默认设置是 Windows 身份验证。 单击绿色箭头在工具栏中运行。 该脚本将启动如何在并行，12 24 批量导入操作并将其保存为每个分区表。 您可以通过设置上面打开 SQL Server 默认数据文件夹监视数据导入进度。

9. PowerShell 脚本报告的开始时间和结束时间。 当所有批量导入完成时，报告的结束时间。 检查目标日志文件夹以验证的批量导入成功，亦即在目标日志文件夹报告任何错误。

10. 您的数据库是现在准备探索、 功能工程和其他所需的操作。 因为根据分区表**拾取\_日期时间**字段，其中包含查询**拾取\_日期时间** **WHERE**子句中的条件将该分区方案中受益。

11. 在**SQL Server 管理 Studio**，研究提供的示例脚本**示例\_queries.sql**。 要运行任何查询示例，突出显示的查询的行，然后单击**！执行**工具栏中。

12. 纽约出租车上行程数据加载在两个独立的表中。 若要提高连接操作，它强烈建议要编制索引的表。 示例脚本**创建\_分区\_index.sql**复合联接键上创建已分区的索引**medallion，攻击\_许可证和拾取\_日期时间**。

## <a name="dbexplore"></a>数据研究和功能在 SQL Server 中的工程

在本节中，我们将通过数据探索和功能生成直接在**SQL Server 管理 Studio**使用前面创建的 SQL Server 数据库中运行 SQL 查询。 一个示例脚本，名为**的示例\_queries.sql**提供的**示例脚本**文件夹中。 修改脚本来更改数据库的名称，如果不采用默认值︰ **TaxiNYC**。

在本练习中，我们将︰

- 连接到**SQL Server 管理 Studio**使用任一 Windows 身份验证或使用 SQL 身份验证的 SQL 登录名和密码。
- 探讨在不同的时间窗口中的几个字段的数据分布。
- 调查数据质量的经度和纬度的字段。
- 生成基于二进制和 multiclass 分类标签**提示\_量**。
- 生成功能和计算/比较旅行距离。
- 联接两个表，并提取可用于构建模型的随机样本。

准备就绪可以继续转到 Azure 机器学习后，可能是︰  

1. 保存最终的 SQL 查询，以提取和数据采样并直接插入[导入的数据]复制和粘贴查询[import-data]模块在 Azure 机器学习，或
2. 保持您打算使用模型生成新的数据库表中，并使用在[导入数据]的新表的采样和工程数据[import-data]Azure 机器学习中的模块。

在本节中我们将保存最终查询来提取和数据采样。 第二种方法是部分所示[数据研究和工程中 IPython 笔记本上的功能](#ipnb)。

在前面使用并行批量导入，填充的表行和列数的快速验证

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>通过 medallion 勘探︰ 行程分布

本示例用 medallion （taxi 数字） 标识在给定的时间段内 100 多个行程。 因为它可调的分区方案，查询将受益于分区的表访问**拾取\_日期时间**。 使用分区表和/或索引扫描，也会使查询的完整的数据集。

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>研究︰ 行程分发 medallion 和 hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>数据质量评估︰ 验证具有正确的经度和纬度的/或记录

此示例调查如果任一经度和纬度的/或字段包含无效值 （弧度度应该是 90-90 之间），或具有 （0，0） 的坐标。

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>研究︰ Tipped 与不附属式行程分布

本示例查找与不附属式在给定的时间段 （或如果覆盖全年的完整数据集） 所附属的行程次数。 这种分布反映了二进制的标签分发以后要用于二进制分类建模。

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>研究︰ 提示类/区域分布

本示例计算在给定的时间段内 （或如果覆盖全年的完整数据集） 的提示区域分布。 这是 multiclass 分类建模将稍后使用的标签类的分布情况。

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>研究︰ 计算和比较行程距离

本示例将挑选和放置经度并到 SQL 地理纬度点、 计算使用 SQL 地理点差异的旅行距离和返回的结果进行比较的随机样本。 该示例结果限制为仅使用前面介绍的数据质量评估查询的有效坐标。

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>在 SQL 查询中的功能工程

标签生成和地理位置转换勘探查询还可用于盘点的部分生成标签中的功能。 [数据研究和工程中 IPython 笔记本功能](#ipnb)一节中提供了附加功能工程 SQL 示例。 它是完整的数据集或使用直接在 SQL Server 数据库实例运行的 SQL 查询的大型子集上运行该功能生成查询效率更高。 可能在**SQL Server 管理 Studio**，IPython 笔记本或任何开发工具/环境的本地或远程方式访问数据库中执行查询。

#### <a name="preparing-data-for-model-building"></a>准备数据模型生成

下面的查询联接**nyctaxi\_行程**和**nyctaxi\_票费**表、 生成二进制分类标签**附属式**，多类分类标签**提示\_类**，并从完全联接的数据集中提取 1%的随机样本。 可以复制该查询，然后将其粘贴到[Azure 机器学习 Studio](https://studio.azureml.net) [导入数据]中直接[import-data]从 Azure 中的 SQL Server 数据库实例的直接数据接收模块。 查询将仅在具有不正确的记录 （0，0） 的坐标。

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>数据研究和功能工程 IPython 笔记本中

在本节中，我们将执行数据探索和使用 Python 和 SQL 查询前面创建的 SQL Server 数据库的功能生成。 一个名为**machine-Learning-data-science-process-sql-story.ipynb**的示例 IPython 笔记本的**示例 IPython 笔记本**文件夹中提供。 此笔记本也是在[GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)上可用。

在处理大数据时建议的顺序是如下︰

- 在小样本的数据读入内存中的数据帧。
- 执行某些可视化效果和使用的样本的数据的研究。
- 试验使用的样本的数据的功能工程。
- 对于较大的数据研究、 数据操作和功能的工程，使用 Python 发出直接对 Azure VM 中的 SQL Server 数据库的 SQL 查询。
- 决定使用 Azure 机器学习模型生成的样本大小。

当准备好继续到 Azure 机器学习时，可能是︰  

1. 保存最终的 SQL 查询，以提取和数据采样并直接插入[导入的数据]复制和粘贴查询[import-data]Azure 机器学习中的模块。 此方法是部分所示[Azure 机器学习中的建筑模型](#mlmodel)。    
2. 保持您计划使用模型生成新的数据库表中的样本和工程数据，然后使用在[导入数据]的新表[import-data]模块。

以下是几个数据研究、 数据可视化和工程示例的功能。 有关更多示例，请参阅中的**示例 IPython 笔记本**文件夹的示例 SQL IPython 笔记本。

#### <a name="initialize-database-credentials"></a>初始化数据库凭据

初始化数据库连接设置以下变量中︰

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>创建数据库连接

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>报表编号的行和列在表 nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- 总行数 = 173179759  
- 总列数 = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>读在从 SQL Server 数据库的较小的数据示例

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

时间阅读示例表是 6.492000 秒  
检索的行和列数 = 84952 (21）

#### <a name="descriptive-statistics"></a>描述性统计信息

现在就可以浏览样本的数据。 我们从看描述性统计开始**行程\_距离**（或任何其他） 个字段︰

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>可视化︰ 框绘图示例

接下来我们看一下行程距离，可以直观地显示 quantiles 盒须图

    df1.boxplot(column='trip_distance',return_type='dict')

![绘制 #1][1]

#### <a name="visualization-distribution-plot-example"></a>可视化︰ 分布图示例

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![绘制 #2][2]

#### <a name="visualization-bar-and-line-plots"></a>可视化︰ 条形图和线图

在此示例中，我们丢弃到五个纸盒的旅行距离和可视化 binning 结果。

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

我们可以绘制上面 bin 分布条或线图如下所示

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![绘制 #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![绘制 #4][4]

#### <a name="visualization-scatterplot-example"></a>可视化︰ Scatterplot 示例

我们显示之间的散点图**行程\_时间\_在\_秒**和**行程\_距离**来查看是否有任何关联

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![绘制 #6][6]

同样，我们可以检测之间的关系**速率\_代码**和**行程\_距离**。

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![绘制 #8][8]

### <a name="sub-sampling-the-data-in-sql"></a>欠采样 SQL 中的数据

准备数据模型构建在[Azure 机器学习 Studio](https://studio.azureml.net)时, 可能决定**直接导入数据模块中使用 SQL 查询**或保持在新表中，您可以使用[导入数据]中的工程和取样数据[import-data]模块，用一个简单的**选择*从 < 您\_新\_表\_名称 >**。

在本节中，我们将创建一个新表来保存样本和工程数据。 [数据研究和工程中 SQL Server 功能](#dbexplore)部分提供了直接的模型生成 SQL 查询的一个示例。

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>创建示例表并填充为联接的表的 1%。 如果存在，则删除表第一。

在本节中，我们加入表**nyctaxi\_行程**和**nyctaxi\_票费**，提取 1%的随机样本，以及保存新表的名称中的采样的数据**nyctaxi\_一\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>数据开采 IPython 笔记本中使用 SQL 查询

在本节中，我们将探讨使用我们上面创建的新表中保留的 1%抽样数据的数据分布。 请注意，可以使用原始的表，还可以使用**TABLESAMPLE**来限制勘探样品或通过将结果限制为一个给定的时间期间使用执行类似成果**拾取\_日期时间**分区，如本文所述的[数据探索和在 SQL Server 中工程功能](#dbexplore)部分。

#### <a name="exploration-daily-distribution-of-trips"></a>研究︰ 每日分发的行程

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>每 medallion 勘探︰ 行程分布

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>在 IPython 笔记本中使用 SQL 查询的功能生成

在本节中，我们将生成新的标签和直接使用 SQL 查询功能，操作上 1%的示例表我们创建上一节中。

#### <a name="label-generation-generate-class-labels"></a>标签生成︰ 生成类标签

在以下示例中，我们生成标签用于建模的两的组︰

1. 二进制类标签**附属式**（预测是否将给出一个提示）
2. 标签 multiclass**提示\_类**（预测提示 bin 或范围）

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>对于分类列功能工程︰ 计数功能

本示例转换的分类字段数值字段替换其事件数据中的计数为每个类别。

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>对于数字列的功能工程︰ Bin 功能

本示例将连续数值字段转换为预设的类别范围，即转换成一个分类字段数值字段。

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>功能的工程︰ 从十进制的纬度/经度提取位置的功能

本示例可划分的十进制表示形式纬度和/或经度字段的不同粒度，多个地区字段如，国家/地区、 城市、 城镇、 块等。请注意新的 geo 字段未映射到实际位置。 有关映射 geocode 位置的信息，请参阅[Bing 地图 REST 服务](https://msdn.microsoft.com/library/ff701710.aspx)。

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>验证 featurized 表的最终形式

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

现在我们可以继续与模型构建和[Azure 机器学习](https://studio.azureml.net)中的模型部署。 数据可用于任何一种预测问题即之前，确定︰

1. 二元分类︰ 预测是否提示已支付的旅行。

2. Multiclass 分类︰ 预测提示支付根据先前定义的类的范围。

3. 回归分析任务︰ 预测的提示支付的旅行量。  


## <a name="mlmodel"></a>在 Azure 机器学习中生成模型

要开始建模练习，请登录到 Azure 机器学习区。 如果尚未创建机器学习工作区，请参阅[创建 Azure 机器学习的工作区](machine-learning-create-workspace.md)。

1. 若要开始使用 Azure 机器学习，请参阅[Azure 机器学习 Studio 是什么？](machine-learning-what-is-ml-studio.md)

2. 登录到[Azure 机器学习 Studio](https://studio.azureml.net)。

3. Studio 主页提供丰富的信息、 视频、 教程、 链接到该模块引用和其他资源。 有关 Azure 机器学习有关的详细信息，请参阅[Azure 机器学习文档中心](https://azure.microsoft.com/documentation/services/machine-learning/)。

在典型培训实验由以下内容组成︰

1. 创建**+ 新**实验。
2. 获取到 Azure 机器学习数据。
3. 预先处理，将转换并根据需要操作数据。
4. 根据需要生成功能。
5. 将数据拆分为培训/验证/测试数据集 (或具有不同的数据集对于每个)。
6. 选择一个或多个机器学习算法根据要解决学习问题。 例如，二进制分类、 multiclass 的分类、 回归。
7. 使用练习数据集的一个或多个模型定型。
8. 分数使用训练有素的模型验证数据集。
9. 计算模型来计算学习问题的相关指标。
10. 正常调整模型，然后选择要部署的最佳模型。

在本练习中，已经研究和工程在 SQL Server 中的数据，我们决定在样本大小在 Azure 机器学习中摄取。 若要生成一个或多个预测模型我们决定︰

1. 获取到 Azure 使用[导入数据]的机器学习数据[import-data]模块，在**数据输入和输出**部分。 有关详细信息，请参阅[导入数据][import-data]模块引用页。

    ![Azure 机器学习导入数据][17]

2. 选择**属性**面板中的**数据源**的**SQL Azure 数据库**。

3. 在**数据库服务器名称**字段中输入数据库的 DNS 名称。 格式︰`tcp:<your_virtual_machine_DNS_name>,1433`

4. 在相应字段中输入**数据库名称**。

5. 输入中的**SQL 用户名****服务器 aqccount 用户名和密码在**服务器用户帐户密码 * *。

6. 选中**接受任何服务器证书**选项。

7. 在**数据库查询**编辑文本区域中，粘贴查询提取所需的数据库域 （包括任何计算的字段，如标签） 和向下到所需的样本大小对数据进行取样。

在 SQL Server 数据库中直接读取数据的二元分类实验的一个示例是在下图中。 类似的实验可以构造 multiclass 分类和回归问题。

![Azure 机器学习训练][10]

> [AZURE.IMPORTANT] 在建模数据的提取和抽样查询示例提供前面各节中，**在三个建模练习所有标签都包含在查询**中。 每个建模练习重要 （必选） 步到**排除**是其他两个问题，和任何其他的**目标泄漏**不必要的标签。 对于例如，当使用二元分类，使用**附属**标签并排除域**提示\_类**，**提示\_量**，和**总计\_量**。 后者是目标泄漏因为这两者提示付款。
>
> 为了排除不必要的列和/或目标泄漏，您可以使用[数据集中选择列][select-columns]模块或[编辑元数据][edit-metadata]。 有关详细信息，请参阅[选择在数据集内的列][select-columns]和[编辑元数据][edit-metadata]参考页。

## <a name="mldeploy"></a>部署在 Azure 机器学习模型

模型准备就绪后，可以轻松地部署为 web 服务直接从实验。 有关部署 Azure 机器学习 web 服务的详细信息，请参阅[部署 Azure 机器学习的 web 服务](machine-learning-publish-a-machine-learning-web-service.md)。

若要部署新的 web 服务，您需要︰

1. 创建记分的实验。
2. 部署 web 服务。

若要创建**已完成**培训实验计分的实验，较低的操作栏中单击**创建评分进行试验**。

![Azure 评分][18]

Azure 机器学习将尝试创建根据培训实验的组件在计分实验。 特别是，它将︰

1. 保存培训的模型和删除模型的培训模块。
2. 确定逻辑**输入的端口**来表示期望输入的数据架构。
3. 确定逻辑**输出端口**来表示预期的 web 服务输出架构。

当创建记分的实验时，审阅并根据需要调整。 替换为输入数据集和/或查询不包括标签字段，因为这些不能调用服务时是典型的调整。 它也是一个好的习惯，可减小到几个记录，输入数据集和/或查询的足够以指示输入的架构。 输出端口，常见的是排除所有输入的字段，并只在输出中使用所[选择的列在数据集中]包括**的得分概率**和**计分标签**[select-columns]模块。

在下图中是评分实验样本。 当准备好部署时，请单击较低的操作栏中的**发布 WEB 服务**按钮。

![发布 azure 的机器学习][11]

来回顾一下，在演练中本教程中，您创建了 Azure 数据科学环境中，大的公用数据集从数据采集培训和部署 web 服务的 Azure 机器学习模型与合作。

### <a name="license-information"></a>许可证信息

本示例演练和其附带的脚本和 IPython notebook(s) 由 Microsoft 共享的 MIT 许可证。 请签入 LICENSE.txt 文件在 GitHub 上的更多详细信息的示例代码的目录中。

### <a name="references"></a>引用

• [Andrés Monroy 纽约出租车上行程下载页面](http://www.andresmh.com/nyctaxitrips/)  
•[阻止 Chris Whong 通过纽约的出租车旅行数据](http://chriswhong.com/open-data/foil_nyc_taxi/)   
•[纽约出租车和 Limousine 委员会研究和统计](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
