<properties 
   pageTitle="开发 Azure 数据湖分析作业的 U SQL 用户定义运算符 |Azure" 
   description="了解如何开发使用和重用数据湖分析作业中的用户定义运算符。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>开发 Azure 数据湖分析作业的 U SQL 用户定义运算符

了解如何开发使用和重用数据湖分析作业中的用户定义运算符。 您将开发一个自定义的运算符，以将国家/地区名称的转换。

##<a name="prerequisites"></a>系统必备组件

- Visual Studio 2015年，Visual Studio 2013年 4 或 Visual Studio 2012 使用更新 Visual C++ 安装 
- Microsoft Azure SDK 的.NET 版本 2.5 或更高。  安装使用 Web 平台安装程序。
- 一种数据湖分析帐户。  请参阅[开始使用 Azure 数据湖分析使用 Azure 门户](data-lake-analytics-get-started-portal.md)。
- 要通过[Azure 数据湖分析 U SQL Studio 入门](data-lake-analytics-u-sql-get-started.md)教程。
- 连接到 Azure，请参阅[开始使用 Azure 数据湖分析 U SQL Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure)。 
- 上载的源数据，请参阅[开始使用 Azure 数据湖分析 U SQL Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files)。 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>定义和 U SQL 中使用用户定义运算符

**若要创建和提交一个 U SQL 作业** 

1. 从**文件**菜单上，单击**新建**，然后单击**项目**。
2. 选择**U SQL 项目**类型。

    ![新 U SQL Visual Studio 项目](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. 单击**确定**。 与 Script.usql 文件，visual studio 将创建一个解决方案。
4. 从**解决方案资源管理器中**，展开 Script.usql，，然后双击**Script.usql.cs**。
5. 将以下代码粘贴到文件中︰

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. 打开 Script.usql，并粘贴下面的 U SQL 脚本︰

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. 从**解决方案资源管理器中**，右键单击**Script.usql**，然后单击**生成脚本**。
6. 从**解决方案资源管理器中**，右键单击**Script.usql**，，然后单击**提交脚本**。
7. 如果您没有连接到 Azure 订购，您将输入的 Azure 帐户凭据的提示。
7. 单击**提交**。 提交结果和作业链接可在结果窗口中完成提交。
8. 您必须单击刷新按钮以查看最新的作业状态并刷新屏幕。

**若要查看作业输出**

1. 从**服务器资源管理器中**，展开**Azure**，展开**数据湖分析**、 展开数据湖分析帐户、 展开**存储帐户**、 默认存储中，用鼠标右键单击，然后单击**资源管理器**。 
2. 展开样本、 展开输出，然后双击**Drivers.csv**。


##<a name="see-also"></a>请参见

- [有关数据使用 PowerShell 的湖泊分析入门](data-lake-analytics-get-started-powershell.md)
- [有关数据使用 Azure 门户的湖泊分析入门](data-lake-analytics-get-started-portal.md)
- [Visual Studio 开发 U SQL 应用程序使用数据湖工具](data-lake-analytics-data-lake-tools-get-started.md)
