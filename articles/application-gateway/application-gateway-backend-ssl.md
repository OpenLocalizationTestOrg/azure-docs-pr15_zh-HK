<properties
   pageTitle="启用 SSL 策略和应用程序网关的端到端 SSL |Microsoft Azure"
   description="此页概述了端到端 SSL 支持应用程序网关。"
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>启用 SSL 策略和应用程序网关的端到端 SSL

## <a name="overview"></a>概述

应用程序网关支持 SSL 端接在网关上后到后端服务器的通信通常流动未加密。 这使 web 服务器能够从成本高昂的加密解密开销 unburdened。 但是对于某些客户到后端服务器的未加密的通信不是可接受的选项。 这可能是因为安全性/合规性要求或应用程序可能会仅接受安全连接。 对于此类应用程序，应用程序网关现在支持端到端 SSL 加密。

端到端的 SSL 使您可以安全地传输敏感数据的后端加密仍然利用 7 层负载平衡功能的优点哪个应用程序网关，cookie 的相似性，如基于 URL 的路由，为提供支持路由基于站点或能够注入 X-转发-* 标头。

用端到端 SSL 通信模式配置，应用程序网关终止用户在网关的 SSL 会话，并解密用户通信。 然后，它将应用配置的规则来选择通信路由到适当的后端池实例。 然后，应用程序网关启动新的 SSL 连接到后端服务器，并重新加密数据传输到后端的请求之前使用的后端服务器的公钥证书。 端到端 SSL 设置为 Https，再应用于后端池 BackendHTTPSetting 协议设置被启用。 使用端到端 SSL 启用后端池中的每个后端服务器必须配置的证书以允许进行安全的通信。

![端到端 ssl 方案][1]

在此示例中，使用 TLS1.2 的请求被传送到后端服务器池 1 中使用端到端 SSL。

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>端到端的 SSL 和证书的添加到白名单

应用程序网关只与他们的证书与应用程序网关已列入白名单的已知的后端实例。 若要启用的证书添加到白名单，必须将后端服务器证书的公钥上载到应用程序网关 （不根证书）。 然后允许仅连接到已知和列入白名单的 backends。 剩余的 backends 产生一个网关错误。 自签署的证书将用于测试目的只并不适合生产工作负载。 此类证书还必须与应用程序网关如才可以使用前面的步骤中所述的白名单。

## <a name="application-gateway-ssl-policy"></a>应用程序网关 SSL 策略

网关应用程序支持用户可配置 SSL 协商策略，这些策略允许客户更好地控制 SSL 连接在应用程序网关。

1. SSL 2.0 和 3.0，默认情况下禁用所有应用程序网关。 它们是不可配置的。
2. SSL 策略定义提供选项以禁用所有下列 3 协议-TLSv1\_0、 TLSv1\_1，TLSv1\_2。
3. 如果没有 SSL 策略定义所有这三种 (TLSv1\_0、 TLSv1\_1，TLSv1_2) 启用。

## <a name="next-steps"></a>下一步行动

在学习关于端到端 SSL 和 SSL 策略，请转到[端到端上启用 SSL 应用程序网关](application-gateway-end-to-end-ssl-powershell.md)能够将通信发送到 backends，以加密的形式创建的应用程序网关。

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png