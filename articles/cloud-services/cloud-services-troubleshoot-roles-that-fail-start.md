<properties
   pageTitle="排除故障无法启动的角色 |Microsoft Azure"
   description="以下是一些常见的原因为什么一个云服务角色可能会启动失败。 此外提供了对这些问题的解决方案。"
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

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>排除故障无法启动的云服务角色

以下是一些常见的问题和解决方案相关的 Azure 云服务不能启动的角色。

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>缺少的 Dll 或依赖项

无响应的角色和**初始化**、**忙**和**停止**的状态之间循环的角色可能致缺少 Dll 或程序集中。

缺少 Dll 或程序集中的症状可以是︰

- 角色实例循环**初始化**、**忙**和**停止**状态。
- 角色实例已移动到**准备就绪，**但如果您导航到 web 应用程序，则不显示页。

有几种建议的方法来研究这些问题。

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>诊断中 web 角色的缺少 DLL 问题

当您导航到网站已部署的 web 角色和浏览器显示类似于下面的服务器错误，它可能指示 DLL 丢失。

![/ 应用程序中的服务器错误。](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>通过关闭自定义错误诊断问题

可以通过 web 角色的自定义错误模式设置为 Off web.config 配置和重新部署该服务查看错误的详细信息。

若要查看详细的错误而无需使用远程桌面︰

1. 在 Microsoft Visual Studio 中打开该解决方案。

2. 在**解决方案资源管理器**中定位到 web.config 文件并打开它。

3. 在 web.config 文件中，找到 system.web 部分，添加以下行︰

    ```xml
    <customErrors mode="Off" />
    ```

4. 保存该文件。

5. 重新打包，并重新部署该服务。

一旦服务被重新部署，您将看到错误消息与缺少程序集或 DLL 的名称。

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>通过查看错误远程诊断问题

您可以使用远程桌面访问角色和远程查看错误的详细信息。 使用以下步骤以查看错误使用远程桌面︰

1. 确保安装了 Azure 1.3 或更高版本的 SDK。

2. 在部署期间使用 Visual Studio 解决方案，选择"配置远程桌面连接..."。 配置远程桌面连接的详细信息，请参阅[使用 Azure 角色使用的远程桌面](../vs-azure-tools-remote-desktop-roles.md)。

3. 在 Microsoft Azure 经典门户中，一旦实例演示的**准备就绪**状态，请单击角色实例之一。

4. 单击功能区的**远程访问权限**区域中的**连接**图标。

5. 通过使用远程桌面配置过程中指定的凭据登录到虚拟机。

6. 打开一个命令窗口。

7. 类型`IPconfig`。

8. 请注意的 IPV4 地址值。

9. 打开 Internet Explorer。

10. 键入地址和 web 应用程序的名称。 例如， `http://<IPV4 Address>/default.aspx`。

导航到该网站现在将返回更明确的错误消息︰

* / 应用程序中的服务器错误。

* 说明︰ 执行当前 web 请求期间出现未处理的异常。 请检查堆栈跟踪有关该错误以及其原始代码中的位置的详细信息。

* 异常详细信息︰ System.IO.FIleNotFoundException︰ 无法加载文件或程序集 Microsoft.WindowsAzure.StorageClient、 版本 = 1.1.0.0，区域性程序 = 31bf856ad364e35 或其中一个依赖项。 系统无法找到指定的文件。

例如︰

![明确的服务器 / 应用程序中的错误](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>通过计算仿真诊断问题

可以使用 Microsoft Azure 计算仿真程序来诊断和排查缺少依赖项和 web.config 错误的问题。

在使用这种诊断方法获得最佳效果，应使用计算机或虚拟机，干净的 Windows 安装。 为了最好地模拟 Azure 的环境，使用 Windows Server 2008 R2 x64。

1. 安装独立的[Azure SDK](https://azure.microsoft.com/downloads/)版本。

2. 开发计算机上，生成云服务项目。

3. 在 Windows 资源管理器中，导航到云服务项目的 bin\debug 文件夹。

4. .Csx 文件夹和.cscfg 文件复制到您要用来调试问题的计算机。

5. 在清理计算机上，打开 Azure SDK 命令提示符窗口并键入`csrun.exe /devstore:start`。

6. 在命令提示符下，键入`run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`。

7. 角色在启动时，您将看到在 Internet Explorer 中的详细的错误消息。 此外可以使用标准的 Windows 故障排除工具来进一步诊断此问题。

## <a name="diagnose-issues-by-using-intellitrace"></a>通过使用 IntelliTrace 诊断问题

工作人员和 web 角色使用.NET Framework 4 中，您可以使用[IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx)，可在[Microsoft Visual Studio 终极](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs)。

请按照以下步骤启用的 intellitrace 部署服务︰

1. 确认安装了 Azure 1.3 或更高版本的 SDK。

2. 通过使用 Visual Studio 的部署解决方案。 在部署期间，检查**.NET 4 角色启用 IntelliTrace**复选框。

3. 该实例启动后，打开**服务器资源管理器**。

4. 展开**Azure\\云服务**节点，然后找到部署。

5. 直到看到角色实例展开部署。 右键单击其中一个实例。

6. 选择**视图 IntelliTrace 日志**。 将会打开**IntelliTrace 摘要**。

7. 找到异常部分中的摘要。 如果出现异常，部分将被标记为**异常数据**。

8. 展开**异常数据**，看起来类似于下面的**System.IO.FileNotFoundException**错误︰

![异常数据、 缺少文件或程序集](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>解决缺少 Dll 和程序集

为了解决缺少 DLL 和程序集的错误，请执行以下步骤︰

1. 在 Visual Studio 中打开该解决方案。

2. 在**解决方案资源管理器中**，打开**引用**文件夹。

3. 请单击错误信息中标识的程序集。

4. 在**属性**窗格中，找到**复制本地属性**并将值设置为**True**。

5. 重新部署云服务。

验证已更正所有错误后，请您可以部署服务而无需检查**.NET 4 角色启用 IntelliTrace**复选框。

## <a name="next-steps"></a>下一步行动

查看更多的[疑难解答文章](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services)的云服务。

若要了解如何通过使用 Azure PaaS 计算机诊断数据解决云服务的角色问题，请参阅[古柯 Williamson 博客系列](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)。
