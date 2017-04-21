<properties
   pageTitle="通过 REST API 的 azure 容器服务容器管理 |Microsoft Azure"
   description="使用 REST API，马拉松到 Azure 容器服务 Mesos 群集部署的容器。"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker，容器，微服务 Mesos，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-rest-api"></a>通过 REST API 的容器管理

DC/OS 提供了部署和调整聚集的工作负载，同时抽象底层硬件的环境。 在 DC/OS，没有管理计划和执行计算的工作负载的框架。

虽然框架适用于许多常见的工作负载，本文档介绍如何创建和扩展容器部署通过马拉松比赛。 这些示例之前，需要 Azure 容器服务中配置一个 DC/OS 群集。 您还需要具有远程连接到该群集。 这些项目的详细信息，请参阅下列文章︰

- [部署群集 Azure 容器服务](container-service-deployment.md)
- [连接到 Azure 容器服务群集](container-service-connect.md)

您已连接到 Azure 容器服务群集后，您可以通过 http://localhost:local 端口访问 DC/OS 和相关的 REST Api。 本文档中的示例假定为在端口 80 上隧道。 例如，可以通过马拉松比赛终点`http://localhost/marathon/v2/`。 不同的 Api 的详细信息，请参阅[马拉松 API](https://mesosphere.github.io/marathon/docs/rest-api.html)和[Chronos API](https://mesos.github.io/chronos/docs/api.html)的 Mesosphere 文档和[Mesos 计划程序 API](http://mesos.apache.org/documentation/latest/scheduler-http-api/)的 Apache 文档。

## <a name="gather-information-from-dcos-and-marathon"></a>从 DC/OS 和马拉松收集信息

对 DC/OS 群集部署容器之前，收集一些有关该 DC/OS 群集，如名和 DC/OS 代理的当前状态的信息。 若要执行此操作，查询`master/slaves`的 DC/OS REST API 端点。 如果一切顺利，您将看到每个的 DC/OS 代理和多个属性的列表。

```bash
curl http://localhost/mesos/master/slaves
```

现在，使用马拉松`/apps`要检查当前的应用程序部署到 DC/OS 群集的终结点。 如果这是一个新的群集，您将看到应用程序为一个空数组。

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>部署 Docker 格式化的容器

通过使用 JSON 文件描述预期的部署中部署 Docker 格式化通过马拉松比赛的容器。 下面的示例将部署 Nginx 容器，容器的端口 80 的 DC/OS 代理绑定端口 80。 此外请注意，acceptedResourceRoles 属性设置为 slave_public。 这会将该容器部署到面向公众的代理比例集中代理。

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

为了部署 Docker 格式化的容器，创建您自己的 JSON 文件，或使用[Azure 容器服务演示](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)在中提供的示例。 将其存储在可访问位置。 接下来，要部署的容器，请运行以下命令。 指定的 JSON 文件的名称。

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

输出将类似如下︰

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

现在，如果您的应用程序查询马拉松比赛，此新的应用程序将显示在输出中。

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>扩展您的容器

此外可以使用马拉松 API 来扩张或缩放应用程序部署中。 在前面的示例中，您已部署应用程序的一个的实例。 让我们来扩展此出到应用程序的三个实例。 若要执行此操作，使用下面的 JSON 文本，创建一个 JSON 文件并将其存储在可访问位置。

```json
{ "instances": 3 }
```

运行以下命令来扩展该应用程序。

>[AZURE.NOTE] 该 URI 将是 http://localhost/marathon/v2/apps/，然后缩放应用程序的 ID。 如果您正在使用 Nginx 示例，这里提供的 URI 将为 http://localhost/marathon/v2/apps/nginx。

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

最后，查询马拉松比赛终结点的应用程序。 您将看到，现在有三个 Nginx 容器。

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>在本练习中使用 PowerShell︰ 与 PowerShell 的马拉松 REST API 交互

可以通过在 Windows 系统上使用 PowerShell 命令来执行这些相同的操作。

若要收集有关 DC/OS 群集，如代理名称和代理状态的信息运行下面的命令。

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

通过使用 JSON 文件描述预期的部署中部署 Docker 格式化通过马拉松比赛的容器。 下面的示例将部署 Nginx 容器，容器的端口 80 的 DC/OS 代理绑定端口 80。

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

创建您自己的 JSON 文件，或使用[Azure 容器服务演示](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)在中提供的示例。 将其存储在可访问位置。 接下来，要部署的容器，请运行以下命令。 指定的 JSON 文件的名称。

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

此外可以使用马拉松 API 来扩张或缩放应用程序部署中。 在前面的示例中，您已部署应用程序的一个的实例。 让我们来扩展此出到应用程序的三个实例。 若要执行此操作，使用下面的 JSON 文本，创建一个 JSON 文件并将其存储在可访问位置。

```json
{ "instances": 3 }
```

运行以下命令来扩展该应用程序。

> [AZURE.NOTE] 该 URI 将是 http://localhost/marathon/v2/apps/，然后缩放应用程序的 ID。 如果您在此处使用 Nginx 示例提供，URI 是 http://localhost/marathon/v2/apps/nginx。

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>下一步行动

- [了解更多关于 Mesos HTTP 端点]( http://mesos.apache.org/documentation/latest/endpoints/)。
- [了解更多关于 REST API，马拉松比赛]( https://mesosphere.github.io/marathon/docs/rest-api.html)。
