<properties
    pageTitle="基于角色的访问控件疑难解答 |Microsoft Azure"
    description="获得帮助的问题或有关基于角色的访问控制资源的问题。"
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>基于角色的访问控制的故障排除

## <a name="introduction"></a>介绍

[基于角色的访问控制](role-based-access-control-configure.md)是一项强大的功能，从而可以委托精细到 Azure 中的资源的访问。 这意味着可以感觉自信使用完全他们需要什么，权限授予特定的人员和没有更多。 但是，有时 Azure 的资源的资源模型可能非常复杂，很难理解完全什么您要授予的权限。

本文将让您知道要使用 Azure 门户中的某些角色。 以下三种角色涵盖所有资源类型︰

- 所有者  
- 参与者  
- 读取器  

所有者和参与者都具有完全访问权限的管理体验，但参与者不能给其他用户或组授予访问。 事情获得具有读者角色，更有趣，这就是，我们将花一些时间。 有关如何授予访问权限，请参阅[基于角色的访问控制的入门文章](role-based-access-control-configure.md)以了解详细信息。

## <a name="app-service-workloads"></a>应用程序服务工作负荷

### <a name="write-access-capabilities"></a>写访问能力

如果您授予用户只读访问单个 web 应用程序时，某些功能被禁用，您可能不希望。 以下管理功能需要到 web 应用程序 （参与者或所有者） 的**写**访问权，并且不能在任何只读的方案。

- 命令 （例如启动、 停止等）
- 更改设置，如常规配置、 刻度设置、 备份的设置和监视设置
- 访问发布的凭据和类似的应用程序设置和连接字符串的其他机密
- 流式传输日志
- 诊断日志配置
- 控制台 （命令提示符）
- （对于本地 git 连续部署） 的活动和最近部署
- 估计花
- Web 测试
- 虚拟网络 （只看到读者如果以前已具有写访问权限的用户通过配置虚拟网络）。

如果您不能访问任何这些图块，需要参与者访问 web 应用程序询问您的管理员联系。

### <a name="dealing-with-related-resources"></a>处理相关资源

Web 应用程序是很复杂的几个不同的资源之间相互关系的存在。 以下是一些网站的典型的资源组︰

![Web 应用程序的资源组](./media/role-based-access-control-troubleshooting/website-resource-model.png)

因此，如果您授权某人访问只是使 web 应用程序，很多在 Azure 门户网站刀片式服务器上的功能将被禁用。

这些项目需要**写入**访问对应到您的网站的**应用程序服务计划**︰  

- 查看 web 应用程序的定价层 （闲或标准）  
- 扩展配置 （实例的数目、 虚拟机大小、 自动缩放设置）  
- 配额 （存储、 带宽、 CPU）  

这些项目需要对整个**资源组**，其中包含您的网站**写**访问权︰  

- （这是因为 SSL 证书可以共享同一资源组中的网站和地理位置之间） 的 SSL 证书和绑定  
- 预警规则  
- 自动缩放设置  
- 应用程序的见解组件  
- Web 测试  

## <a name="virtual-machine-workloads"></a>虚拟机的工作负载

更像 web 应用程序与虚拟机刀片上的某些功能需要写访问权限到虚拟机，或其他资源的资源组中。

虚拟机与域的名称、 虚拟网络、 存储帐户和预警规则。

这些项目需要**写入**访问**虚拟机**︰

- 终结点  
- IP 地址  
- 磁盘  
- 扩展  

这些要求对**虚拟机**和**资源组**（及域名），它是在**写**访问权︰  

- 可用性设置  
- 负载平衡的设置  
- 预警规则  

如果您不能访问任何这些图块，您需要请求资源组参与者访问您的管理员联系。

## <a name="see-more"></a>详细信息，请参阅
- [基于角色的访问控制](role-based-access-control-configure.md)︰ 开始使用 RBAC 在 Azure 的门户。
- [内置角色](role-based-access-built-in-roles.md)︰ 获得有关标准在 RBAC 角色的详细信息。
- [在 Azure RBAC 中的自定义角色](role-based-access-control-custom-roles.md)︰ 学习如何创建自定义角色以满足您的访问需求。
- [创建访问变更历史记录报告](role-based-access-control-access-change-history-report.md)︰ 跟踪的 RBAC 中不断变化的角色分配。
