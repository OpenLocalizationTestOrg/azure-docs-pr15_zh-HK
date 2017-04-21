<properties
   pageTitle="应用程序或特定于用户的马拉松服务 |Microsoft Azure"
   description="创建应用程序或特定于用户的马拉松服务"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="容器，马拉松，微-服务，DC/OS，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>创建应用程序或特定于用户的马拉松服务

Azure 容器服务提供了一套主服务器在其我们预配置 Apache Mesos 和马拉松比赛。 这些可以用来协调您的应用程序在集群上，但最好不要为此目的使用的主服务器。 例如，调整马拉松比赛的配置需要登录到主服务器本身，并将变更--该鼓励唯一标准略有不同，需要进行的太和独立管理的主服务器。 此外，通过一个团队所需的配置可能不是另一个团队的优化配置。

在本文中，我们将介绍如何添加应用程序或特定于用户的马拉松比赛服务。

因为此服务将属于单个用户或团队，他们可以自由地在他们希望的任何方式对其进行配置。 另外，Azure 容器服务将确保该服务仍会继续运行。 如果服务失败，Azure 容器服务会重新启动它为您。 大多数情况下您可能会甚至发现它已停机。

## <a name="prerequisites"></a>系统必备组件

与控制器类型的 DC/OS[的 Azure 容器服务实例部署](container-service-deployment.md)并[确保您的客户端可以连接到群集](container-service-connect.md)。 此外，执行以下步骤。

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>创建应用程序或特定于用户的马拉松服务

首先，创建一个 JSON 配置文件定义要创建的应用程序服务的名称。 在这里我们使用`marathon-alice`的框架名称。 将该文件另存为类似`marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

接下来，使用 DC/OS CLI 安装马拉松实例配置文件中设置的选项︰

```bash
dcos package install --options=marathon-alice.json marathon
```

现在，您应看到您`marathon-alice`DC/OS 用户界面的服务选项卡中运行服务。 用户界面将`http://<hostname>/service/marathon-alice/`如果想要直接访问它。

## <a name="set-the-dcos-cli-to-access-the-service"></a>将 DC/OS CLI 访问服务设置

您也可以配置您的 DC/OS CLI 访问此新的服务通过设置`marathon.url`属性以指向`marathon-alice`实例，如下所示︰

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

您可以验证您 CLI 正在与对照的马拉松哪个实例`dcos config show`命令。 可以恢复到主马拉松服务使用命令`dcos config unset marathon.url`。
