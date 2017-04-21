<properties
    pageTitle="如何安装和配置 Azure PowerShell"
    description="了解如何安装和配置 Azure PowerShell。"
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>如何安装和配置 Azure PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>什么是 Azure PowerShell？
Azure PowerShell 是一组提供 cmdlet 来管理与 Windows PowerShell Azure 的模块。 可以使用 cmdlet 创建、 测试、 部署和管理的解决方案和通过 Azure 平台提供的服务。 在大多数情况下，这些 cmdlet 可以用于与 Azure 门户，例如，创建和配置云服务、 虚拟机、 虚拟网络和 web 应用程序相同的任务。

## <a name="how-versioning-works"></a>版本控制的工作原理

Azure PowerShell 使用版本控制语义，这意味着如果一个版本 > 版本 B，然后一个版本包含最新的 Api。 同时，它表明中一个或多个 cmdlet，更改主要版本意味着重大的变化。  因此，例如，版本 1.7.0 是一个修补程序才能解决的 Azure PowerShell 1.x 版本中的重大更改。

在 Azure PowerShell 语义版本管理做法有关的详细信息，请参阅在语义版本控制指标︰ http://semver.org
 
若要获取最新的 Api，您应使用版本 2.x。 但是，如果您已编写的脚本版本对照 1.x 和您不想吸收版中的新的重大更改 2.x 然后安装 1.7.0 2.x[发行说明](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md)中所述。

如果最新版本的配置文件模块已经安装，并且随后加载依赖于它的模块的早期版本，则会导致版本不匹配。 要解决此问题的最简单方法是从最新的.msi 安装。 .Msi 文件自动清理旧版本的模块。
 
###<a name="installing-module-versions-side-by-side"></a>安装模块版本通过并行

2.1.0 版 （AzureStack 1.2.6 版） 是设计用于安装的第一个模块版本，使用并排比较。 由于 Azure PowerShell 使用二进制模块，必须打开一个新窗口，PowerShell 和**导入模块**用于导入的 AzureRM cmdlet 的特定版本︰

    Import-Module AzureRM -RequiredVersion 2.1.0**

版本 2.1.0 （而不是 1.2.6) 不起作用之前也与其他 Azure PowerShell 模块版本并行。 加载时使用上面的命令的 Azure PowerShell 模块的早期版本，不兼容版本的**AzureRM.Profile**模块将会加载，从而导致要求您登录的 cmdlet 每当执行 cmdlet，即使您已登录。

最简单的方法来解决这是为了从 WebPI 安装最新的 Azure PowerShell 送或.msi – 这会删除早期版本的库中安装的模块。 

请注意，Azure 和 AzureRM 模块依赖项共同点，因此，如果您使用两个模块，一个更新时，则应更新两个。 Azure 模块的早期版本有并排比较模块加载 AzureRM 模块的早期版本具有相同问题。

<a id="Install"></a>
## <a name="step-1-install"></a>步骤 1︰ 安装

以下是您可以依据安装 Azure PowerShell 的两种方法。 可以将其从 PowerShell 库或 WebPI 安装。

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>从 PowerShell 库安装 Azure PowerShell

首选的方法是使用 PowerShell 库。 您需要使用 PowerShell 图库的 PowerShellGet 模块。 这是这里提供︰ [PowerShellGallery.com](https://www.powershellgallery.com/)

安装 Azure PowerShell 1.3.0 或更高版本从 PowerShell 图库提升的 Windows PowerShell 或 PowerShell 集成脚本环境 (ISE) 提示符下使用以下命令︰

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>有关这些命令的详细信息

- **安装模块 AzureRM**安装 Azure 资源管理器 cmdlet 的汇总模块。 AzureRM 模块依赖于 
- Azure 资源管理器中的每个模块的特定版本范围。 包含的版本范围可确保使用相同的主版本中安装 AzureRM 模块时，可以导致无重大模块更改包括。 AzureRM 模块安装时，以前未安装任何 Azure 资源管理器模块将被下载并安装从 PowerShell 库。 Azure PowerShell 模块所使用的语义版本控制的详细信息，请参阅[semver.org](http://semver.org)。 
- **安装模块 Azure**安装 Azure 的模块。 此模块是从 Azure PowerShell 服务管理模块 0.9.x。 这应该会否主要更改是可互换的 Azure 的模块的早期版本。

###<a name="installing-azure-powershell-from-webpi"></a>从 WebPI 安装 Azure PowerShell

从 WebPI 安装 Azure PowerShell 1.0 和更高版本是相同的 0.9.x 的样子。 下载[Azure PowerShell](http://aka.ms/webpi-azps)并开始安装。 如果您有 Azure PowerShell 0.9.x 安装、 版本 0.9.x 将作为升级的一部分被卸载。 如果您从 PowerShell 库安装 Azure PowerShell 模块，安装程序将自动删除之前安装以确保一致的 Azure PowerShell 环境模块。

> [AZURE.NOTE] 如果您以前从 PowerShell 库安装了 Azure 模块，安装程序会将它们自动删除。 有关哪些模块版本已经安装并且位于何处，这样可以避免混淆。 **%ProgramFiles%\WindowsPowerShell\Modules**将正常安装 PowerShell 库模块。 与此相反，WebPI 安装程序将安装 Azure 的模块中含有**%programfiles (x86) %\Microsoft SDKs\Azure\PowerShell\**。如果在安装过程中发生错误，您可以手动删除 Azure*文件夹在您的**%ProgramFiles%\WindowsPowerShell\Modules**文件夹中，并尝试再次安装。

当安装完成后时，您```$env:PSModulePath```设置应包括包含 Azure PowerShell cmdlet 的目录。

> [AZURE.NOTE] 没有一个已知的问题，使用 PowerShell **$env: PSModulePath**从 WebPI 安装时可发生。 如果您的计算机需要因系统更新或其他安装而重新启动，可能会导致更新**$env: PSModulePath** ，不包括 Azure PowerShell 的安装位置的路径。 如果发生这种情况，您可能尝试在安装或升级后使用 Azure PowerShell cmdlet 时看到无法识别的 cmdlet 消息。 如果发生这种情况，则重新启动计算机来解决该问题。

如果试图加载或执行的 cmdlet，您会收到类似下面这样的消息︰

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

这可以通过重新启动计算机或从 C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ 导入这些 cmdlet，按如下更正 (其中 XXXX 是 PowerShell 安装的版本︰

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>第 2 步︰ 开始
使用标准 Windows PowerShell 控制台，或从 PowerShell 集成脚本环境 (ISE)，您可以运行这些 cmdlet。
打开任一控制台所使用的方法取决于您运行的 Windows 的版本︰

- 至少运行的计算机上 Windows 8 或 Windows Server 2012，您可以使用内置的搜索功能。 从**开始**屏幕上，开始输入电源。 此方法返回指定了作用域的应用程序列表，其中包含 Windows PowerShell。 若要打开控制台，请单击任一应用程序。 （若要固定到**开始**屏幕的应用程序，右击图标。）

- 在计算机上运行的版本早于 Windows 8 或 Windows Server 2012，使用**开始菜单**。 从**开始**菜单中，单击**所有程序**、**附件**、 **Windows PowerShell**文件夹，然后都单击**Windows PowerShell**。

您还可以运行**Windows PowerShell ISE**使用菜单项和键盘快捷键执行很多的会在 Windows PowerShell 控制台中执行的相同任务。 若要使用 ISE，时在 Windows PowerShell 控制台中，Cmd.exe，或者在**运行**框中，键入， **powershell_ise.exe**。

###<a name="commands-to-help-you-get-started"></a>命令来帮助您入门

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>第 3 步︰ 连接
这些 cmdlet 需要订购，因此他们可以管理您的服务。 如果您没有，您可以购买 Azure 的订阅。 有关说明，请参阅[如何购买 Azure](http://go.microsoft.com/fwlink/p/?LinkId=320795)。

1. 类型**登录 AzureRmAccount**

2. 键入的电子邮件地址和密码与您的帐户相关联。 Azure 进行身份验证并保存的凭据信息，然后关闭该窗口。

-或者-

登录到您的工作或学校的帐户︰

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] 如果您有多个租户与您组织的帐户，指定 TenantId 参数︰

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] 此非交互式登录方法仅适用于工作或学校的帐户中。 工作或学校的帐户是由您的工作或学校，并为您的工作或学校的 Azure Active Directory 实例中定义的用户。 如果当前没有一个工作或学校的账户，并使用 Microsoft 帐户登录到 Azure 订购，您可以轻松创建一个使用下列步骤。

> 1. 登录到[Azure 的传统门户网站](https://manage.windowsazure.com)，然后单击**活动目录**。

> 2. 如果不存在目录，选择**创建您的目录**，并提供所需的信息。

> 3. 选择您的目录，并添加新的用户。 该新用户可以登录使用的工作或学校的帐户。 在用户创建后，您将提供用户和临时密码的电子邮件地址。 保存此信息，下面 5 步骤中使用它。

> 4. 从 Azure 的传统门户网站，选择**设置**，然后选择**管理员**。 选择**添加**，并作为共同管理员添加新的用户。 这样的工作或学校的帐户管理 Azure 订购。

> 5. 最后，从 Azure 的传统门户网站注销，然后重新登录使用工作或学校科目。 如果是第一次登录此帐户，系统将提示您更改密码。

> 注册 Microsoft Azure 的工作或学校的帐户的详细信息，请参阅[Microsoft Azure 作为一个组织注册](./active-directory/sign-up-organization.md)。

> 在 Azure 中的身份验证和订阅管理有关的详细信息，请参阅[管理帐户、 预订和管理角色](http://go.microsoft.com/fwlink/?LinkId=324796)。

### <a name="view-account-and-subscription-details"></a>查看帐户和申请详细信息

您可以有多个帐户和可供使用 Azure PowerShell 的订阅。 通过多次运行**添加 AzureRmAccount** ，您可以添加多个帐户。

若要显示可用的 Azure 帐户，请键入**获取 AzureAccount**。

若要显示您 Azure 的订阅内容，请键入**获取 AzureRmSubscription**。

##<a id="Help"></a>获取帮助##

这些资源为特定的 cmdlet 提供帮助︰


-   从在控制台中，您可以使用内置的帮助系统。 **获取帮助**cmdlet 提供访问此系统。 

- 来自社区的帮助，请尝试这些热门的论坛︰

 - [在 MSDN 上的 azure 论坛]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>了解更多信息


请参见下列资源以了解有关使用 cmdlet:

有关使用 Windows PowerShell 的基本说明，请参阅[使用 Windows PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=321939)。

有关这些 cmdlet 的参考信息，请参阅[Azure Cmdlet 参考](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx)。

示例脚本并帮助您了解如何使用脚本来管理 Azure 的说明，请参阅[脚本中心](http://go.microsoft.com/fwlink/p/?LinkId=321940)。

