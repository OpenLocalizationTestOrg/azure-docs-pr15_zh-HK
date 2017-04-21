<properties 
    pageTitle="从 FTP 服务器移动数据 |Microsoft Azure" 
    description="了解如何将数据从 FTP 服务器使用 Azure 数据工厂移动。" 
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
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>将数据从 FTP 服务器使用 Azure 数据工厂
这篇文章概括介绍了如何在 Azure 数据工厂中使用复制活动将数据从 FTP 服务器移动到一个受支持的接收数据存储区。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章概要介绍了数据移动提供复制活动和支持作为源/接收器的数据存储区列表。 

数据工厂目前支持仅移动数据从 FTP 服务器的其他数据存储，而不是将数据从其他数据存储区移动到 FTP 服务器。 它支持内部部署和云 FTP 服务器。 

如果将数据从**内部**FTP 服务器移动到云数据存储区 (示例︰ Azure Blob 存储)、 安装和使用数据管理网关。 数据管理网关是一个使云服务来连接到内部资源您本地计算机安装的客户机代理。 网关的详细信息，请参阅[数据管理网关](data-factory-data-management-gateway.md)。 设置网关和使用它，请参阅[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)分步指导文章。 使用网关连接到 FTP 服务器，即使服务器在 Azure IaaS 虚拟机 (VM)。 

作为 FTP 服务器，可以在同一本地计算机或 Azure IaaS VM 安装网关。 但是，我们建议您安装网关，在另外一台计算机或不同的 Azure IaaS VM 避免资源争用和更好的性能。 在网关安装在单独的计算机上时，计算机应该能够访问 FTP 服务器。 

## <a name="copy-data-wizard"></a>复制数据向导
创建管线从 FTP 服务器复制数据的最简便方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>示例︰ 将数据从 FTP 服务器复制到 Azure 的斑点

此示例演示如何将数据从 FTP 服务器复制到 Azure Blob 存储。 但是，数据可以复制**直接**对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  
 
此示例具有以下数据工厂实体︰

- 链接的类型[FtpServer](#ftp-linked-service-properties)的服务。
- 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
- [文件](#fileshare-dataset-type-properties)类型输入的[数据集](data-factory-create-datasets.md)。
- 类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
- [管线](data-factory-create-pipelines.md)与使用[FileSystemSource](#ftp-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从 FTP 服务器到 Azure 的 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

**FTP 链接服务**此示例使用基本身份验证与用户名称和密码以纯文本格式。 您还可以使用下列方法之一︰ 

- 匿名身份验证 
- 基本身份验证与加密凭据
- FTP 通过 SSL/TLS （ftp） 将另

请参见针对不同类型的身份验证，您可以使用[FTP 链接服务](#ftp-linked-service-properties)一节。 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
    }

**Azure 存储链接服务**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**FTP 输入数据集**此数据集是指 FTP 文件夹`mysharedfolder`和`test.csv`。 管道文件复制到目标位置。 

设置为"外部":"真正的"通知数据工厂服务数据集外部数据工厂并不由数据工厂中的活动。
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure Blob 输出数据集**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**管线与复制活动**

管线包含被配置为使用的输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**FileSystemSource** ，**接收器**类型设置为**BlobSink**。 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>FTP 链接服务属性

下表提供了 JSON 元素特定于 FTP 链接的服务的说明。

| 属性 | 说明 | 必填 | 默认 |
| -------- | ----------- | -------- | ------- | 
| 类型 | 类型属性必须设置为 FtpServer | 是的 | &nbsp;
| 主机 | FTP 服务器的名称或 IP 地址 | 是的 | &nbsp;
| authenticationType | 指定身份验证类型 | 是的 | 基本的匿名 |
| 用户名 | 有权访问 FTP 服务器的用户 | 不 | &nbsp;
| 密码 | （用户名） 的用户密码 | 不 | &nbsp;
| encryptedCredential | 要访问该 FTP 服务器的加密的凭据 | 不 | &nbsp;
| gatewayName | 数据管理网关网关连接到内部 FTP 服务器名称 | 不    | &nbsp;
| 端口 | FTP 服务器正在侦听的端口 | 不 | 21 |
| enableSsl | 指定是否通过 SSL/TLS 通道使用 FTP | 不 | 真 | 
| enableServerCertificateValidation | 指定是否通过 SSL/TLS 通道使用 FTP 时启用服务器 SSL 证书验证 | 不 | 真 | 

### <a name="using-anonymous-authentication"></a>使用匿名身份验证

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>使用用户名和密码以纯文本格式，用于基本身份验证
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>使用端口、 enableSsl、 enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>使用身份验证和网关 encryptedCredential

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

有关设置内部 FTP 数据源的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security)。

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>FTP 复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。 

该活动的 typeProperties 部分中可用的属性在另一方面随每种活动类型。 复制活动类型属性而异的种源和接收器。

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。

## <a name="next-steps"></a>下一步行动
请参阅以下文章︰ 

- 使用复制活动创建管线的指导[教程复制活动](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。 
