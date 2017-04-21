<properties
   pageTitle="为 ACS 应用程序启用公共访问 |Microsoft Azure"
   description="如何启用公共访问到 Azure 容器服务。"
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker，容器，微服务 Mesos，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>启用公共访问到 Azure 容器服务应用程序

在 ACS[公用代理池](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container)任何 DC/OS 容器自动暴露给 internet。 默认情况下，端口**80**， **443**，打开**8080** ，和 （公共） 侦听这些端口的任何容器都可以访问。 本文介绍了如何在 Azure 容器服务中打开多个端口用于您的应用程序。

## <a name="open-a-port-portal"></a>打开的端口 （入口） 

首先，我们需要打开我们想要的端口。

1. 登录到门户中。
2. 查找部署到 Azure 容器服务的资源组。
3. 选择代理负载平衡器 （它的名称类似于**XXXX 代理磅 XXXX**）。

    ![Azure 容器服务负载平衡器](media/container-service-dcos-agents/agent-load-balancer.png)

4. 单击**探测**，再**添加**。

    ![Azure 容器服务负载平衡器探测](media/container-service-dcos-agents/add-probe.png)

5. 填写探测窗体并单击**确定**。

  	| 字段 | 说明 |
  	| ----- | ----------- |
  	| 名称  | 该探测器的描述性名称。 |
  	| 端口  | 若要测试容器的端口。 |
  	| 路径  | （HTTP 模式时）要探测的相对的网站路径。 不支持 HTTPS。 |
  	| 时间间隔 | 将尝试探测之间的时间量，以秒为单位。 |
  	| 不正常的阈值 | 在考虑该容器不正常之前尝试次数连续探测。 | 
    

6. 面对代理负载平衡器的属性，请单击**负载平衡规则**，然后再**添加**。

    ![Azure 容器服务负载平衡器规则](media/container-service-dcos-agents/add-balancer-rule.png)

7. 负载平衡器表单模板并单击**确定**。

  	| 字段 | 说明 |
  	| ----- | ----------- |
  	| 名称  | 负载平衡器的描述性名称。 |
  	| 端口  | 公共的传入端口。 |
  	| 后端端口 | 其中路由通讯量的容器内部公共端口。 |
  	| 池后端 | 此库中的容器将该负载平衡器的目标。 |
  	| 探测器 | 探测器用来确定目标**池后端**是否正常。 |
  	| 会话持久性 | 确定会话的持续时间内如何处理来自客户端的通信。<br><br>**无**︰ 连续请求来自同一客户端可以处理由任何容器。<br>**客户端 IP**︰ 来自相同客户端 IP 的后续请求将由同一个容器。<br>**客户端 IP 和协议**︰ 由同一个容器处理来自相同客户端 IP 和协议组合的后续请求。 |
  	| 空闲超时 | (仅适用于 TCP)在几分钟内，以保持 TCP/HTTP 客户端的时间打开，而不依赖于*保持活动*消息。 |

## <a name="add-a-security-rule-portal"></a>添加安全规则 （门户）

接下来，我们需要添加从我们打开端口通过防火墙路由通信流的安全规则。

1. 登录到门户中。
2. 查找部署到 Azure 容器服务的资源组。
3. 选择**公用**代理网络安全组 （名为类似于**XXXX-代理-公共的 nsg-XXXX**）。

    ![Azure 容器服务网络安全组](media/container-service-dcos-agents/agent-nsg.png)

4. 选择**入站安全规则**，然后再**添加**。

    ![Azure 容器服务网络安全组规则](media/container-service-dcos-agents/add-firewall-rule.png)

5. 填写的防火墙规则以允许公用端口并单击**确定**。

  	| 字段 | 说明 |
  	| ----- | ----------- |
  	| 名称  | 防火墙规则的描述性的名称。 |
  	| 优先级 | 该规则的优先级等级。 越低的数字的优先级就越高。 |
  	| 来源 | 将传入的 IP 地址范围以允许或拒绝通过此规则的限制。 使用**任何**未指定的限制。 |
  	| 服务 | 选择预定义的服务这个安全规则是一组。 否则，使用**自定义**来创建您自己。 |
  	| 协议 | 限制基于**TCP**或**UDP**通信。 使用**任何**未指定的限制。 |
  	| 端口范围 | **自定义****服务**时，请指定此规则影响的端口范围。 您可以使用单个端口，例如**80**或如**1024年 1500年**范围。 |
  	| 操作 | 允许或拒绝通信量符合条件。 |

## <a name="next-steps"></a>下一步行动

了解[公钥和私钥 DC/OS 代理](container-service-dcos-agents.md)之间的差异。

阅读有关[管理 DC/OS 容器](container-service-mesos-marathon-ui.md)的详细信息。