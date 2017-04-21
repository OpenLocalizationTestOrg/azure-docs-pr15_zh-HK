<properties
    pageTitle="日志记录和错误处理逻辑的应用程序中 |Microsoft Azure"
    description="查看高级的错误处理和日志记录与逻辑应用程序的实际用例"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>日志记录和逻辑的应用程序中的错误处理

本文介绍了如何扩展逻辑应用程序能够更好地支持异常处理。 它是真实的用例，我们解答这一问题，"没有逻辑的应用程序支持的异常和错误处理？"

>[AZURE.NOTE]当前版本的 Microsoft Azure 应用程序服务的应用程序逻辑功能的操作响应提供一个标准的模板。
>这包括内部验证和从 API 应用程序返回的错误响应。

## <a name="overview-of-the-use-case-and-scenario"></a>介绍了用例和方案

下面的故事是本文中的用例。
已知的医疗保健组织参与我们开发者使用 Microsoft Dynamics CRM Online 创建患者门户 Azure 解决方案。 他们需要发送 Dynamics CRM Online 的患者门户与队伍之间的约会记录。  我们被要求对所有病历使用[HL7 FHIR](http://www.hl7.org/implement/standards/fhir/)标准。

该项目有两个主要要求︰  

 -  日志记录发送 Dynamics CRM Online 的门户网站的一种方法
 -  查看工作流中发生的任何错误的办法


## <a name="how-we-solved-the-problem"></a>我们如何解决问题

>[AZURE.TIP] 在[集成用户组](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "的集成用户组")，您可以查看项目的高级视频。

我们选择[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") （DocumentDB 将作为文档记录） 的日志和错误记录的储存库。 由于逻辑应用程序的所有响应的标准模板，我们就不需要创建自定义的架构。 我们可以创建**插入**和**查询**API 应用程序错误和日志记录。 我们还可以为 API 应用程序中的每个定义架构。  

另一个要求是要清除在某个日期之后的记录。 DocumentDB 具有一个名为[生存时间](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "生存时间")(TTL)，这使我们能够将每个记录或集合的**生存时间**值设置属性。 这消除了需要手动删除 DocumentDB 中的记录。

### <a name="creation-of-the-logic-app"></a>逻辑应用程序的创建

第一步是创建逻辑应用程序并将其加载设计器中。 在此示例中，我们将使用父-子逻辑应用程序。 让我们假设我们已经创建了父并打算创建一个子逻辑应用程序。

因为我们将会记录从 Dynamics CRM Online 的记录，让我们从顶端开始。 我们需要使用请求触发器，因为父逻辑应用程序触发此子。

> [AZURE.IMPORTANT] 若要完成本教程，您需要创建一个 DocumentDB 数据库和两个集合 （日志记录和错误）。

### <a name="logic-app-trigger"></a>逻辑应用程序触发

下面的示例中所示，我们正在使用请求触发器。

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>步骤

我们需要从 Dynamics CRM Online 的门户记录患者的记录源 （请求）。

1. 我们需要从 Dynamics CRM Online 获取新的约会记录。
    来自 CRM 的触发器为我们提供**CRM PatentId**，**记录类型**，**新建或更新的记录**(新或更新的布尔值)，和**SalesforceId**。 **SalesforceId**可以为 null，因为它仅用于更新。
    我们将会使用 CRM **PatientID**和**记录类型**的 CRM 记录。
1. 接下来，我们需要将我们 DocumentDB API 应用程序**InsertLogEntry**操作添加下图中所示。


#### <a name="insert-log-entry-designer-view"></a>插入日志项的设计器视图

![插入日志条目](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>插入错误条目设计器视图
![插入日志条目](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>检查创建记录失败

![条件](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>逻辑应用程序源代码

>[AZURE.NOTE]  以下是示例只。 本教程基于当前生产中实现，因为**源节点**的值可能不会显示与安排约会相关的属性。

### <a name="logging"></a>日志记录
下面的逻辑应用程序的代码示例演示如何处理日志记录。

#### <a name="log-entry"></a>日志条目
这是用于插入日志项的逻辑应用程序源代码。

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>请求日志

这是日志请求消息发送到 API 的应用程序。

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>响应日志

这是从 API 应用程序日志的响应消息。

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

现在让我们看一下错误处理步骤。


### <a name="error-handling"></a>错误处理

逻辑应用程序下面的代码示例演示如何实现错误处理。

#### <a name="create-error-record"></a>创建错误记录

这是创建一个错误记录的逻辑应用程序源代码。

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>插入错误到 DocumentDB-请求

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>DocumentDB-响应中插入错误


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>销售队伍错误响应

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>返回响应返回给父逻辑应用程序

响应后，可以将它传递回父逻辑应用程序中。

#### <a name="return-success-response-to-the-parent-logic-app"></a>成功将响应返回到父逻辑应用程序

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>返回到父逻辑应用程序错误响应

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>DocumentDB 存储库和门户

我们的解决方案添加了与[DocumentDB](https://azure.microsoft.com/services/documentdb)的其他功能。

### <a name="error-management-portal"></a>错误管理门户

若要查看这些错误，您可以创建 MVC web 应用程序来显示 DocumentDB 中的错误记录。 在当前版本中包括**列表**、**详细信息**、**编辑**和**删除**操作。

> [AZURE.NOTE]编辑操作︰ DocumentDB does 替换整个文档。
> **列表**和**详细信息**视图中显示的记录是只样本。 它们不是实际的患者约会记录。

以下是我们使用前面所述的方法创建 MVC 应用程序详细信息的示例。

#### <a name="error-management-list"></a>错误管理列表

![错误列表](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>管理详细信息视图时出错

![错误详细信息](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>日志管理门户

若要查看日志，我们还会创建一个 MVC web 应用程序。  以下是我们使用前面所述的方法创建 MVC 应用程序详细信息的示例。

#### <a name="sample-log-detail-view"></a>日志详细信息视图的示例

![日志详细信息视图](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API 的应用程序的详细信息

#### <a name="logic-apps-exception-management-api"></a>逻辑应用程序异常管理 API

我们开放源代码逻辑应用程序异常管理 API 应用程序提供了下列功能。

有两个控制器︰

- **ErrorController** DocumentDB 集合中插入错误记录 （文档）。
- **LogController**DocumentDB 集合中插入日志记录 （文档）。

> [AZURE.TIP] 两个控制器使用`async Task<dynamic>`操作。 它允许操作人员以解决在运行时，因此我们可以操作的正文中创建的 DocumentDB 架构。

在 DocumentDB 中的每个文档都必须具有唯一的 id。 我们使用的`PatientId`并添加转换为 Unix 时间戳值 （双精度型） 的时间戳。 我们截断以删除小数部分的值。

您可以查看我们错误控制器 API[从 GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)的源代码。

我们通过使用以下语法从逻辑应用程序调用 API。

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

上述代码示例中的表达式检查**失败**的*Create_NewPatientRecord*状态。

## <a name="summary"></a>摘要

- 日志记录和错误处理逻辑应用程序中，您可轻松实施。
- 为存储库中可以使用 DocumentDB 为日志和错误记录 （文档）。
- 可以使用 MVC 创建门户以显示日志和错误记录。

### <a name="source-code"></a>源代码
逻辑应用程序异常管理 API 应用程序的源代码是(https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "逻辑应用程序异常管理 API")此[GitHub 存储库]中可用。


## <a name="next-steps"></a>下一步行动
- [查看更多的逻辑应用程序示例和应用场景](app-service-logic-examples-and-scenarios.md)
- [了解逻辑应用程序监视工具](app-service-logic-monitor-your-logic-apps.md)
- [创建逻辑应用程序自动的部署模板](app-service-logic-create-deploy-template.md)
