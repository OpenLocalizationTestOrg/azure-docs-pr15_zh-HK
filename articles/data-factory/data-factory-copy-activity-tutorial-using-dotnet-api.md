<properties 
    pageTitle="教程︰ 创建使用.NET API 的复制活动与管道 |Microsoft Azure" 
    description="在本教程中，您创建 Azure 数据工厂管道与复制活动使用.NET API。" 
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
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>教程︰ 创建管线与使用.NET API 的复制活动
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [复制向导](data-factory-copy-data-wizard-tutorial.md)
- [Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure 的资源管理器模板](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API，](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


本教程演示如何创建并监控使用.NET API Azure 数据工厂。 数据工厂中的管道使用复制活动 Azure Blob 存储 SQL Azure 数据库之间复制数据。

复制活动在 Azure 数据工厂执行数据移动。 该活动是由全局可用的服务，可将各种数据存储安全、 可靠和可扩展的方式之间的数据复制的供电。 请参阅[数据移动活动](data-factory-data-movement-activities.md)文章复制活动有关的详细信息。   

> [AZURE.NOTE] 
> 本文未涵盖所有数据工厂.NET API。 数据工厂.NET SDK 有关的详细信息，请参阅[数据工厂.NET API 参考](https://msdn.microsoft.com/library/mt415893.aspx)。 

## <a name="prerequisites"></a>系统必备组件
- 经过[教程概述和必备](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)了解本教程的概况并完成的**先决条件**步骤。 
- Visual Studio 2012 或 2013年和 2015
- 下载并安装[Azure.NET SDK](http://azure.microsoft.com/downloads/)
- Azure PowerShell。 按照[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)文章中的说明进行操作，在您的计算机上安装 Azure PowerShell。 您可以使用 Azure PowerShell 创建 Azure Active Directory 应用程序。

### <a name="create-an-application-in-azure-active-directory"></a>在 Azure 活动目录中创建应用程序
创建 Azure Active Directory 应用程序、 创建服务主体的应用程序，并将其分配给**数据工厂参与者**角色。  

1. 启动**PowerShell**。 
1. 运行以下命令，并输入用户名和密码用于登录到 Azure 的门户。
    
        Login-AzureRmAccount   
2. 运行以下命令来查看该帐户的所有订阅。

        Get-AzureRmSubscription 
3. 运行下面的命令以选择想要使用的订阅。 更换**&lt;NameOfAzureSubscription** &gt; Azure 订阅的名称。 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] 此命令的输出中，请注意下**SubscriptionId**和**TenantId** 。 
4. 创建名为**ADFTutorialResourceGroup** ，通过运行以下命令继续 Azure 的资源。  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    如果资源组已经存在，则指定是否更新 (Y) 或使其成为 (N)。 

    如果您使用不同的资源组，您需要在本教程中使用 ADFTutorialResourceGroup 替代资源组的名称。
5. 创建 Azure 活动目录的应用程序。 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    如果出现以下错误，指定不同的 URL，并再次运行该命令。 

        Another object with the same value for property identifierUris already exists.

6. 创建 AD 服务主体。 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. 添加到**数据工厂参与者**角色服务主体。 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. 获取应用程序 id。

        $azureAdApplication

    记下应用程序 ID (从输出**applicationID** )。

您应该了解这些步骤从下列四个值︰ 

- 租户 ID
- 订购 ID
- 应用程序 ID 
- （在第一个命令中指定） 的密码   

## <a name="walkthrough"></a>演练
1. 使用 Visual Studio 2012/2013年/2015年，创建一个 C#.NET 控制台应用程序。
    1. 启动**Visual Studio** 2012/2013年/2015年。
    2. 单击**文件**，指向**新建**，单击**项目**。
    3. 展开**模板**，然后选择**C#**。 在本演练中，您使用 C#，但您可以使用任何.NET 语言。
    4. 从右侧的项目类型的列表中选择**控制台应用程序**。
    5. 对于名称中输入**DataFactoryAPITestApp** 。
    6. 位置选择**C:\ADFGetStarted** 。
    7. 单击**确定**以创建项目。
2. 单击**工具**，指向**Nuget 程序包管理器**，，单击**程序包管理器控制台**。
3.  在**程序包管理器控制台**，请执行以下步骤︰ 
    1.  运行以下命令以安装数据工厂包︰`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  运行以下命令以安装 Azure Active Directory 程序包 （不使用活动目录 API 代码中）︰`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. 在**App.config**文件中添加下面的**appSetttings**部分。 这些设置可使用的帮助器方法︰ **GetAuthorizationHeader**。 

    替换值**&lt;应用程序 ID&gt;**，**&lt;密码&gt;**，**&lt;订阅 ID&gt;**，和**&lt;租户 ID&gt;**与您自己的值。 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. 将以下**using**语句添加到项目中的源文件 (Program.cs)。

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. 添加下面的代码创建到**Main**方法中的**DataPipelineManagementClient**类的一个实例。 您可以使用此对象来创建数据工厂、 链接的服务、 输入和输出数据集和管线。 此外可以使用此对象来监视在运行时的数据集进行切片。    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > **ResourceGroupName**的值替换 Azure 的资源组的名称。 
    > 
    > 更新数据工厂 (**dataFactoryName**) 是唯一的名称。 数据工厂的名称必须是全局唯一的。 请参阅[数据工厂的命名规则](data-factory-naming-rules.md)命名规则对数据工厂项目的主题。 

7. 添加以下代码，创建一个到**Main**方法中的**数据工厂**。

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. 添加下面的代码创建**Azure 存储链接服务**到**Main**方法中。 

    > [AZURE.IMPORTANT] 名称和键的 Azure 存储帐户替换**storageaccountname**和**accountkey** 。 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. 添加下面的代码创建**SQL Azure 链接服务**到**Main**方法中。
 
    > [AZURE.IMPORTANT] SQL Azure 服务器、 数据库、 用户和密码的名称替换**服务器名称**、**数据库名称**、**用户名**和**密码**。  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. 添加下面的代码创建到**Main**方法的**输入和输出数据集**。 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. 添加下面的代码，**创建并激活管线**到**Main**方法中。 此管线已采用**BlobSource**作为源的**CopyActivity**和**BlobSink**作为一个接收器。

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. 将以下代码添加到**Main**方法来获得输出数据集的数据切片的状态。 没有只应在此示例中的切片。   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. 添加以下代码，以获取运行到**Main**方法中的数据切片的详细信息。

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. 添加下面的帮助器方法，由**程序**类的**Main**方法。  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


15. 在解决方案资源管理器中，展开项目 (**DataFactoryAPITestApp**)，右键单击**引用**，并单击**添加引用**。 选择"**System.Configuration**"的程序集的复选框，然后单击**确定**。 
16. 生成控制台应用程序。 单击**生成**菜单上，单击**生成解决方案**。 
16. 确认有至少一个文件在**adftutorial**容器在 Azure blob 存储中。 如果没有，请在记事本中创建**Emp.txt**文件，使用以下内容并将其上传到 adftutorial 容器。

        John, Doe
        Jane, Doe
     
17. 通过单击**调试**运行的示例 -> 菜单上的**启动调试**。 当您看到**获取运行数据切片的详细信息**时，请等待几分钟，然后按**ENTER**。 
18. 使用 Azure 的门户来验证数据工厂**APITutorialFactory**创建具有以下项目︰ 
    - 链接服务︰ **LinkedService_AzureStorage** 
    - 数据集︰ **DatasetBlobSource**和**DatasetBlobDestination**。
    - 管道︰ **PipelineBlobSample** 
18. 验证在"**emp**"表中指定的 SQL Azure 数据库创建了两个雇员记录。

## <a name="next-steps"></a>下一步行动

- 阅读[数据移动活动](data-factory-data-movement-activities.md)的文章中，它提供了您在本教程中使用复制活动的详细的信息。
- 数据工厂.NET SDK 有关的详细信息，请参阅[数据工厂.NET API 参考](https://msdn.microsoft.com/library/mt415893.aspx)。 本文未涵盖所有数据工厂.NET API。 

 
