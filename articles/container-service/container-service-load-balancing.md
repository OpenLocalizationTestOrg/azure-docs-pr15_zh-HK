<properties
   pageTitle="负载平衡容器在 Azure 容器服务群集 |Microsoft Azure"
   description="负载平衡在 Azure 容器服务群集中的多个容器。"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="容器，微服务 DC/OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>在 Azure 容器服务群集负载平衡容器

在本文中，我们将探索如何创建在内部负载平衡器直流/OS 管理 Azure 容器服务使用马拉松磅。 这将使您可以横向扩展您的应用程序。 它将使您能够利用公共和私人代理的群集通过将负载平衡群集公用和专用群集上的您应用程序容器。

## <a name="prerequisites"></a>系统必备组件

与控制器类型的 DC/OS[的 Azure 容器服务实例部署](container-service-deployment.md)并[确保您的客户端可以连接到群集](container-service-connect.md)。 

## <a name="load-balancing"></a>负载平衡

我们将创建容器服务群集中有两种负载平衡图层︰ 

  1. Azure 负载平衡器提供公共入口点 （最终用户会碰到的那些）。 这 Azure 容器服务将自动提供，默认情况下，配置为在端口 80、 443 或 8080 的公开。
  2. 马拉松比赛负载平衡器 （马拉松磅） 将入站的请求路由到这些请求提供服务的容器实例。 我们扩展我们的 web 服务提供的容器，如马拉松磅进行动态调整。 默认情况下，在容器服务，未提供该负载平衡器，但它是很容易安装。

## <a name="marathon-load-balancer"></a>马拉松比赛负载平衡器

马拉松比赛负载平衡器动态地重新配置基于您已经部署了容器本身。 如果发生这种情况，Apache Mesos 将只需重新启动其他位置的容器，将调整马拉松比赛磅，还有弹性容器或代理的损失。

若要安装马拉松负载平衡器可以使用任一 DC/OS 的 web 用户界面或命令行。

### <a name="install-marathon-lb-using-dcos-web-ui"></a>安装马拉松磅使用 DC/OS Web 用户界面

  1. 请单击 '世界'
  2. 马拉松磅中搜索
  3. 单击安装

![通过直流/OS Web 界面的安装马拉松磅](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>安装使用 DC/OS CLI 马拉松磅

后安装 DC/OS CLI 并确保您可以连接到群集中，从客户端计算机上运行以下命令︰

```bash
dcos package install marathon-lb
```

此命令会自动安装在公用代理群集上的负载平衡器。

## <a name="deploy-a-load-balanced-web-application"></a>部署了负载平衡的 Web 应用程序

现在，我们有马拉松磅包，我们可以将部署我们希望进行负载平衡的应用程序容器。 本例中我们将部署一个简单的 web 服务器使用以下配置︰

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * 设置的值`HAProxy_0_VHOST`为您代理负载平衡器的 FQDN。 这是在窗体中`<acsName>agents.<region>.cloudapp.azure.com`。 例如，如果创建一个容器服务群集名称`myacs`地区`West US`，FQDN 将为`myacsagents.westus.cloudapp.azure.com`。 您还可以寻找名称中的"代理"负载平衡器，当您通过在[Azure 门户](https://portal.azure.com)容器服务创建的资源组中的资源查找发现这。
  * 将 servicePort 设置为端口 > = 10000。 这标识该服务正在运行中此容器-马拉松磅使用它来确定它应在平衡的服务。
  * 设置`HAPROXY_GROUP`标记为"外部"。
  * 设置`hostPort`为 0。 这意味着马拉松将任意分配一个可用的端口。
  * 设置`instances`为您想要创建的实例数。 您可以始终扩展这些向上和向下以后。

Noing 值得的是，默认情况下，马拉松比赛将部署到专用群集，这意味着，上述部署将仅可通过负载平衡器，通常就是我们所希望的行为。

### <a name="deploy-using-the-dcos-web-ui"></a>部署使用 DC/OS Web 用户界面

  1. （后设置[SSH 隧道](container-service-connect.md)和单击访问 http://localhost/marathon 处的马拉松页`Create Appliction`
  2. 在`New Application`对话框，单击`JSON Mode`在右上角
  3. 上面的 JSON 粘贴到编辑器
  4. 单击`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>部署使用 DC/OS CLI

要部署此应用程序与 DC/OS CLI 直接复制上面的 JSON 到名为的文件`hello-web.json`，然后运行︰

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure 负载平衡器

默认情况下，Azure 负载平衡器公开端口 80、 8080，和 443。 如果您使用的一这三个端口 （像我们一样在上面的示例），则没有什么需要做。 您应该能够命中代理负载平衡器的 FQDN，然后刷新时，每次您将击中三个 web 服务器以轮循机制方式之一。 但是，如果您使用不同的端口，您需要添加一个循环规则和探测器在负载平衡器使用的端口。 你可以从[Azure CLI](../xplat-cli-azure-resource-manager.md)命令`azure network lb rule create`， `azure network lb probe create`。 您还可以执行此使用 Azure 门户。


## <a name="additional-scenarios"></a>其他方案

您可以让的方案中使用不同的域不同的服务进行公开。 例如︰

mydomain1.com-> Azure LB:80-> 马拉松-lb:10001-mycontainer1:33292 >  
mydomain2.com-> Azure LB:80-> 马拉松-lb:10002-mycontainer2:22321 >

要实现这一点，签出[虚拟主机](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/)，它提供的信息关联到特定的马拉松磅路径域的方法。

或者，可以公开不同的端口，并重新映射到马拉松磅背后正确的服务。例如︰

Azure lb:80-> 马拉松-lb:10001-mycontainer:233423 >  
Azure lb:8080-> 马拉松-lb:1002-mycontainer2:33432 >


## <a name="next-steps"></a>下一步行动

请参阅更多在[马拉松磅](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/)的 DC/OS 文档。
