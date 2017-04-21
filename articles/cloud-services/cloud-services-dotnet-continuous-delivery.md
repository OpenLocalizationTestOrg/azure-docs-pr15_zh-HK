<properties
    pageTitle="不断提供云服务使用 TFS Azure 中 |Microsoft Azure"
    description="了解如何设置 Azure 的云应用程序的连续传递。 MSBuild 命令行语句和 PowerShell 脚本代码示例。"
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>不断在 Azure 的云服务的提供

本文中介绍的过程演示了如何设置 Azure 的云应用程序的连续传递。 此过程使您能够自动创建包并将包部署到 Azure 之后每个代码签入。 本文介绍软件包生成过程等效于在 Visual Studio 时，**打包**命令并发布步骤都等价于在 Visual Studio 中的**发布**命令。
文章介绍的方法将用于创建使用 MSBuild 命令行语句和 Windows PowerShell 脚本生成服务器，它还演示如何配置 Visual Studio Team Foundation Server-团队生成定义以使用 MSBuild 命令和 PowerShell 脚本 （可选）。 是可以自定义您的生成环境和 Azure 目标环境的过程。

您还可以使用 Visual Studio 的团队服务，为承载在 Azure，可以更轻松地执行此操作的 TFS 版本。 有关详细信息，请参阅[连续传递到 Azure 使用 Visual Studio 的团队服务][]。

在开始之前，您应发布 Visual Studio 应用程序。
这将确保所有资源都都可用且初始化时尝试自动发布过程。

## <a name="1-configure-the-build-server"></a>1︰ 配置生成服务器

您可以通过使用 MSBuild 创建 Azure 包之前，您必须生成服务器上安装所需的软件和工具。

不需要生成服务器上安装 Visual Studio。 如果您想要使用团队基础生成服务来管理您的生成服务器，请按照[团队基础生成服务][]文档中的说明进行操作。

1.  在生成服务器上，安装了[.NET Framework 4.5.2][]，其中包括 MSBuild。
2.  安装最新的[Azure.NET 的创作工具](https://azure.microsoft.com/develop/net/)。
3.  安装[.net Azure 库](http://go.microsoft.com/fwlink/?LinkId=623519)。
4.  从 Visual Studio 安装的 Microsoft.WebApplication.targets 文件复制到生成的服务器。

    在计算机上安装 Visual Studio，此文件位于该目录为 c:\\程序 Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications。 您应将其复制到生成服务器上的同一目录中。
5.  安装[Visual Studio Azure 的工具](https://www.visualstudio.com/features/azure-tools-vs.aspx)。

## <a name="2-build-a-package-using-msbuild-commands"></a>2︰ 生成软件包使用 MSBuild 命令

本部分介绍如何构造生成 Azure 包 MSBuild 命令。 生成服务器，以验证一切都已正确配置并 MSBuild 命令，执行您的预期来操作上执行此步骤。 可以将此命令行添加到现有的生成脚本在生成服务器上，也可以使用命令行在 TFS 构建定义中，在下一节中所述。 有关命令行参数和 MSBuild 的详细信息，请参阅[MSBuild 命令行参考](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx)。

1.  如果在生成服务器上安装 Visual Studio，找到并选择**Visual Studio 的命令提示**窗口中的**Visual Studio 工具**文件夹中。

    如果在生成服务器上未安装 Visual Studio，打开一个命令提示符并确定 MSBuild.exe 路径可以访问。 MSBuild 安装.NET Framework 中路径 %WINDIR%\\Microsoft.NET\\框架\\*版本*。 例如，要将 MSBuild.exe 添加到 PATH 环境变量时必须安装.NET Framework 4 中，请在命令提示符下键入以下命令︰

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  在命令提示符下，导航到包含要生成的 Azure 项目文件的文件夹。

3.  通过 /target 运行 MSBuild︰ 发布选项，如以下示例所示︰

        MSBuild /target:Publish

    此选项可以缩写为 /t︰ 发布。 Azure SDK 安装后，不应与在 Visual Studio 中可用的发布命令混淆在 MSBuild 的 /t:Publish 选项。 /T︰ 发布 Azure 包的选项只生成。 在 Visual Studio 中的发布命令一样，它不会不部署程序包。

    （可选） 您可以指定项目名称作为 MSBuild 参数。 如果未指定，则使用当前目录。 MSBuild 命令行选项的详细信息，请参阅[MSBuild 命令行参考](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx)。

4.  查找输出。 默认情况下，此命令将创建的目录相对于根文件夹中的项目，如*ProjectDir*\\bin\\*配置*\\app.publish\\。 在 Azure 项目生成时，将生成两个文件、 包文件本身和附带的配置文件︰

    -   Project.cspkg
    -   服务配置。*TargetProfile*.cscfg

    默认情况下，每个 Azure 项目包括为本地 （调试） 版本，另一个用于云 （临时或生产） 生成一个服务配置文件 （.cscfg 文件），但您可以添加或删除服务配置文件，根据需要。 生成在 Visual Studio 中的包时，将要求您的服务配置文件以包含包的旁边。

5.  指定的服务配置文件。 通过使用 MSBuild 生成包时，默认情况下包含本地服务配置文件。 要将不同的服务配置文件，设置 TargetProfile 属性的 MSBuild 命令，如以下示例所示︰

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  指定的输出位置。 将路径设置通过使用 /p:PublishDir =*目录*\\选项，其中包括尾部的反斜杠分隔符，如以下示例所示︰

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    一旦您已经构建和测试适当的 MSBuild 命令行生成项目并将它们组合到 Azure 的包，您可以添加此命令行生成脚本。 如果您的生成服务器使用自定义脚本，此过程将取决于您的自定义生成过程的细节。 如果您正在使用 TFS 作为生成环境，您可以按照下一步中的说明进行操作，将 Azure 包生成添加到您的生成过程。

## <a name="3-build-a-package-using-tfs-team-build"></a>3︰ 生成软件包使用 TFS 团队项目生成

如果您有 Team Foundation Server (TFS) 设置为生成控制器和生成服务器设置为 TFS 生成计算机，然后您可以选择设置了自动构建 Azure 包。 有关如何设置和使用 Team Foundation 服务器作为生成系统的信息，请参阅[您的生成系统的扩张][]。 特别是，以下过程假定您已配置您的生成服务器中所述[部署和配置生成服务器][]，，您可以创建一个团队项目，在团队项目中创建一个云服务项目。

若要配置 TFS 构建 Azure 的软件包，请执行以下步骤︰

1.  在 Visual Studio 开发计算机上，在视图菜单中选择**团队资源管理器**，或选择 Ctrl +\\，Ctrl + M。 在团队资源管理器窗口中，展开**生成**节点或选择**生成**页并选择**新建生成定义**。

    ![新的生成定义选项][0]

2.  选择**触发器**选项卡，然后指定所需的条件，当您想要生成软件包时。 例如，指定**持续集成**生成软件包时源控件发生签入。

3.  选择**源设置**选项卡，并确保您的项目文件夹列出在**源代码管理文件夹**列中，并处于**活动**状态。

4.  选择**生成默认值**选项卡，并在生成控制器，请验证生成服务器的名称。  另外，选择**复制生成输出复制到以下放置文件夹**选项并指定所需的放置位置。

5.  选择**进程**选项卡。 在进程选项卡上选择的默认模板，**生成**下，如果它尚未被选中，并展开网格的**高级**部分中的**生成**部分，请选择项目。

6.  选择**MSBuild 参数**，并在上面的步骤 2 中所述设置适当的 MSBuild 命令行参数。 例如，输入**/t︰ 发布 /p:PublishDir =\\\\myserver\\下降\\**以生成软件包并将包文件复制到的位置\\ \\myserver\\下降\\:

    ![MSBuild 参数][2]

    **注意︰**将文件复制到一个公用的共享便于手动部署从开发计算机软件包。

5.  通过签入更改的项目，测试成功的生成步骤或新生成进行排队。 要排队等候新的版本，在团队资源管理器中，**所有生成定义，**用鼠标右键单击，然后选择**使新生成入队**。

## <a name="4-publish-a-package-using-a-powershell-script"></a>4︰ 发布使用 PowerShell 脚本包

本部分介绍如何构建一个 Windows PowerShell 脚本，它会发布 Azure 使用可选参数对云应用程序的包输出。 生成步骤中将自定义生成自动化后，可以调用此脚本。 它也可以称为从 Visual Studio TFS 团队生成过程模板工作流活动。

1.  安装[Azure PowerShell cmdlet][] (v0.6.1 或更高版本)。
    在 cmdlet 安装阶段选择安装作为管理单元。 请注意此正式支持的版本替换旧版本 CodePlex，通过提供，虽然以前版本的编号为 2.x.x。

2.  开始使用开始菜单的 Azure PowerShell 或开始页。 如果以这种方式启动时，将加载 Azure PowerShell cmdlet。

3.  PowerShell 提示时，请验证输入部分命令是否加载了 PowerShell cmdlet `Get-Azure` ，然后按 Tab 键以完成语句。

    如果您反复按 Tab 键，您应该看到各种 Azure PowerShell 命令。

4.  验证您可以通过从.publishsettings 文件导入您的订阅信息连接到 Azure 订购。

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    然后输入命令

    `Get-AzureSubscription`

    这将显示有关您的订购信息。 验证所有信息正确。

4.  保存到脚本文件夹为 c︰ 这篇文章末尾提供的脚本模板\\脚本\\WindowsAzure\\**PublishCloudService.ps1**。

5.  查看参数部分中的脚本。 添加或修改任何默认值。 始终可以通过显式参数传入覆盖这些值。

6.  确保在您的订阅，可以通过发布脚本目标中创建有效的云服务和存储帐户。 存储帐户 （blob 存储） 将用于上载并在创建部署时临时存储部署包和配置文件。

    -   若要创建新的云服务，可以调用该脚本或使用[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)。 云服务名称将用作一个完全限定的域名称中的前缀，因此它必须是唯一的。

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   若要创建新的存储帐户，可以调用该脚本或使用[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)。 存储的帐户名称将用作一个完全限定的域名称中的前缀，因此它必须是唯一的。 您可以尝试在云服务中使用相同的名称。

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  直接从 Azure PowerShell 调用脚本或此脚本绑定到主机的生成自动化软件包生成后发生。

    >[AZURE.IMPORTANT] 该脚本将始终删除或替换现有部署默认情况下，如果检测到它们。 这是必要的以实现从自动化连续传递在没有用户提示有可能。

    **示例方案 1:**连续部署到临时环境的服务︰

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    这通常是由测试运行验证和 VIP 交换后续工作。 可以通过[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)或通过移动部署 cmdlet VIP 交换。

    **示例方案 2:**连续部署到生产环境的一种专用的测试服务

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **远程桌面︰**

    如果您将需要执行其他一次性的措施，来确保正确的云服务证书上载到此脚本的目标的所有云服务的 Azure 项目中启用了远程桌面。

    找到您的角色所需的证书指纹值。 证书云配置文件 (即 ServiceConfiguration.Cloud.cscfg) 的一节中的指纹值是可见的。 它也会出现在 Visual Studio 中的远程桌面配置对话框显示时选项和查看所选证书。

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    作为一次性设置步骤使用以下 cmdlet 脚本上载远程桌面证书︰

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    例如︰

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    或者可以导出带有私钥的证书文件 PFX，并将证书传到每个目标云服务使用[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)。 
    阅读以下文章以了解详细信息: [http://msdn.microsoft.com/library/windowsazure/gg443832.aspx] []。

    **升级部署与删除部署-\>新部署**

    该脚本将默认执行升级的部署 ($enableDeploymentUpgrade = 1) 时不传入任何参数或显式传递值 1。 对于单个实例这有花更少的时间比完全部署的优点。 对于升级要求高可用性，还其优点的离开而其他运行的一些实例的实例 （遍历更新域），再加上您 VIP 不会被删除。

    可以在脚本中禁用升级部署 ($enableDeploymentUpgrade = 0) 或通过作为参数传递*的 enableDeploymentUpgrade 0* ，这些改变要首先删除任何现有的部署，然后创建新部署的脚本行为。

    >[AZURE.IMPORTANT] 该脚本将始终删除或替换现有部署默认情况下，如果检测到它们。 这是必要的以实现从自动化连续传递其中无用户/操作员提示是可能。

## <a name="5-publish-a-package-using-tfs-team-build"></a>5︰ 发布包使用 TFS 团队项目生成

此可选步骤连接 TFS 团队生成到步骤 4 中创建的脚本处理到 Azure 的包版本的发布。 这就需要修改过程模板由您的生成定义，以便使其在工作流的末尾运行的发布活动。 发布活动将执行 PowerShell 命令从生成参数中传递。 MSBuild 的输出目标并发布脚本将被输送到标准生成输出。

1.  编辑生成定义负责连续部署。

2.  选择**进程**选项卡。

3.  按照[这些指令](http://msdn.microsoft.com/library/dd647551.aspx)将活动项目的生成过程模板添加、 下载默认模板、 将其添加到该项目并将其签入。 使生成过程模板的新名称，如 AzureBuildProcessTemplate。

3.  返回到**进程**选项卡，然后使用**显示详细信息**以显示可用的生成过程模板的列表。 选择**新建...**按钮，然后导航到您刚刚添加并签入该项目。 找到的模板，您只需创建并选择**确定**。

4.  打开用于编辑所选的过程模板。 您可以打开直接在工作流设计器中或在 XML 编辑器中使用 XAML。

5.  作为单独的行项工作流设计器的参数选项卡中添加下面的新的参数列表。 所有的参数应具有方向 = 中，键入 = 字符串。 这将用流参数通过生成定义到工作流，然后获取用于调用发布脚本。

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![参数列表][3]

    相应的 XAML 如下所示︰

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  在代理上运行的末尾添加一个新的序列︰

    1.  通过添加一个 If 语句活动来检查有有效的脚本文件来启动。 将条件设置为此值︰

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  在那种情况下的 If 语句中，添加一个新的系列活动。 设置为开始发布显示名称

    3.  与开始发布仍处于选中状态的序列中，作为单独的行项工作流设计器的变量选项卡中添加以下新的变量列表。 所有变量都应该都具有变量类型 = 字符串和范围 = 开始发布。 这将用流参数通过生成定义到工作流，然后获取用于调用发布脚本。

        -   SubscriptionDataFilePath，属于字符串类型

        -   PublishScriptFilePath，属于字符串类型

            ![新变量][4]

    4.  如果您正在使用 TFS 2012 或更早版本，新序列的开始处添加 ConvertWorkspaceItem 活动。 如果您正在使用 TFS 2013 或以后，新序列的开始处添加一个 GetLocalPath 的活动。 对于 ConvertWorkspaceItem，设置属性，如下所示︰ 方向 = ServerToLocal，显示名称 = 转换发布脚本文件名，请输入 = 'PublishScriptLocation'，结果 = 'PublishScriptFilePath'，工作区 = 工作区。 对于 GetLocalPath 的活动，将属性设置为 'PublishScriptLocation'，IncomingPath 和结果与 PublishScriptFilePath。 此活动从 TFS 服务器位置 （如果适用） 为标准的本地磁盘路径将路径转换为发布脚本。

    5.  如果您正在使用 TFS 2012 或更早版本，新序列的结尾处添加另一个 ConvertWorkspaceItem 活动。 方向 = ServerToLocal，显示名称 = 转换订阅文件名，输入 = 'SubscriptionDataFileLocation'，结果 = 'SubscriptionDataFilePath'，工作区 = 工作区。 如果正在使用 TFS 2013 或更高版本中，添加另一个 GetLocalPath。 IncomingPath = SubscriptionDataFileLocation，并将结果 = 'SubscriptionDataFilePath'。

    6.  新序列的结尾处添加 InvokeProcess 活动。
        此活动由生成定义传入的参数调用 PowerShell.exe。

        1.  参数 = String.Format ("的文件""{0}""服务-{1}-storageAccountName {2}-packageLocation""{3}""-cloudConfigLocation""{4}""-subscriptionDataFile""{5}""-selectedSubscription {6}-环境""{7}"""，PublishScriptFilePath、 服务名、 StorageAccountName、 PackageLocation、 CloudConfigLocation、 SubscriptionDataFilePath、 SubscriptionName、 环境)

        2.  显示名称 = 执行发布脚本

        3.  文件名 ="PowerShell"（包括引号）

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  在 InvokeProcess 的**处理标准输出**部分文本框中，文本框将值设置为数据。 这是一个变量来存储标准输出数据。

    8.  添加 WriteBuildMessage 活动的**处理标准输出**部分的下方。 设置重要性 = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' 和消息 = '数据'。 这可确保生成输出将写入标准输出的脚本。

    9.  在**处理错误输出**部分文本框中的 InvokeProcess，文本框将值设置为数据。 这是一个变量来存储标准错误数据。

    10. 添加 WriteBuildError 活动**处理错误输出**部分的下方。 设置消息 = '数据'。 这样可以确保将到生成错误输出写入标准错误的脚本。

    11. 更正所有错误，由蓝色的惊叹号。 将鼠标悬停在惊叹号，以获取有关错误的提示。 保存工作流，以清除错误。

    发布工作流活动的最终结果将如下所示在设计器中︰

    ![工作流活动][5]

    在 XAML 中，发布工作流活动的最终结果看起来如下︰

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  保存的生成过程模板的工作流和签入此文件。

8.  编辑生成定义 （如果已打开它关闭），并选择**新建**按钮，如果您看不到新模板的过程模板列表中。

9.  设置参数属性值中的杂项部分，如下所示︰

    1.  CloudConfigLocation = c:\\下降\\app.publish\\ServiceConfiguration.Cloud.cscfg'*从派生此值: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation = c:\\下降\\app.publish\\ContactManager.Azure.cspkg'*从派生此值: ($PublishDir)($ProjectName).cspkg*

    3.  PublishScriptLocation = c:\\脚本\\WindowsAzure\\PublishCloudService.ps1

    4.  服务 = mycloudservicename*使用适当的云服务名称*

    5.  环境 = '临时'

    6.  StorageAccountName = mystorageaccountname*使用适当的存储帐户名*

    7.  SubscriptionDataFileLocation = c:\\脚本\\WindowsAzure\\Subscription.xml

    8.  SubscriptionName = 默认值

    ![参数属性值][6]

10. 将更改保存到生成定义。

11. 执行包生成并发布对生成进行排队。 如果设置为持续集成触发器后，您将在每次签入时执行此行为。

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 脚本模板

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>下一步行动

若要启用远程调试，使用连续的传递时，请参阅[启用远程调试时使用连续传递发布到 Azure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md)。

  [连续传递到 Azure 使用 Visual Studio 的团队服务]: cloud-services-continuous-delivery-use-vso.md  
  [团队的基础生成服务]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [扩展您的生成系统]: https://msdn.microsoft.com/library/dd793166.aspx
  [部署和配置生成服务器]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure PowerShell cmdlet]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
