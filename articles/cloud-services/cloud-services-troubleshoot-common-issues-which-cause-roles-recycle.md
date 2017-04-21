<properties
   pageTitle="常见的原因的云服务角色回收 |Microsoft Azure"
   description="一个云服务角色，突然回收可能会导致大量的停机时间。 以下是一些常见的问题，导致角色要回收，这可能有助于减少停机时间。"
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>常见的问题，导致回收的角色

本文讨论了一些部署问题的常见原因，并提供故障排除的提示，以帮助您解决这些问题。 表明应用程序存在问题时，角色实例无法启动，或将在初始化、 忙、 和停止的状态之间循环。

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>缺少运行时依赖关系

如果您的应用程序中的角色依赖于任何程序集，则不属于.NET Framework 或 Azure 托管库，您必须显式包含该程序集应用程序包中。 请记住其他 Microsoft 框架不是 Azure 上默认情况下是可用的。 如果您的角色依赖于这样的一个框架，您必须将这些程序集添加到应用程序包。

在构建和打包您的应用程序之前，请验证以下各项︰

- 如果使用 Visual studio，请确保为每个引用的程序集不是 Azure SDK 或.NET Framework 的一部分您项目中的**复制本地**属性设置为**True** 。

- 请确保 web.config 文件未引用任何未使用的程序集编译元素中。

- 每个.cshtml 文件的**生成操作**设置为**内容**。 这将确保文件将正确显示在包中，并使其他引用的文件在包中出现。

## <a name="assembly-targets-wrong-platform"></a>程序集的目标平台不正确

Azure 是一个 64 位环境。 因此，.NET 程序集编译为 32 位目标就不能在 Azure。

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>角色在初始化或停止时引发未经处理的异常

[RoleEntryPoint]类，后者包括[OnStart]、 [OnStop]和[运行]的方法，则方法引发的任何异常都是未经处理的异常。 如果未处理的异常发生在以下其中一个方法，将回收的角色。 如果角色反复循环使用，它可能会引发未处理的异常每当它试图启动。

## <a name="role-returns-from-run-method"></a>从 Run 方法返回角色

[Run]方法将无限期地运行。 如果您的代码重写[方法]，应无限期地休眠。 [Run]方法返回时，如果该角色回收。

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>DiagnosticsConnectionString 设置不正确

如果应用程序使用 Azure 诊断，必须指定您的服务配置文件`DiagnosticsConnectionString`配置设置。 此设置应在 Azure 中指定 HTTPS 连接到您的存储帐户。

若要确保您`DiagnosticsConnectionString`设置是正确的您的应用程序包部署到 Azure 之前，请验证以下︰  

- `DiagnosticsConnectionString`到 Azure 的有效存储帐户设置点。  
  默认情况下，此设置点到仿真的存储帐户，以便在部署您的应用程序包之前，必须显式更改此设置。 如果不更改此设置，当尝试启动诊断监视器角色实例时引发异常。 这可能会导致无限循环的角色实例。

- 按以下[格式](../storage/storage-configure-connection-string.md)指定的连接字符串。 （必须指定协议作为 HTTPS。）*MyAccountName*替换为您的存储帐户和*MyAccountKey*与您的访问密钥的名称︰    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  如果您要通过使用 Microsoft Visual Studio Azure 工具开发您的应用程序，您可以使用[属性页](https://msdn.microsoft.com/library/ee405486)设置此值。

## <a name="exported-certificate-does-not-include-private-key"></a>导出的证书不包含专用密钥

若要运行在 SSL 的 web 角色，您必须确保管理导出的证书，包括私钥。 如果您使用*Windows 证书管理器*导出证书，请确保**导出私钥**的选项选择**是**。 必须以 PFX 格式，即当前支持的唯一格式导出证书。

## <a name="next-steps"></a>下一步行动

查看更多的[疑难解答文章](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services)的云服务。

查看回收情况在[古柯 Williamson 博客系列](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)的多个角色。

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[运行]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
