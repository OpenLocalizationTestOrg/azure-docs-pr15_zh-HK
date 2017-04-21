## <a name="repeatability-during-copy"></a>在复制期间可再现

当从其他数据数据复制到 Azure SQL/SQL Server 存储一个需要记住一点，以避免意外的成果可再现。 

将数据复制到 Azure SQL/SQL Server 数据库，复制活动将按默认值追加到接收器表的数据集，默认情况下使用。 例如，复制来源 CSV （逗号分隔值数据） 文件包含到 Azure SQL/SQL Server 数据库的两个记录，这是表如下所示︰
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

假设您在源代码文件中找到错误并更新的数量下管从 2 到源文件中的 4。 如果您重新运行段的数据切片，您可以找到两个新的记录追加到 Azure SQL/SQL Server 数据库。 下面假定上述表中的列具有主键约束。
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

若要避免此问题，您将需要利用一种指定 UPSERT 的语义如下所述的 2 架下面。

> [AZURE.NOTE] 一块可以重新自动运行在 Azure 数据工厂按照指定的重试策略。

### <a name="mechanism-1"></a>1 的机制

您可以利用**sqlWriterCleanupScript**属性运行切片时第一次执行清理操作。 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

清理脚本就会执行第一块将从该片与对应的 SQL 表中删除数据的给定的复制期间。 该活动将随后将数据插入到 SQL 表。 

如果现在重新运行，则您将会发现作为更新的数量是切片所需。
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

假设垫圈记录已删除从原始 csv。 然后重新运行该扇区，将产生以下结果︰ 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

新鲜，必须完成。 复制活动已运行的清理脚本删除该片的相应数据。 然后输入读取 csv （其中则包含只有 1 记录） 并将其插入到表中。 

### <a name="mechanism-2"></a>2 架
> [AZURE.IMPORTANT] sliceIdentifierColumnName 不支持 Azure SQL 数据仓库这一次。 

通过在目标表中有一个专用的列 (**sliceIdentifierColumnName**) 是另一种机制，以实现可再现。 此列将 Azure 数据工厂，用于确保源和目标保持同步。 这种方法工作时更改或定义目标 SQL 表架构的灵活性。 

此列将使用 Azure 数据工厂实现可再现的目的和过程中 Azure 数据工厂不会进行任何架构更改的表。 使用这种方法的方法︰

1.  定义目标 SQL 表中的列的二进制类型 (32)。 应该在此列上的没有限制。 让我们为了使本示例命名为 ColumnForADFuseOnly 的此列。
2.  在中使用它的复制活动，如下所示︰

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure 数据工厂将会填充此列按照其需要确保源和目标保持同步。 此列的值不应在此上下文的外部用户使用。 

类似于机制 1，复制活动将自动首先清除目标 SQL 表中给定的扇区的数据，然后运行通常要将数据源插入到该片的目标的复制活动。 
