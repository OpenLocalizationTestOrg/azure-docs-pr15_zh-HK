## <a name="repeatability-during-copy"></a>在复制期间可再现

数据复制和到关系存储，需要记住一点，以避免意外的成果可再现。 

一块可以重新自动在 Azure 数据工厂按照指定的重试策略。 我们建议您设置以防止瞬间失败重试策略。 因此可再现是来处理在数据移动过程中的一个重要方面。 

**作为源︰**

> [AZURE.NOTE] 下面的示例 SQL Azure 的但它们适用于任何数据存储区支持矩形的数据集。 您可能需要调整源和**查询**属性的**类型**(例如︰ 查询而不是 sqlReaderQuery) 的数据存储。   

通常情况下时读取关系存储，您可能需要阅读只对应于该扇区的数据。 要做到这一点的方法就是通过在 Azure 数据工厂中使用的 WindowStart 和 WindowEnd 变量。 阅读有关的变量和函数在 Azure 数据工厂在[计划和执行](../articles/data-factory/data-factory-scheduling-and-execution.md)项目。 示例︰ 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

此查询从 MyTable 切片工期范围内读取数据。 重新运行此片的也始终将确保这种行为。 

在其他情况下，您可能希望阅读整个表 （假设一次仅移动） 可以定义 sqlReaderQuery，如下所示︰

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
