<properties
    pageTitle="启用会话关系为 Eclipse 使用 Azure Toolkit"
    description="了解如何启用对 Eclipse 使用 Azure Toolkit 的会话关联。"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>启用会话关联 #

在 Eclipse 的 Azure Toolkit，您可以启用 HTTP 会话关联或"粘滞会话"，为您的角色。 下图显示了**负载平衡**的属性对话框用于启用了会话关联功能︰

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>若要启用您的角色的会话关联 ##

1. 右击在 Eclipse 的项目资源管理器中的角色， **Azure**，请单击，然后单击**负载平衡**。
1. 在**WorkerRole1 负载平衡属性**对话框中︰
    1. 检查**为此角色的启用 HTTP 会话关联 （粘滞会话）。**
    1. 为**要使用的输入终结点**，选择输入终结点，例如，使用**http (公钥︰ 80，私有︰ 8080)**。 您的应用程序必须使用该终结点作为其 HTTP 端点。 您可以启用多个终结点，您的角色，但您可以选择仅其中之一来支持粘滞会话。
    1. 重新生成您的应用程序。

一旦启用，如果您有多个角色实例，来自特定客户端的 HTTP 请求将继续处理相同的角色实例。

日蚀式 Toolkit 使这通过安装到每个角色实例调用应用程序请求路由 (ARR) 的特殊 IIS 模块。 ARR 重排 HTTP 请求路由到相应角色实例。 该工具包自动重新配置所选终结点，以便将传入的 HTTP 通信量首先被发送到 ARR 软件。 该工具包还会创建一个新的内部终结点，您的 Java 服务器配置为侦听。 这就是由 ARR 重新路由到相应角色实例的 HTTP 通信的终结点。 这样一来，多实例部署中的每个角色实例作为启用粘滞会话的所有其他情况下，反向代理。

## <a name="notes-about-session-affinity"></a>有关会话关联的注释 ##

* 计算仿真程序中时，会话相似性不起作用。 设置可应用于计算仿真程序而不会干扰您的生成过程，或计算仿真程序执行，但该功能不起作用计算仿真程序内。
* 启用会话关系将导致如将下载额外的软件，并将其安装到角色实例，Azure 云启动您的服务时在 Azure，部署占用的磁盘空间量的增加。
* 初始化每个角色的时间会更长。
* 将 added.ss 作为通信 rerouter 如上所述，内部端点。

有关如何启用会话关系时保留会话数据的示例，请参阅[如何维护与会话关联的会话数据][]。

## <a name="see-also"></a>请参见 ##

[日蚀式的 azure Toolkit][]

[Azure 在 Eclipse 中创建一个 Hello World 应用程序][]

[日蚀式安装 Azure Toolkit][] 

[如何维护与会话关联的会话数据][]

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure 在 Eclipse 中创建一个 Hello World 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699533
[如何维护与会话关联的会话数据]: http://go.microsoft.com/fwlink/?LinkID=699539
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png
