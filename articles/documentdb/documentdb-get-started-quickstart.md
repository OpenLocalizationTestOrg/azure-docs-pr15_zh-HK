<properties
    pageTitle="NoSQL 教程︰ DocumentDB.NET SDK |Microsoft Azure"
    description="创建联机数据库和 C# 控制台应用程序使用 DocumentDB.NET SDK NoSQL 教程。 DocumentDB 是用于 JSON 使用 NoSQL 数据库。"
    keywords="nosql 教程、 联机数据库，c# 控制台应用程序"
    services="documentdb"
    documentationCenter=".net"
    authors="AndrewHoh"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/01/2016"
    ms.author="anhoh"/>

# <a name="nosql-tutorial-build-a-documentdb-c-console-application"></a>NoSQL 教程︰ 生成 DocumentDB C# 控制台应用程序

> [AZURE.SELECTOR]
- [.NET](documentdb-get-started.md)
- [Node.js](documentdb-nodejs-get-started.md)

欢迎使用 NoSQL 教程的 Azure DocumentDB.NET sdk ！ 后获得快速启动项目或完成本教程，您必须创建一个控制台应用程序和查询 DocumentDB 资源。

- **[快速入门](#quickstart)**︰ 下载的样例项目，添加连接信息，以及有 DocumentDB 的应用程序运行在 10 分钟之内。
- **[教程](#tutorial)**︰ 在 30 分钟内生成从零开始快速入门应用程序。

## <a name="prerequisites"></a>系统必备组件

- 活动的 Azure 帐户。 如果您没有，您可以注册[免费的帐户](https://azure.microsoft.com/free/)。
- [Visual Studio 2013年或 Visual Studio 2015年](http://www.visualstudio.com/)。
- .NET Framework 4.6

## <a name="quickstart"></a>快速入门

1. 从[GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started-quickstart/archive/master.zip)下载示例项目.zip 或克隆[documentdb-最低的入门-开始-快速入门](https://github.com/Azure-Samples/documentdb-dotnet-getting-started-quickstart)repo。
2. 使用 Azure 门户创建[一个 DocumentDB 帐户](documentdb-create-account.md)。
3. 在 App.config 文件中，将 EndpointUri 和字段名值替换从[Azure 的门户网站](https://portal.azure.com/)，通过导航到**DocumentDB (NoSQL)**刀片式服务器，然后单击**帐户名**，然后单击菜单上的资源的**键**的值。
    ![在 App.config 中替换的 EndpointUri 和字段名值的屏幕抓图](./media/documentdb-get-started-quickstart/nosql-tutorial-documentdb-keys.png)
4. 生成项目。 控制台窗口中显示新的资源被创建、 查询，然后清理。
    
    ![控制台输出的屏幕抓图](./media/documentdb-get-started-quickstart/nosql-tutorial-documentdb-console-output.png)

## <a id="tutorial"></a>教程

本教程将引导您完成创建的 DocumentDB 数据库，DocumentDB 集合，并且 JSON 文档。 然后将查询的集合，并清理和删除数据库。 本教程将生成同一个项目为快速启动项目，但将以增量方式生成它，将会收到有关您要添加到项目中的代码说明。

## <a name="step-1-create-a-documentdb-account"></a>步骤 1︰ 创建一个 DocumentDB 帐户

让我们创建一个 DocumentDB 帐户。 如果您已经有您想要使用的帐户，您可以跳至[安装 Visual Studio 解决方案](#SetupVS)。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="SetupVS"></a>步骤 2︰ 设置 Visual Studio 解决方案

1. 在您的计算机上打开**Visual Studio 2015年**。
2. 在**文件**菜单上选择**新建**，然后选择**项目**。
3. 在**新建项目**对话框中，选择**模板** / **C#** / **控制台应用程序**，项目，命名，然后再单击**确定**。
![新建项目窗口的屏幕抓图](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)
4. 在**解决方案资源管理器**中右键单击新控制台应用程序，即在 Visual Studio 解决方案。
5. 再无需离开菜单上，单击上**管理 NuGet 程序包...**
![的屏幕抓图右 Clicked 菜单项目](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. **Nuget**选项卡中，单击**浏览**并在搜索框中键入**azure documentdb** 。
7. 在结果中，发现**Microsoft.Azure.DocumentDB** ，然后单击**安装**。
DocumentDB 客户端库的包 ID 是[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)
![的屏幕抓图 Nuget 菜单查找 DocumentDB 客户端 SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

很好 ！ 既然我们完成安装程序，让我们开始编写一些代码。 您可以在[GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs)找到完整的代码项目的本教程。

## <a id="Connect"></a>第 3 步︰ 连接到一个 DocumentDB 帐户

首先，在 Program.cs 文件中添加的 C# 应用程序，开始这些引用︰

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [AZURE.IMPORTANT] 若要完成本教程中 NoSQL，确保添加上面的依赖关系。

现在，添加这两个常量和公用类*程序*下的*客户端*变量。

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUri = "<your endpoint URI>";
        private const string PrimaryKey = "<your key>";
        private DocumentClient client;

下一步，到[Azure 门户网站](https://portal.azure.com)以检索您的 URI 和主键的头。 DocumentDB 的 URI 和主键都有必要了解在何处可以连接到您的应用程序和 DocumentDB 信任应用程序的连接。

在 Azure 门户中，导航到您的 DocumentDB 帐户，然后单击**项**。

从门户复制 URI，并将其粘贴到`<your endpoint URI>`program.cs 文件中。 从门户复制主键，然后将其粘贴到`<your key>`。

![Azure 门户使用 NoSQL 教程创建一个 C# 控制台应用程序的屏幕抓图。 用突出显示活动中心、 键按钮突出显示在 DocumentDB 帐户刀片式服务器，并突出显示键刀片式服务器上的 URI、 为主键和辅助键值显示的 DocumentDB 帐户，][keys]

我们将通过创建新实例的**DocumentClient**开始获取启动应用程序。

下面**Main**方法中，添加名为**GetStartedDemo**，它将实例化我们新**DocumentClient**这个新的异步任务。

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
    }

添加以下代码，以从**Main**方法运行异步任务。 **Main**方法将捕捉异常并将它们写到控制台。

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
            try
            {
                    Program p = new Program();
                    p.GetStartedDemo().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

按**F5**以运行应用程序。

祝贺您 ！ 您已成功连接到一个 DocumentDB 帐户，让我们现在看看如何使用 DocumentDB 资源。  

## <a name="step-4-create-a-database"></a>步骤 4︰ 创建数据库
添加用于创建数据库的代码之前，添加帮助器方法将写入到控制台。

复制并粘贴下面的**GetStartedDemo**方法的**WriteToConsoleAndPromptToContinue**方法。

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

可以通过使用[CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx)的**DocumentClient**类的方法创建 DocumentDB[数据库](documentdb-resources.md#databases)。 数据库是跨集合分区的 JSON 文档存储的逻辑容器。

复制并粘贴下面的**WriteToConsoleAndPromptToContinue**方法的**CreateDatabaseIfNotExists**方法。

    // ADD THIS PART TO YOUR CODE
    private async Task CreateDatabaseIfNotExists(string databaseName)
    {
            // Check to verify a database with the id=FamilyDB does not exist
            try
            {
                    await this.client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(databaseName));
                    this.WriteToConsoleAndPromptToContinue("Found {0}", databaseName);
            }
            catch (DocumentClientException de)
            {
                    // If the database does not exist, create a new database
                    if (de.StatusCode == HttpStatusCode.NotFound)
                    {
                            await this.client.CreateDatabaseAsync(new Database { Id = databaseName });
                            this.WriteToConsoleAndPromptToContinue("Created {0}", databaseName);
                    }
                    else
                    {
                            throw;
                    }
            }
    }

复制并粘贴以下代码到您在客户端创建下面的**GetStartedDemo**方法。 这将创建名为*FamilyDB*的数据库。

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseIfNotExists("FamilyDB_va");

按**F5**以运行应用程序。

祝贺您 ！ 您已成功创建的 DocumentDB 数据库。  

## <a id="CreateColl"></a>步骤 5︰ 创建集合  

> [AZURE.WARNING] **CreateDocumentCollectionAsync**将创建一个新集合，以保留已定价的含义的吞吐量。 有关详细信息，请访问我们的[定价页](https://azure.microsoft.com/pricing/details/documentdb/)。

可以通过使用**DocumentClient**类的[CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx)方法创建一个[集合](documentdb-resources.md#collections)。 集合是 JSON 文档和关联的 JavaScript 应用程序逻辑的容器。

复制并粘贴您的**CreateDatabaseIfNotExists**方法下的**CreateDocumentCollectionIfNotExists**方法。

    // ADD THIS PART TO YOUR CODE
    private async Task CreateDocumentCollectionIfNotExists(string databaseName, string collectionName)
    {
        try
        {
            await this.client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName));
            this.WriteToConsoleAndPromptToContinue("Found {0}", collectionName);
        }
        catch (DocumentClientException de)
        {
            // If the document collection does not exist, create a new collection
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                DocumentCollection collectionInfo = new DocumentCollection();
                collectionInfo.Id = collectionName;

                // Configure collections for maximum query flexibility including string range queries.
                collectionInfo.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

                // Here we create a collection with 400 RU/s.
                await this.client.CreateDocumentCollectionAsync(
                    UriFactory.CreateDatabaseUri(databaseName),
                    collectionInfo,
                    new RequestOptions { OfferThroughput = 400 });

                this.WriteToConsoleAndPromptToContinue("Created {0}", collectionName);
            }
            else
            {
                throw;
            }
        }
    }

复制并粘贴以下代码到您数据库创建下面的**GetStartedDemo**方法。 这将创建一个名为*FamilyCollection_va*的文档集合。

        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        await this.CreateDatabaseIfNotExists("FamilyDB_oa");

        // ADD THIS PART TO YOUR CODE
        await this.CreateDocumentCollectionIfNotExists("FamilyDB_va", "FamilyCollection_va");

按**F5**以运行应用程序。

祝贺您 ！ 您已成功创建 DocumentDB 文档集合。  

## <a id="CreateDoc"></a>第 6 步︰ 创建 JSON 文档
通过使用[CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx)方法的**DocumentClient**类，可以创建[文档](documentdb-resources.md#documents)。 文档是用户定义的 （任意） JSON 内容。 我们现在可以插入一个或多个文档。 如果您已经有了想要在数据库中存储的数据，您可以使用 DocumentDB 的[数据迁移工具](documentdb-import-data.md)。

首先，我们需要创建一个**家族**类表示存储在 DocumentDB 此示例中的对象。 我们还将创建**父**、**子**、**宠物**、 在**家庭**中使用的**地址**子类。 请注意，文档必须具有**Id**属性序列化为 json 格式的**id** 。 通过**GetStartedDemo**方法后添加以下内部子类创建这些类。

复制并粘贴**系列**、**父**、**子**、**宠物**和**地址**类别下的**WriteToConsoleAndPromptToContinue**方法。

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
                return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

复制并粘贴您的**CreateDocumentCollectionIfNotExists**方法下的**CreateFamilyDocumentIfNotExists**方法。

    // ADD THIS PART TO YOUR CODE
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
            }
            else
            {
                throw;
            }
        }
    }

然后将两个文档，分别用于莹家族和 Wakefield 的家人。

复制并粘贴以下代码到您创建文档集下的**GetStartedDemo**方法。

    await this.CreateDatabaseIfNotExists("FamilyDB_va");

    await this.CreateDocumentCollectionIfNotExists("FamilyDB_va", "FamilyCollection_va");

    // ADD THIS PART TO YOUR CODE
    Family andersenFamily = new Family
    {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                    new Parent { FirstName = "Thomas" },
                    new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FirstName = "Henriette Thaulow",
                            Gender = "female",
                            Grade = 5,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Fluffy" }
                            }
                    }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_va", "FamilyCollection_va", andersenFamily);

    Family wakefieldFamily = new Family
    {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                    new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                    new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FamilyName = "Merriam",
                            FirstName = "Jesse",
                            Gender = "female",
                            Grade = 8,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Goofy" },
                                    new Pet { GivenName = "Shadow" }
                            }
                    },
                    new Child
                    {
                            FamilyName = "Miller",
                            FirstName = "Lisa",
                            Gender = "female",
                            Grade = 1
                    }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_va", "FamilyCollection_va", wakefieldFamily);

按**F5**以运行应用程序。

祝贺您 ！ 您已成功创建两个 DocumentDB 文档。  

![用图表演示帐户、 在线数据库、 集合和使用 NoSQL 教程创建一个 C# 控制台应用程序的文档之间的层次关系](./media/documentdb-get-started/nosql-tutorial-account-database.png)

##<a id="Query"></a>第 7 步︰ 查询 DocumentDB 资源

DocumentDB 支持丰富[查询](documentdb-sql-query.md)存储在每个集合中的 JSON 文档。  下面的代码示例显示了各种查询-使用两种 DocumentDB SQL 语法上一步中，我们可以对运行文档我们插入以及 LINQ 的。

复制并粘贴您的**CreateFamilyDocumentIfNotExists**方法下的**ExecuteSimpleQuery**方法。

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

复制并粘贴以下代码到您下第二个文档创建的**GetStartedDemo**方法。

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_va", "FamilyCollection_va", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB_va", "FamilyCollection_va");

按**F5**以运行应用程序。

祝贺您 ！ 您已成功查询的 DocumentDB 集合对象。

下图演示对 LINQ 查询以及如何应用 DocumentDB SQL 查询语法调用针对集合的创建和相同的逻辑。

![图说明范围和意义 NoSQL 教程用于创建 C# 控制台应用程序的查询](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

[FROM](documentdb-sql-query.md#from-clause)关键字是可选的查询，因为 DocumentDB 查询已作用于单个集合。 因此，"从家族 f"可以使用更换"从根 r"或任何其他变量命名为您选择。 DocumentDB 将推断该系列、 根或您选择的变量名称，默认情况下引用的当前集合。

##<a id="ReplaceDocument"></a>步骤 8︰ 更换 JSON 文档

DocumentDB 支持替换的 JSON 文档。  

复制并粘贴您的**ExecuteSimpleQuery**方法下的**ReplaceFamilyDocument**方法。

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
        try
        {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
            this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }

复制并粘贴以下代码到您的**GetStartedDemo**方法，下面的查询执行。 之后替换文档，这将运行相同的查询，再次以查看更改的文档。

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_va", "FamilyCollection_va", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB_va", "FamilyCollection_va");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB_va", "FamilyCollection_va", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB_va", "FamilyCollection_va");

按**F5**以运行应用程序。

祝贺您 ！ 您已经成功地取代了 DocumentDB 文档。

##<a id="DeleteDocument"></a>步骤 9︰ 删除 JSON 文档

DocumentDB 支持删除的 JSON 文档。  

复制并粘贴您的**ReplaceFamilyDocument**方法下的**DeleteFamilyDocument**方法。

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
        try
        {
            await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
            Console.WriteLine("Deleted Family {0}", documentName);
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }

复制并粘贴以下代码到您下第二个查询执行的**GetStartedDemo**方法。

    await this.ReplaceFamilyDocument("FamilyDB_va", "FamilyCollection_va", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB_va", "FamilyCollection_va");

    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB_va", "FamilyCollection_va", "Andersen.1");

按**F5**以运行应用程序。

祝贺您 ！ 已成功删除 DocumentDB 文档。

##<a id="DeleteDatabase"></a>第 10 步︰ 删除数据库

删除创建的数据库将删除数据库及其所有的子资源 （集合、 文档等）。

复制并粘贴以下代码到您文档下面的**GetStartedDemo**方法删除要删除整个数据库和所有儿童资源。

    this.ExecuteSimpleQuery("FamilyDB_va", "FamilyCollection_va");

    await this.DeleteFamilyDocument("FamilyDB_va", "FamilyCollection_va", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_va"));

按**F5**以运行应用程序。

祝贺您 ！ 已成功删除 DocumentDB 数据库。

##<a id="Run"></a>步骤 11︰ 运行 C# 控制台应用程序一起 ！

按 F5 在 Visual Studio 生成该应用程序在调试模式下。

您应看到您获取已启动的应用程序的输出。 输出将显示查询我们添加并应与下面的示例文本相匹配的结果。

    Created FamilyDB_va
    Press any key to continue ...
    Created FamilyCollection_va
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

祝贺您 ！ 您已经完成本 NoSQL 教程，有一个工作 C# 控制台应用程序 ！

## <a name="next-steps"></a>下一步行动

- 想要一个更复杂的 ASP.NET MVC NoSQL 教程？ 请参阅[生成和使用 DocumentDB 的 ASP.NET MVC web 应用程序](documentdb-dotnet-application.md)。
- 要执行的规模和性能测试与 DocumentDB？ 请参阅[性能和可扩展性测试使用 Azure DocumentDB](documentdb-performance-testing.md)
-   了解如何[监视 DocumentDB 帐户](documentdb-monitor-accounts.md)。
-   对[查询运动场](https://www.documentdb.com/sql/demo)在我们的示例数据集运行查询。
-   了解有关在[文档页中找到 DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/)的发展部分的编程模型。

[documentdb-create-account]: documentdb-create-account.md
[documentdb-manage]: documentdb-manage.md
[keys]: media/documentdb-get-started-quickstart/nosql-tutorial-keys.png

