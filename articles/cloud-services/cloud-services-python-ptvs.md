<properties
    pageTitle="与 Visual Studio 的 Python 站点和辅助角色 |Microsoft Azure"
    description="使用 Visual Studio 的 Python 工具创建 Azure 的云服务，其中包括 web 角色和辅助角色的概述。"
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>使用 Visual Studio 的 Python 工具的 Python web 和工作角色

这篇文章概述了使用 Python 站点和辅助角色使用[Visual Studio 的 Python 工具][]。 您将学习如何使用 Visual Studio 创建和部署使用 Python 的基本的云服务。

## <a name="prerequisites"></a>系统必备组件

 - Visual Studio 2013年或 2015
 - [Visual Studio 的 Python 工具][](PTVS)
 - [VS 2013 的 azure SDK 工具][]或[VS 2015 的 Azure SDK 工具][]
 - [Python 2.7 32 位][]或者[Python 3.5 32 位][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Python 的 web 和工作人员的角色是什么？

Azure 提供了三个计算模型，用于运行应用程序︰[在 Azure 应用程序服务 Web 应用程序功能][execution model-web sites]， [Azure 虚拟机][execution model-vms]，和[Azure 云服务][execution model-cloud services]。 所有三个型号都支持 Python。 云服务，包括网站和辅助的角色，提供*平台即服务 (PaaS)*。 在云服务中，web 角色提供专用的 Internet Information Services (IIS) web 服务器主机前端 web 应用程序，而工作者角色可以运行异步、 长期或永久任务独立于用户交互或输入。

有关详细信息，请参阅[云服务是什么？]。

> [AZURE.NOTE]*希望构建一个简单的网站吗？*
如果您的方案涉及的只是简单网站前端，请考虑使用 Azure 应用程序服务中的轻量的 Web 应用程序功能。 您可以轻松地升级到云服务，为您的网站的发展和需求的变化。 请参阅<a href="/develop/python/">Python 开发者中心</a>的文章涵盖了 Azure 应用程序服务中的 Web 应用程序功能的开发。
<br />


## <a name="project-creation"></a>创建项目

在 Visual Studio 中，您可以选择在**新建项目**对话框中的**Python**， **Azure 云服务**。

![新建项目对话框](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

在 Azure 云服务向导中，您可以创建新的 web 和辅助角色。

![Azure 的云服务对话框](./media/cloud-services-python-ptvs/new-service-wizard.png)

辅助角色模板附带样板化的代码连接到 Azure 存储帐户或 Azure 服务总线。

![云服务解决方案](./media/cloud-services-python-ptvs/worker.png)

可以在任何时候添加到现有的云服务的 web 或辅助角色。  您可以选择在您的解决方案中添加现有项目或创建新模板。

![添加角色命令](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

云服务可以包含用不同的语言实现的角色。  例如，您可以使用 Python，或 C# 辅助角色使用 Django，实现了 Python web 角色。  您可以轻松地使用服务总线队列存储您角色之间进行通信。

## <a name="install-python-on-the-cloud-service"></a>在云服务上安装 Python

>[AZURE.WARNING] 安装脚本使用 Visual Studio 安装 （在这篇文章最后更新的时间） 不起作用。 本节介绍了一种解决方法。

安装脚本的主要问题是，他们不安装 python。 首先，定义在[ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef)文件中的两个[启动任务](cloud-services-startup-tasks.md)。 第一项任务 (**PrepPython.ps1**) 下载并安装 Python 运行时。 第二项任务 (**PipInstaller.ps1**) 运行 pip 安装可能具有的任何依赖项。

下面的脚本编写面向 Python 3.5。 如果您想要使用的版本 2.x 的 python，设置**PYTHON2**变量文件为**在**这两个启动任务和运行时任务︰ `<Variable name="PYTHON2" value="<mark>on</mark>" />`。


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

**PYTHON2**和**PYPATH**变量需要将添加到工作人员启动任务。 如果变量**PYTHON2**设置为**on**，才使用**PYPATH**变量。

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>ServiceDefinition.csdef 示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



下一步，创建中的**PrepPython.ps1**和**PipInstaller.ps1**文件**。 /bin**文件夹的角色。

#### <a name="preppythonps1"></a>PrepPython.ps1

此脚本安装 python。 如果**PYTHON2**环境变量设置为将安装**上**然后 Python 2.7，否则将安装 Python 3.5。

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

此脚本 pip 会调用，并且在**requirements.txt**文件中安装所有依赖项。 如果**PYTHON2**环境变量设置为将使用**上**然后 Python 2.7，否则将使用 Python 3.5。

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>修改 LaunchWorker.ps1

>[AZURE.NOTE] 对于一个**辅助角色**项目中， **LauncherWorker.ps1**文件需要执行启动文件。 在**web 角色**项目中，启动文件而是在项目属性中定义的。

**Bin\LaunchWorker.ps1**最初创建做了大量准备工作，但它实际上不起作用。 在该文件中的内容替换为下面的脚本。

此脚本从 python 项目调用**worker.py**文件。 如果**PYTHON2**环境变量设置为将使用**上**然后 Python 2.7，否则将使用 Python 3.5。

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>ps.cmd

Visual Studio 模板应已创建的**ps.cmd**文件中**。 /bin**文件夹。 此外壳脚本出上面的 PowerShell 包装脚本调用，并提供基于 PowerShell 包装调用的名称的日志记录。 如果没有创建此文件，下面是它应该是什么。 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>在本地运行

如果将云服务项目设置为启动项目并按 f5 键，云服务将运行在本地 Azure 仿真程序。

虽然 PTVS 支持在仿真器中，调试 （例如，断点） 启动不起作用。

若要调试 web 和辅助角色，可以角色项目设置为启动项目并调试，相反。  您还可以设置多个启动项目。  用鼠标右键单击该解决方案，然后选择**设置启动项目**。

![解决方案启动项目属性](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>发布到 Azure

若要发布，请右击解决方案中的云服务项目，然后选择**发布**。

![Microsoft Azure 发布登录](./media/cloud-services-python-ptvs/publish-sign-in.png)

按照向导的说明。 如果需要可以启用远程桌面。 当您需要调试代码时，远程桌面将非常有用。

配置设置完成后，单击**发布**。

一些进度将显示在输出窗口中，然后您将看到 Microsoft Azure 活动日志窗口。

![Microsoft Azure 活动日志窗口](./media/cloud-services-python-ptvs/publish-activity-log.png)

部署将需要几分钟才能完成，然后将对 Azure 上运行 web 和/或辅助角色 ！

### <a name="investigate-logs"></a>检查日志

云服务虚拟机启动和安装 Python 之后，您可以查看日志，以查找任何失败消息。 这些日志不位于**C:\Resources\Directory\\{角色} \LogFiles**文件夹。 **PrepPython.err.txt**将有至少一个错误中从该脚本尝试检测如果 Python 安装和**PipInstaller.err.txt**可能会抱怨 pip 版本已过时。

## <a name="next-steps"></a>下一步行动

有关使用 Visual Studio 的 Python 工具中的 web 和辅助角色的详细信息，请参阅 PTVS 文档︰

- [云服务项目][]

有关使用 web 和工作人员角色，例如使用 Azure 存储或服务总线从 Azure 服务的更多详细信息请参阅以下文章。

- [Blob 服务][]
- [表服务][]
- [队列的服务][]
- [服务总线队列][]
- [服务总线主题][]


<!--Link references-->

[云服务是什么？]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Blob 服务]: ../storage/storage-python-how-to-use-blob-storage.md
[队列的服务]: ../storage/storage-python-how-to-use-queue-storage.md
[表服务]: ../storage/storage-python-how-to-use-table-storage.md
[服务总线队列]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[服务总线主题]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Visual Studio 的 Python 工具]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[云服务项目]: http://go.microsoft.com/fwlink/?LinkId=624028
[VS 2013 的 azure SDK 工具]: http://go.microsoft.com/fwlink/?LinkId=323510
[VS 2015 的 azure SDK 工具]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32 位]: https://www.python.org/downloads/
[Python 3.5 32 位]: https://www.python.org/downloads/
