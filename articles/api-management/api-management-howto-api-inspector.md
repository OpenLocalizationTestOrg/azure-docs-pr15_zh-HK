<properties 
    pageTitle="在 Azure API 管理中如何使用 API 跟踪检查呼叫" 
    description="了解如何跟踪在 Azure API 管理使用 API 检查器的调用。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>在 Azure API 管理中如何使用 API 跟踪检查呼叫

API 管理提供了一个 API 检查器工具，以帮助您进行调试和排除故障您的 Api。 API 检查器可以以编程方式使用，也可以直接从开发人员门户使用。 

除了跟踪操作 API 检查器还跟踪[策略表达式](https://msdn.microsoft.com/library/azure/dn910913.aspx)求值。 有关演示，请参见[云介绍节目 177︰ 更多 API 管理功能](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/)和快进到 21:00。

本指南提供了使用 API 检查器中的浏览。

>[AZURE.NOTE] 检查器 API 跟踪仅生成，并可用于包含订阅键的所属的[管理员](api-management-howto-create-groups.md)帐户的请求。

## <a name="trace-call"></a>使用 API 检查器来跟踪调用

若要使用 API 检查器，添加**ocp apim 跟踪︰ 真**请求给您操作调用，头，然后下载并使用通过**ocp apim 跟踪位置**响应标头中指定的 URL 的跟踪检查。 这可以以编程的方式，来完成，也可以直接从开发人员门户。

本教程展示如何使用 API 检查器使用基本配置[管理您的第一个 API](api-management-get-started.md)获取入门教程中的计算器 API 跟踪操作。 如果您没有完成该指南只需要花费一些时间来导入基本的计算器 API，也可以使用另一个 API 回响 API 如您选择。 每个 API 管理服务实例都预先配置了与 Echo API，可用于试验和了解 API 管理。 回音式 API 回返回任何输入发送给它。 若要使用它，您可以调用任何 HTTP 谓词，并返回值只是发送的内容。 



若要开始，请单击 API 管理服务的 Azure 经典**开发人员门户**。 可以直接从开发人员门户，可以方便地查看和测试操作的 API 的调用操作。

>如果还没有创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

![API 管理开发人员门户][api-management-developer-portal-menu]

从顶部的菜单中，单击**Api** ，然后单击**基本的计算器**。

![回音式 API][api-management-api]

单击**试一试**着尝试**添加两个整数**操作。

![试一试][api-management-open-console]

保持默认的参数值，并选择您想要使用**订阅项**下拉列表中的该产品的订阅键。

默认情况下，在开发人员门户**Ocp Apim 跟踪**头已设置为**true**。 此标头配置生成的跟踪。

![发送][api-management-http-get]

请单击**发送**来调用操作。

![发送][api-management-send-results]

在响应标头将**ocp apim 跟踪位置**类似于下面的示例值。

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

可以从指定的位置下载跟踪和审查的下一步所示。

## <a name="inspect-trace"></a>检查跟踪

若要查看跟踪内的值，请从**ocp apim 跟踪位置**URL 下载跟踪文件。 它是以 JSON 格式的文本文件，包含与以下示例类似的条目。

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"></a>下一步行动

-   观看演示的跟踪中的策略表达式[云介绍节目 177︰ 更多 API 管理功能](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/)。 快进到 21:00，查看演示。

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 