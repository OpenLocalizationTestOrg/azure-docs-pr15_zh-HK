<properties
   pageTitle="应用程序网关 WebSocket 支持 |Microsoft Azure"
   description="此页提供的应用程序网关 WebSocket 支持概述。"
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>应用程序网关 WebSocket 支持

应用程序网关提供 WebSocket 跨网关大大小小的本机的支持。 没有任何用户可配置的设置，有选择地启用或禁用 WebSocket 支持。 您可以继续使用标准的 HTTPListener 在端口 80/443 接收 WebSocket 通信。 WebSocket 通信然后转向 WebSocket 启用后端服务器使用应用程序网关规则中指定适当的后端库。 WebSocket 协议标准化为[RFC6455](https://tools.ietf.org/html/rfc6455)长时间运行的 TCP 连接上启用服务器和客户端之间的全双工通信。 此功能允许 web 服务器和客户端，可以是双向而无需为需要的基于 HTTP 的实现轮询之间的交互性更强通信。  WebSocket HTTP 与具有低开销和可重复使用同一个 TCP 连接的多个请求/响应导致更高效的资源利用率。 WebSocket 协议被设计用于通过传统的 HTTP 端口 80 和 443。

后端服务器必须响应应用程序网关探测器，[探测器概述健康](application-gateway-probe-overview.md)一节中描述。 应用程序网关健康探测器只是 HTTP/HTTPS，这意味着每个后端服务器必须响应应用程序网关将 WebSocket 通信路由到服务器的 HTTP 探测器。

## <a name="listener-configuration-element"></a>侦听器配置元素

现有 HTTPListener 可用于支持 WebSocket。 下面是一个示例模板文件中的 HttpListeners 元素的代码段。 您将需要 HTTP 和 HTTPS 侦听器支持 WebSocket 并确保 WebSocket 通信的安全。 同样使用[门户网站](application-gateway-create-gateway-portal.md)或[PowerShell](application-gateway-create-gateway-arm.md)创建侦听器应用程序网关在端口 80/443 来支持 WebSocket 通信。


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool、 BackendHttpSetting 和路由规则配置

BackendAddressPool 用于定义启用的 WebSocket 服务器的后端池。 BackendHttpSetting 应定义与后端端口 80/443 只。 基于 cookie 的相似性和 requestTimeouts 属性到 WebSocket 通信无关。 不没有路由规则中所需的任何更改。 基本应继续用于绑定到相应的后端地址池相应的侦听器的路由规则。 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>WebSocket 启用后端

您的后端必须运行在已配置的 HTTP/HTTPS web 服务器端口 (通常 80/443) 的 WebSocket 工作。 这一要求是因为 WebSocket 协议要求为 HTTP 与升级到 WebSocket 协议标头字段作为初始握手。

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

另一个原因是该应用程序网关后端健康探测器支持 HTTP/HTTPS 协议只。 如果后端服务器不响应 HTTP/HTTPS 的探测器，它将离开后端库和任何请求包括 WebSocket 请求，到达该后端。

## <a name="next-steps"></a>下一步行动

WebSocket 支持学习后, 转到[创建的应用程序网关](application-gateway-create-gateway.md)入门 WebSocket 启用 web 应用程序。
