<properties
 pageTitle="计划程序的出站身份验证"
 description="计划程序的出站身份验证"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>计划程序的出站身份验证

调度程序作业可能需要动员到要求身份验证的服务。 这样一来，被调用的服务可以确定是否调度作业可以访问它的资源。 其中一些服务包括其他 Azure 服务、 Salesforce.com、 Facebook 和安全的自定义网站。

## <a name="adding-and-removing-authentication"></a>添加和删除身份验证

添加到计划程序作业的身份验证很简单 – 添加一个 JSON 子元素`authentication`到`request`元素时创建或更新作业。 机密信息传递给传、 修补或 POST 请求 – 中计划服务的一部分`authentication`对象--永远不会在响应中返回。 在响应中，机密信息被设置为 null 或已表示经过验证的实体的公钥标记。

若要删除身份验证，放置或修补作业显式地设置`authentication`为空值的对象。 您不会看到在响应中的任何身份验证属性。

目前，唯一受支持的身份验证类型是`ClientCertificate`（对于使用 SSL/TLS 客户端证书），模型`Basic`模型 （用于基本身份验证），和`ActiveDirectoryOAuth`模型 （对于当前目录 OAuth 验证）。

## <a name="request-body-for-clientcertificate-authentication"></a>提出身份验证请求正文

当添加身份验证使用`ClientCertificate`模型，则在请求正文中指定下面的其他元素。  

|元素|说明|
|:---|:---|
|_身份验证 （父元素）_|使用 SSL 客户端证书身份验证对象。|
|_类型_|必填。 身份验证类型。对于 SSL 客户端证书，值必须为`ClientCertificate`。|
|_pfx_|必填。 PFX 文件的 Base64 编码的内容。|
|_密码_|必填。 要访问该 PFX 文件密码。|


## <a name="response-body-for-clientcertificate-authentication"></a>提出身份验证响应正文

当使用身份验证信息发送一个请求时，响应将包含下列身份验证相关的元素。

|元素 |说明 |
|:--|:--|
|_身份验证 （父元素）_ |使用 SSL 客户端证书身份验证对象。|
|_类型_ |身份验证类型。 对于 SSL 客户端证书，则值是`ClientCertificate`。|
|_certificateThumbprint_ |证书的指纹。|
|_certificateSubjectName_ |证书的主题可分辨的名称。|
|_certificateExpiration_ |证书的到期日期。|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>提出身份验证的示例 REST 请求

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>其余部分响应示例提出身份验证

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>对于基本身份验证的请求正文

当添加身份验证使用`Basic`模型，则在请求正文中指定下面的其他元素。

|元素|说明|
|:--|:--|
|_身份验证 （父元素）_ |使用基本身份验证的身份验证对象。|
|_类型_ |必填。 身份验证类型。 该值必须是用于基本身份验证， `Basic`。|
|_用户名_ |必填。 进行身份验证的用户名。|
|_密码_ |必填。 进行身份验证的密码。|

## <a name="response-body-for-basic-authentication"></a>对于基本身份验证的响应正文

当使用身份验证信息发送一个请求时，响应将包含下列身份验证相关的元素。

|元素|说明|
|:--|:--|
|_身份验证 （父元素）_ |使用基本身份验证的身份验证对象。|
|_类型_ |身份验证类型。 值是用于基本身份验证， `Basic`。|
|_用户名_ |经过身份验证的用户名。|

## <a name="sample-rest-request-for-basic-authentication"></a>对于基本身份验证的示例 REST 请求

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>对于基本身份验证的示例其余部分响应

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>对于 ActiveDirectoryOAuth 身份验证请求正文

当添加身份验证使用`ActiveDirectoryOAuth`模型，则在请求正文中指定下面的其他元素。

|元素 |说明 |
|:--|:--|
|_身份验证 （父元素）_ |使用 ActiveDirectoryOAuth 身份验证身份验证对象。|
|_类型_ |必填。 身份验证类型。 ActiveDirectoryOAuth 验证的值必须为`ActiveDirectoryOAuth`。|
|_租户_ |必填。 Azure AD 租户租户标识符。|
|_访问群体_ |必填。 此选项设置为 https://management.core.windows.net/。|
|_客户机 Id_ |必填。 Azure AD 应用程序提供的客户端标识符。|
|_秘密_ |必填。 客户端请求令牌的机密。|

### <a name="determining-your-tenant-identifier"></a>确定您组织的标识符

可以找到租户标识符的 Azure AD 租户通过运行`Get-AzureAccount`在 Azure PowerShell。

## <a name="response-body-for-activedirectoryoauth-authentication"></a>对于 ActiveDirectoryOAuth 身份验证响应正文

当使用身份验证信息发送一个请求时，响应将包含下列身份验证相关的元素。

|元素 |说明 |
|:--|:--|
|_身份验证 （父元素）_ |使用 ActiveDirectoryOAuth 身份验证身份验证对象。|
|_类型_ |身份验证类型。 对于 ActiveDirectoryOAuth 身份验证，则值是`ActiveDirectoryOAuth`。|
|_租户_ |Azure AD 租户租户标识符。 |
|_访问群体_ |此选项设置为 https://management.core.windows.net/。|
|_客户机 Id_ |Azure AD 应用程序的客户端标识符。|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>ActiveDirectoryOAuth 身份验证的示例 REST 请求

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>其余部分响应示例 ActiveDirectoryOAuth 身份验证

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>请参见


 [计划是什么？](scheduler-intro.md)

 [Azure 计划程序概念、 术语和实体层次结构](scheduler-concepts-terms.md)

 [开始在 Azure 门户使用计划程序](scheduler-get-started-portal.md)

 [计划和 Azure 计划程序中的计费](scheduler-plans-billing.md)

 [Azure 计划其余部分 API 参考](https://msdn.microsoft.com/library/mt629143)

 [Azure 计划 PowerShell cmdlet 引用](scheduler-powershell-reference.md)

 [Azure 的计划程序的高可用性和可靠性](scheduler-high-availability-reliability.md)

 [Azure 计划限制、 默认值和错误代码](scheduler-limits-defaults-errors.md)
