<properties
    pageTitle="在 Linux 中使用卷曲和 Azure REST API，HDInsight 上创建 Hadoop，HBase 或风暴的群集 |Microsoft Azure"
    description="了解如何创建基于 Linux 的 HDInsight 群集使用卷曲、 Azure 资源管理器模板和 Azure REST API。 您可以指定群集类型 （Hadoop，HBase 或占领） 或使用脚本安装自定义组件。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>在 HDInsight 使用卷曲和 Azure REST API 创建基于 Linux 的群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure REST API，允许您执行管理操作上承载在 Azure 平台，包括创建新的资源，如基于 Linux 的 HDInsight 群集服务。 在本文中，您将学习如何创建 Azure 资源管理器模板来配置 HDInsight 群集和关联的存储，然后使用 cURL 将模板部署到 Azure REST API 来创建一个新的 HDInsight 群集。

> [AZURE.IMPORTANT] 此文档中的步骤使用 HDInsight 群集默认的辅助节点 (4) 数。 如果您计划多 32 个工作节点，在创建群集或通过创建之后，扩展群集您必须选择至少 8 核和 14 GB ram 的头节点大小。
>
> 节点的大小和相关的成本的详细信息，请参阅[HDInsight 定价](https://azure.microsoft.com/pricing/details/hdinsight/)。

##<a name="prerequisites"></a>系统必备组件

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- __Azure CLI__。 Azure CLI 用于创建服务主体，然后用来生成对 Azure REST API 的请求的身份验证令牌。

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __卷曲__。 此实用程序可通过您软件包管理系统中，或者可以从[http://curl.haxx.se/](http://curl.haxx.se/)下载。

    > [AZURE.NOTE] 如果您使用 PowerShell 在此文档中运行命令，您必须首先删除`curl`，默认情况下创建的别名。 此别名而不是卷曲使用时使用调用 WebRequest，PowerShell cmdlet，`curl`在 PowerShell 提示符下命令，并对于许多本文档中使用的命令会返回错误。
    > 
    > 要删除此别名，请使用 PowerShell 提示符下的以下内容︰
    >
    > `Remove-item alias:curl`
    >
    > 一旦删除别名，应该能够使用已安装在您系统的卷的形式。

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>创建模板

Azure 的资源管理模板是 JSON 文档描述一个__资源组__，并在其所有的资源 （如 HDInsight。)此基于模板方法允许您定义的 HDInsight 在一个模板中，您需要的所有资源并管理组的更改作为一个整体通过更改应用于组的__部署__。

分两个部分; 通常提供了模板模板自身，并使用特定的值填充到您的配置参数文件。 对于示例、 群集名称、 管理员名称和密码。 当直接使用 REST API，您必须将其合并为一个文件。 此 JSON 文档的格式是︰

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

例如，以下是从[https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password)，这将创建基于 Linux 的群集使用密码来保护 SSH 用户帐户的模板和参数文件的合并。

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

本示例将在本文档中的步骤中使用。 必须将文档结尾处的__参数__一节中的占位符_值_替换想要对您的群集使用的值。

##<a name="login-to-your-azure-subscription"></a>登录到 Azure 订阅

按照[连接到 Azure 订阅从 Azure 命令行界面 (Azure CLI)](../xplat-cli-connect.md)中记录的步骤，并连接到您的订阅使用`azure login`命令。

##<a name="create-a-service-principal"></a>创建服务主体

> [AZURE.NOTE] 这些步骤是信息的文档的截选的版本的_主体与密码-Azure CLI 的身份验证服务_[进行身份验证的服务主体与 Azure 资源管理器中](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli)的一节中提供。 下列步骤创建新的服务主体，可以用于创建 Azure 的资源，如 HDInsight 群集使用 REST API 请求进行身份验证。

1. 从命令提示符、 终端会话或外壳程序中，使用以下命令列出 Azure 的订阅。

        azure account list
        
    在列表中，选择您想要使用，并注意__Id__列的订阅。 这是__订阅 ID__ ，并将中的大多数步骤在此文档中使用。

2. 在 Azure 活动目录中创建新的应用程序。

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    替换的值`--name`， `--home-page`，和`--identifier-uris`与您自己的值。 为新的活动目录项中提供的密码。
    
    > [AZURE.NOTE] 由于您正在创建此应用程序身份验证服务主体，通过`--home-page`和`--identifier-uris`的值不需要引用实际的网页上互联网;他们只需要是唯一的 Uri。
    
    从返回的数据，保存__应用程序标识__值。
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. 创建主体使用的__应用程序标识__值返回以前的服务。

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     从返回的数据，保存的__对象 Id__值。
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. 分配给服务主体使用__对象 ID__的值返回以前的__所有者__角色。 您还必须使用前面获得__订阅 ID__ 。
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    此命令完成后，主体服务现在都拥有所有者访问指定的订阅 id。

##<a name="get-an-authentication-token"></a>获取身份验证令牌

1. 使用以下方法来查找您的订阅的__租户 ID__ 。

        azure account show -s <subscription ID>
        
    从返回的数据，发现__租户 ID__。
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. 生成新的令牌使用 Azure REST API。

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    获取或使用以前的值替换__TenantID__，__应用程序标识__和__密码__。

    如果此请求成功，您将收到的 200 系列响应和响应正文中将包含一个 JSON 文档。

    此请求返回的 JSON 文档将包含元素名为__access_token__;此元素的值是您必须使用身份验证请求在本文档的以下各节中使用的访问令牌。
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>创建资源组

以下用于创建新的资源组。 创建资源如 HDInsight 群集之前，必须首先创建组。 

* 创建服务主体时接收到的订阅 ID 替换__SubscriptionID__ 。
* 将__AccessToken__替换为上一步中收到的访问令牌。
* 替换为要在创建资源组和资源，数据中心__DataCenterLocation__ 。 例如，美国。 
* 将__ResourceGroupName__替换为您希望使用此组的名称︰

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

如果此请求成功，您将收到的 200 系列响应和响应正文中将包含一个 JSON 文档包含有关组的信息。 `"provisioningState"`元素将包含值为`"Succeeded"`。

##<a name="create-a-deployment"></a>创建部署

使用下列部署群集配置 （模板和参数值，） 给资源组。

* __SubscriptionID__和__AccessToken__替换为使用以前的值。 
* 在上一节中创建的资源组名称替换__ResourceGroupName__ 。
* 您想要使用此部署的名称替换__DeploymentName__ 。

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] 如果您保存包含的模板和文件参数的 JSON 文档，则可以使用以下而不是`-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

如果此请求成功，您将收到的 200 系列响应和响应正文中将包含一个 JSON 文档包含有关部署操作的信息。

> [AZURE.IMPORTANT] 注意部署已提交，但这一次没有完成。 它可能需要几分钟，通常大约 15 部署完成。

##<a name="check-the-status-of-a-deployment"></a>检查部署状态

要检查部署状态，请使用以下方法︰

* __SubscriptionID__和__AccessToken__替换为使用以前的值。 
* 在上一节中创建的资源组名称替换__ResourceGroupName__ 。

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

这将返回信息包含在部署操作有关的信息的 JSON 文档。 `"provisioningState"`元素将包含状态的部署;如果此属性包含的值为`"Succeeded"`，然后部署已成功完成。 此时，您的群集应可供使用。

##<a name="next-steps"></a>下一步行动

现在，您已成功创建一个 HDInsight 群集，使用以下方法来学习如何使用您的群集。 

###<a name="hadoop-clusters"></a>Hadoop 群集

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [使用 HDInsight 的小猪](hdinsight-use-pig.md)
* [HDInsight 使用 MapReduce](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase 群集

* [在 HDInsight 上的 HBase 入门](hdinsight-hbase-tutorial-get-started-linux.md)
* [开发 HBase HDInsight 上的 Java 应用程序](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>风暴的群集

* [在 HDInsight 上的风暴为开发 Java 拓扑](hdinsight-storm-develop-java-topology.md)
* [在 HDInsight 上的风暴中使用 Python 组件](hdinsight-storm-develop-python-topology.md)
* [部署和监视在 HDInsight 上的风暴与拓扑](hdinsight-storm-deploy-monitor-topology-linux.md)
