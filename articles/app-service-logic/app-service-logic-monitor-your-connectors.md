<properties
    pageTitle="管理和监视您的连接器和 API 在应用程序服务的应用程序 |Microsoft Azure"
    description="查看性能连接器和 API 应用程序中的逻辑应用程序;microservices 的体系结构"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>管理和监视您的内置 API 的应用程序和连接器

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2014年-12-01-预览架构版本。

您创建了一个内置的 API 应用程序。 现在怎么办？

在 Azure，每个 API 的应用程序是托管在 Azure 上单独的网站。 因此，您可以轻松看到多少请求进行，以及有多少数据正在使用连接器。 您可以备份您的 API 应用程序、 创建警报、 启用锡箔安全和添加用户和角色。

本主题介绍了一些不同的选项来管理您的 API 应用程序。

若要查看这些内置功能，打开在[Azure 门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)您 API 的应用程序。 如果 API 应用程序位于您的 startboard，选择以打开属性。 可以选择**浏览**、 选择**API 的应用程序**，然后选择您的 API 应用程序︰

![][browse]

## <a name="see-the-properties-you-entered"></a>您输入的属性，请参阅

当您打开 API 的应用程序时，有几个功能和任务︰

![][settings]

您可以：

- **设置**API 应用程序中，您的订阅的详细信息，包括显示的特定信息，并列出有权访问您的 API 应用程序的用户。 您还可以增加或减少您的 API 应用程序使用缩放功能; 实例数在其他功能。
- 使用**开始**和**停止**按钮来控制 API 的应用程序。
- 当产品更新所使用的 API 的应用程序基础文件时，您可以单击**更新**以获取最新版本。 例如，如果没有 Microsoft 发布的安全更新或修复程序，单击**更新**自动更新您的 API 应用程序包含此修补程序。
- 选择**更改规划**升级或降级基于 API 的应用程序的数据使用情况。 您可以使用此功能来查看您的数据的使用情况。
- 当您创建连接器具有表，如 SQL 接口，可以选择输入要连接到的表名。 根据表的架构时，会自动创建并可供单击**下载架构**。 然后可以使用此下载的架构创建一个转换或映射。

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>更改您的接口或 API 您输入的配置值

配置或创建构建连接器后，您可以更改您输入的值。 例如，如果配置 SQL 接口，并且您想要更改的 SQL Server 名称或表名称，你可以刀片式服务器 API 应用程序中为您的连接器。

包括以下步骤︰

1. 打开您的接口或 API 的应用程序。 执行操作时，将打开刀片式服务器 API 的应用程序。
2. 在**精要**在主机属性中单击超链接。 超链接是*slackconnector*或*microsoftsqlconnector123*等取名称︰

    ![][apiapphost]

3. 在 API 的应用程序主机刀片式服务器，选择**设置**。 在设置刀片式服务器，选择**应用程序设置**。 在**应用程序设置**下，列出了您配置的值︰

    ![][hostsettings]

4. 单击您想要更改，请输入新值，并**将保存**您更改的设置。


## <a name="install-the-hybrid-connection-manager---optional"></a>安装混合连接管理器-可选

![][hcsetup]

混合连接管理器使您能够连接到内部部署的系统，如 SQL Server 或 SAP。 此混合连接使用 Azure 服务总线来连接并控制之间 Azure 资源和内部资源的安全。

请参阅[使用 Azure 应用程序服务中混合连接管理器](app-service-logic-hybrid-connection-manager.md)。

> [AZURE.NOTE] 仅当您要连接到的内部资源位于防火墙后面，混合连接管理器是必需的。 如果您未连接到内部系统，混合连接管理器可能没有列入连接器刀片。

## <a name="monitor-the-performance"></a>监视性能
性能指标是内置功能，包括您创建的每个 API 应用程序使用。 这些指标是特定于您的 API 在 Azure 中承载的应用程序。 一些衡量标准示例︰

![][monitoring]

您可以：

- 选择**请求和错误**添加不同的性能指标包括常为人所知的 HTTP 错误代码，如 200、 400 或 500 的 HTTP 状态代码。 此外可以看到响应时间、 看到多少请求对 API 的应用程序，并查看多少数据后，数据量熄灭。 根据性能指标，如果一个指标超过某个阈值选择的可以创建电子邮件通知。
- 在**使用**中，您可以看到多少**CPU**使用的 API 的应用程序、 查看当前**使用率配额**以 mb 为单位，并了解您基于成本层的最大数据使用情况。 **预计花费**可以帮助您确定运行 API 的应用程序的潜在成本。
- 选择要打开进程浏览器的**进程**。 这将显示您的网站实例和它们的属性，包括线程计数和内存使用情况。

使用这些工具，您可以确定是否应用程序服务计划应放大或缩小，根据您的业务需求。 这些功能是无需其他工具具有内置到门户网站。

## <a name="control-the-security"></a>安全控件

API 应用程序使用基于角色的安全性。 这些角色适用于整个 Azure 的经验，包括 API 的应用程序和其他 Azure 的资源。 角色包括︰

角色 | 说明
--- | ---
所有者 | 具有完全访问权限的管理经验，可以为其他用户或组授予访问。
参与者 | 具有完全访问权限的管理经验。 不能允许其他用户或组访问。
读取器 | 可以查看所有资源除机密信息。
用户访问管理员 | 所有资源、 创建管理角色，可以都查看和创建管理支持票据。

请参阅[Microsoft Azure 门户中基于角色的访问控制](../active-directory/role-based-access-control-configure.md)。

您很容易可以添加用户并将它们分配给您的 API 应用程序特定的角色。 门户显示您具有访问和其分配的角色的用户︰

![][access]  

- 选择要添加用户、 指定角色，和删除用户的**用户**。
- 选择要查看属于某个特定角色的所有用户的**角色**将用户添加到角色，从角色中删除用户。


## <a name="more-good-stuff"></a>更多的好东西
- 选择要打开自动创建的 Swagger 文件进行特定的 API 应用程序的**API 定义**。
- 选择**依赖项**以查看您的 API 应用程序所需的文件。 例如，如果您使用的 SAP 连接器，安装其他一些文件在内部混合连接管理器。 这些依赖项显示在您的 API 的应用程序刀片式服务器。

>[AZURE.IMPORTANT] 当您打开您的 API 的应用程序属性并看看下面**精要**时有打开新的刀片服务器的**主机**和**网关**链接︰
>
> ![][host]
>
>这些属性是特定于宿主 API 应用程序的网站。 使用内置的 API 的应用程序或连接器时，这些属性中的大多数真的不适用，我们建议您不更新这些属性。 如果您在 Visual Studio 中创建您自己的 API 应用程序并将其部署到 Azure 订购，您可以使用的主机和网关的薄片。 <br/><br/>


>[AZURE.NOTE] 入门的逻辑应用程序在 Azure 帐户注册之前，转到[重试逻辑应用程序](https://tryappservice.azure.com/?appservice=logic)。 您可以创建一个短期初学者逻辑应用程序。 没有所需的信用卡并没有承诺。

## <a name="read-more"></a>阅读更多

[监视您的逻辑应用程序](app-service-logic-monitor-your-logic-apps.md)<br/>
[连接器和应用程序服务中的 API 的应用程序列表](app-service-logic-connectors-list.md)<br/>
[在 Microsoft Azure 门户的基于角色的访问控制](../active-directory/role-based-access-control-configure.md)<br/>
[在 Azure 应用程序服务使用混合连接管理器](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
