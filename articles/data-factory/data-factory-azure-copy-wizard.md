<properties
    pageTitle="数据工厂 Azure 复制向导 |Microsoft Azure"
    description="了解有关如何使用数据工厂 Azure 复制向导支持的数据源的数据复制到接收器。"
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
    ms.date="10/03/2016"
    ms.author="spelluru"/>

# <a name="azure-data-factory---copy-wizard"></a>Azure 数据工厂-复制向导
Azure 数据工厂复制向导是为了简化接收数据，通常是在端到端数据集成方案的第一步的过程。 Azure 数据工厂复制向导时，您不需要了解任何 JSON 的定义，链接的服务、 数据集和管道。 但是，完成向导中的所有步骤后，向导将自动创建管线从所选的数据源的数据复制到所选目标盘。 此外，复制向导可帮助您验证数据正在 ingested 时的创作，从而节省了很多的时间，尤其当您正在接收数据第一次从数据源。 要开始复制向导，请单击数据工厂的主页上**复制数据**的图块。

![复制向导](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>直观的向导将数据复制
此向导允许您轻松地将数据从各种源与目标以分钟为单位。 在经过该向导，使用复制活动管线是自动为您创建以及相关数据工厂实体 （链接的服务和数据集）。 无需其他步骤需要创建管线。   

![选择数据源](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] 若要创建要复制一个样本管道的分步说明请参阅[复制向导教程](data-factory-copy-data-wizard-tutorial.md)文章从 Azure 数据 blob Azure SQL 数据库的表。 

在向导的设计从开始构思中的大数据。 它是简单而高效地创作移动数百个文件夹、 文件或使用向导中的数据复制的表的数据工厂管线。 该向导支持以下三种功能︰ 自动数据预览、 捕获架构和映射和筛选数据。 

## <a name="automatic-data-preview"></a>自动数据预览 
复制向导使您可以查看您可以验证是否是正确的数据要复制数据的选定的数据源中数据的一部分。 此外，如果源数据是在一个文本文件中，复制向导将分析自动了解行和列分隔符，以及架构的文本文件。 

![文件格式设置](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>架构捕获和映射 
输入的数据的架构可能不匹配在某些情况下的输出数据的架构。 在这种情况下，您需要来自源架构与目标架构中的列的列映射。 

复制向导自动将源架构中的列映射到目标架构中的列。 您可以通过使用下拉列表来替换映射 （或者） 指定某一列是否需要复制数据时跳过。   

![架构映射](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>筛选数据  
该向导允许您筛选来选择数据要复制到的目标/接收器数据存储所需的源数据。 筛选可缩减到接收器的数据存储区中复制的数据量，因此增强了复制操作的吞吐量。 通过使用 SQL 查询语言 （或） 文件在 Azure blob 文件夹中使用[数据工厂函数和变量](data-factory-functions-variables.md)，它提供一种筛选数据的关系数据库的灵活方式。   

### <a name="filtering-of-data-in-a-database"></a>在数据库中的数据的筛选  
在此示例中，使用 SQL 查询`Text.Format`函数和`WindowStart`变量。 

![验证表达式](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>筛选的 Azure blob 文件夹中的数据
可以在文件夹路径中使用变量来确定在运行时基于[系统变量](data-factory-functions-variables.md#data-factory-system-variables)文件夹中复制数据。 受支持的变量是: **{年}**、 **{月}**、 **{一天}**、 **{小时}**、 **{分钟}**和**{自定义}**。 例如︰ inputfolder / {年} / {月} 每天 {}。

假设您输入文件夹以下面的格式︰

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

对**文件或文件夹**中单击**浏览**按钮，浏览到一个文件夹 (例如，2016年-> 03-> 01-> 02)，并单击**选择**。 您应该会看到`2016/03/01/02`在文本框中。 现在，替换**2016年** **{年}**、 **{**月} **03** 、 与**{一天}**， **01**和**02** **{小时}**，并按 tab 键。 您应该看到下拉列表以选择这四个变量的格式︰

![使用系统变量](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

下面的屏幕快照中所示，您可以使用任何[支持格式字符串](https://msdn.microsoft.com/library/8kb3ddd4.aspx)和**自定义**变量。 若要选择与该结构的文件夹，请先使用**浏览**按钮。 然后用**{自定义}**替换一个值并按 Tab 来查看的文本框，您可以在其中键入格式字符串。     

![使用自定义变量](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>支持不同的数据和对象类型
通过使用复制向导，您可以高效地移动数以百计的文件夹、 文件或表。

![选择要从中复制数据的表](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>日程排定选项
（每小时、 每日等），可以一次或按计划运行的复制操作。 两个选项可以在内部部署、 云和本地桌面副本用于连接器的广度。

一次性拷贝操作一次只能允许从一个源到目标的数据移动。 它适用于任意大小和任何受支持的格式的数据。 计划的复制可以将有关规定定期拷贝。 可以使用格式设置 （如重试超时值，以及警报） 配置计划的复制。

![计划属性](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>下一步行动
有关使用数据工厂复制向导创建复制活动与管线的快速演练，请参见[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)。
