<properties
    pageTitle="PHP web 和辅助角色 |Microsoft Azure"
    description="在 Azure 的云服务，创建 PHP web 和辅助角色和 PHP 运行时配置的指南。"
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>如何创建 PHP web 和员工角色

## <a name="overview"></a>概述

本指南将向您介绍如何在 Windows 开发环境中创建 PHP 网站或辅助角色，从可用的"内置"版本中选择特定版本的 PHP、 更改 PHP 配置、 启用扩展，并最后，将部署到 Azure。 它还介绍了如何配置要使用 （与自定义配置和扩展） PHP 运行您所提供的 web 或工作人员的角色。

## <a name="what-are-php-web-and-worker-roles"></a>PHP web 和员工角色是什么？

Azure 提供了三个计算运行应用程序的模型︰ Azure 应用程序服务、 Azure 虚拟机和 Azure 云服务。 所有三个型号都支持 PHP。 云服务，包括网站和辅助的角色，提供*平台即服务 (PaaS)*。 在云服务中，web 角色提供专用的 Internet Information Services (IIS) web 服务器主机前端 web 应用程序。 辅助角色可以运行异步、 长期或永久任务独立于用户交互或输入。

有关这些选项的详细信息，请参阅[计算提供的 Azure 的宿主选项](./cloud-services/cloud-services-choose-me.md)。

## <a name="download-the-azure-sdk-for-php"></a>下载 php 的 Azure SDK

[PHP 的 Azure SDK]包含几个组件。 这篇文章将使用其中的两个︰ Azure PowerShell 和 Azure 仿真程序。 这两个组件可以通过 Microsoft Web 平台安装程序安装。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。

## <a name="create-a-cloud-services-project"></a>创建一个云服务项目

创建 PHP 网站或辅助角色的第一步是创建一个 Azure 服务项目。 Azure 服务项目作为一种逻辑容器，用于 web 和辅助的角色，并包含项目的[服务定义 (.csdef)]和[服务 (.cscfg) 配置]文件。

若要创建新的 Azure 服务项目，以管理员身份运行 Azure PowerShell 并执行以下命令︰

    PS C:\>New-AzureServiceProject myProject

此命令将创建一个新目录 (`myProject`)，您可以向其中添加 web 和员工角色。

## <a name="add-php-web-or-worker-roles"></a>添加 PHP web 或辅助角色

若要向项目中添加 PHP web 角色，运行下面的命令从项目的根目录中︰

    PS C:\myProject> Add-AzurePHPWebRole roleName

为辅助角色中，使用以下命令︰

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] `roleName`参数是可选的。 如果省略它，则将自动生成的角色名称。 创建的第一个 web 角色将为`WebRole1`，将第二个`WebRole2`，依此类推。 将创建的第一个工作人员角色`WorkerRole1`，将是第二个`WorkerRole2`，依此类推。

## <a name="specify-the-built-in-php-version"></a>指定内置 PHP 版本

当您向项目中添加 PHP 网站或辅助角色时，以便在部署时，将应用程序的每个网站或辅助实例上安装 PHP 修改项目的配置文件。 若要查看 PHP 将默认安装的版本，请运行以下命令︰

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

上面的命令的输出将类似于下面显示的内容。 在此示例中，`IsDefault`标志设置为`true`表示，它将安装的默认 PHP 版本的 php 5.3.17。

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

您可以设置为任何列出的 PHP 版本 PHP 运行时版本。 例如，若要设置 PHP 版本 (的角色名称`roleName`) 对含有 5.4.0，使用下面的命令︰

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] 可用的 PHP 版本可能会在将来更改。

## <a name="customize-the-built-in-php-runtime"></a>自定义内置 PHP 运行时

您可以完全控制当您按照上面的步骤，其中包括修改已安装的 PHP 运行时配置`php.ini`设置和启用的扩展。

若要自定义内置 PHP 运行时，请执行以下步骤︰

1. 添加新文件夹，命名为`php`到`bin`目录的 web 角色。 辅助角色，请将其添加到该角色的根目录下。
2. 在`php`文件夹中，创建另一个文件夹名为`ext`。 放入任何`.dll`扩展名的文件 (例如， `php_mongo.dll`) 想要在此文件夹中启用。
3. 添加`php.ini`文件为`php`文件夹。 启用任何自定义扩展，并在此文件中设置的任何 PHP 指令。 例如，如果您希望将`display_errors`上并启用`php_mongo.dll`扩展了的内容您`php.ini`文件应如下︰

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] 您没有显式设置中的任何设置`php.ini`自动提供将文件设置为其默认值。 但是，请记住，您可以添加一个完整`php.ini`文件。

## <a name="use-your-own-php-runtime"></a>使用您自己的 PHP 运行时
在某些情况下，而不是选择一个内置 PHP 运行时和配置它上面所述，您可以提供您自己的 PHP 运行时。 例如，可以在开发环境中使用 web 或辅助角色中使用同一个 PHP 运行时。 这使得更容易以确保应用程序不会更改在您的生产环境中的行为。

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>配置以使用您的 PHP 运行时 web 角色

若要配置要使用 PHP 运行时您所提供的 web 角色，请执行以下步骤︰

1. 创建 Azure 服务项目并添加 PHP web 角色，如本主题中前面所述。
2. 创建`php`文件夹中的`bin`文件夹，再在 web 角色的根目录，然后添加到您的 PHP 运行 （所有二进制文件、 配置文件、 子文件夹等）`php`文件夹。
3. （可选）如果您的 PHP 运行时使用[Microsoft SQL Server 的 PHP 驱动程序][sqlsrv drivers]，您需要配置您的 web 角色安装[SQL Server 本机客户端 2012年][sql native client]配置时。 要做到这一点，添加到[sqlncli.msi x64 安装程序]`bin`在您的 web 角色根目录下的文件夹。 下一步中所述的启动脚本将以静默方式运行安装程序时配置该角色。 如果您的 PHP 运行时不使用 Microsoft 驱动程序的 PHP 的 SQL Server，您可以用下一步中所示的脚本删除以下行︰

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. 定义启动任务配置[Internet Information Services (IIS)] [iis.net]以 PHP 运行用于处理请求的`.php`页。 若要执行此操作，请打开`setup_web.cmd`文件 (在`bin`的 web 角色的根目录下的文件) 在文本编辑器中并将其内容替换为下面的脚本︰

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. 将应用程序文件添加到 web 角色的根目录下。 这将是 web 服务器的根目录。

6. 发布您的应用程序，如下面的[发布您的应用程序](#how-to-publish-your-application)一节中所述。

> [AZURE.NOTE] `download.ps1`脚本 (在`bin`的 web 角色的根目录下的文件夹) 使用您的 PHP 运行时上述步骤之后，可以删除。

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>配置以使用您的 PHP 运行时辅助角色

若要配置要使用 PHP 运行时您所提供的工作人员角色，请执行以下步骤︰

1. 创建 Azure 服务项目并添加 PHP 辅助角色，如本主题中前面所述。
2. 创建`php`文件夹根目录中，辅助角色，然后将您的 PHP 运行 （所有二进制文件、 配置文件、 子文件夹等） 添加到`php`文件夹。
3. （可选）如果您的 PHP 运行时将使用[Microsoft SQL Server 的 PHP 驱动程序][sqlsrv drivers]，您需要配置您的工作角色安装[SQL Server 本机客户端 2012年][sql native client]配置时。 若要执行此操作，请向辅助角色的根目录下添加[sqlncli.msi x64 安装程序]。 下一步中所述的启动脚本将以静默方式运行安装程序时配置该角色。 如果您的 PHP 运行时不使用 Microsoft 驱动程序的 PHP 的 SQL Server，您可以用下一步中所示的脚本删除以下行︰

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. 定义启动任务添加您`php.exe`可执行到辅助角色的 PATH 环境变量时配置角色。 若要执行此操作，请打开`setup_worker.cmd`（中辅助角色的根目录下） 在文本编辑器中的文件和其内容替换为下面的脚本︰

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. 将应用程序文件添加到辅助角色的根目录下。

6. 发布您的应用程序，如下面的[发布您的应用程序](#how-to-publish-your-application)一节中所述。

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>在计算和存储模拟器中运行应用程序

Azure 仿真程序提供本地环境中可以测试 Azure 应用程序之前将其部署到云。 有模拟器和 Azure 的环境之间的一些差异。 若要更好地理解这一点，请参阅[使用 Azure 存储仿真程序的开发和测试](./storage/storage-use-emulator.md)。

请注意，您必须具有本地要使用计算仿真程序安装的 PHP。 计算仿真程序将使用您本地的 PHP 安装运行您的应用程序。

在仿真程序中运行您的项目，从项目的根目录执行下面的命令︰

    PS C:\MyProject> Start-AzureEmulator

您将看到类似于下面的输出︰

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

您可以查看应用程序在仿真程序中运行打开 web 浏览器，并浏览到本地地址显示在输出中 (`http://127.0.0.1:81`在上面的示例输出)。

若要停止对仿真程序，请执行以下命令︰

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>发布您的应用程序

若要发布您的应用程序，您需要首先导入您通过[导入 AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet 的发布设置。 然后您可以通过使用[发布 AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet 发布您的应用程序。 有关登录的信息，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。

## <a name="next-steps"></a>下一步行动

有关详细信息，请参见[PHP 开发人员中心](/develop/php/)。

[Php 的 azure SDK]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[服务定义 (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[服务配置 (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 安装程序]: http://go.microsoft.com/fwlink/?LinkID=239648
