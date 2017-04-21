<properties
    pageTitle="Azure 角色属性"
    description="了解如何使用 Eclipse 的 Azure Toolkit Azure 角色的设置。"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Azure 角色属性 #

可以在 Eclipse 的 Azure Toolkit 内设置 Azure 角色的各种配置设置。

## <a name="configuring-azure-role-properties"></a>配置 Azure 角色属性 ##

配置您的 Azure 角色属性被通过辅助角色的属性对话框。 在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单并选择**Azure**子菜单。 （如果看不到 Eclipse 项目资源管理器中的角色，展开项目资源管理器中的项目内 Azure。）

![][ic789599]

本主题介绍可以从**属性**对话框中设置的各种属性。 请注意，许多属性自动填充时创建新的 Azure 的部署项目。

下面的属性页是 Azure 角色可用的。

* [虚拟机属性](#virtual_machine_properties)
* [缓存属性](#caching_properties)
* [证书属性](#certificates_properties)
* [组件属性](#components_properties)
* [调试属性](#debugging_properties)
* [终结点属性](#endpoints_properties)
* [环境变量属性](#environment_variables_properties)
* [负载平衡 / 会话关联 (a.k.a"粘滞会话") 属性](#session_affinity_properties)
* [本地存储属性](#local_storage_properties)
* [服务器配置属性](#server_configuration_properties)
* [SSL 卸载属性](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>虚拟机属性 ###

如下图所示，该角色在 Eclipse 的项目资源管理器窗格中，单击**Azure**，，然后单击**属性**，并将能够更改虚拟机大小，并更改实例的数目、 打开上下文菜单。

![][ic719499]

>[AZURE.NOTE] 仅限 Windows︰ 将实例数设置的值大于 1，并且您还可以配置应用程序服务器，该工具包将允许仅 1 个角色实例运行在仿真器中，无论该设置如何。 这是为了避免端口绑定不同的服务器实例之间的冲突 （例如，所有试图绑定到端口 8080） 时它们在同一台计算机上运行。 保留您所需的实例计数设置，但仅当您将部署到云进入效果。

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>缓存属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单， **Azure**，请单击，然后单击**缓存**。 在此对话框中，您可以启用命名并存的还兼容缓存，从而可以有助于提高 web 应用程序的速度。

![][ic719483]

**缓存**属性页面中，您可以指定下面的全局设置︰

* 是否位于缓存已启用。
* 高速缓存大小的内存的百分比。
* 如果您不想保存缓存状态为云服务，或不运行应用程序时保存缓存状态存储帐户名。 （存储的帐户名称不是使用计算仿真程序中运行应用程序时。如果将存储帐户名设置为**（自动）** （这是默认设置），您的缓存配置将自动使用相同的存储帐户与您在**发布到 Azure**对话框中选择的一个。

>[AZURE.NOTE] **（自动）**设置将具有所需的效果只有在发布您的部署使用 Eclipse 工具包发布向导。 如果发布的.cspkg 文件，手动使用外部机制，如[Azure 管理门户网站][]，改为部署将不能正常工作。

下面的对话框显示缓存的属性。

![][ic719501]

* **名称︰**并存的缓存的名称。
* **端口号︰**要为缓存使用的端口号。
* **过期策略︰**以下值之一，指定缓存中的项时到期。
    * **绝对︰**密钥过期时达到的时间由**分钟才能生存**。
    * **NeverExpires:**密钥没有过期时间。
    * **SlidingWindow:**密钥过期，如果不按**分钟才能生存**; 指定的时间内访问每次访问它时，会重置到期时钟。
* **分钟才能生存︰**最大 memcached 键来居住，如有的过期策略的分钟数。
* **高可用性功能，不同的角色实例上的已复制备份︰**如果启用，则帮助提供高可用性利用复制不同角色实例上的备份。 请注意，至少两个角色实例必须有效使用该功能为您的部署。

若要添加新的缓存，请单击**缓存**属性页面中的**添加**按钮并**配置命名缓存**对话框将打开。 上面描述的属性提供值。

若要修改一个已命名的高速缓存，缓存中选择，单击**缓存**属性页面的**编辑**按钮。 将允许您修改缓存属性打开一个对话框。 请按**确定**以保存缓存的值。

删除缓存，选择缓存和单击**缓存**属性页面中的**删除**按钮，然后单击**是**以确认删除。

有关如何使用缓存的详细信息，请参阅[如何使用 Co-located 缓存][]。

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>证书属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单，单击**Azure**，，然后单击**证书**。

![][ic710964]

在此对话框中，您可以添加或删除您的 Eclipse 项目所引用的证书。 请注意，此处列出的证书不会自动存储在任何 Java 密钥库中，因此不会自动提供从 Java 应用程序中的任何使用。 他们只注册 Azure，以便它们可以被预先加载到 Windows 证书将存储在运行您的部署的虚拟机并随后使用由其他 Windows 软件。 目前，该工具包使用**证书**对话框中的此方式引用的证书的唯一功能是[SSL 卸载][]，由于依靠 Internet Information Services (IIS) 和应用程序请求路由 (ARR)，需要适当的证书可供以这种方式。

当您将项目部署到 Azure 使用发布向导时，将提示您指向对应这些证书，和他们的密码，以便自动将其上载到 Azure 服务，但是仅当它们尚未上载那里以前的个人信息交换 (PFX) 文件。

<a name="components_properties"></a> 
### <a name="components-properties"></a>组件属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单，单击**Azure**，，然后单击**组件**。 在此对话框中，可以添加、 修改或删除您的角色，这些组件以及更改的顺序进行处理的能力。

![][ic719502]

组件功能可以将依赖关系添加到 Azure 的部署项目中，如 Java 应用程序项目、 特殊文件和可执行文件的命令行语句所需的部署。

对于每个组件，您可以指定︰

* 当将组件导入到 Azure 的部署项目，生成它时要采取的步骤。
* 部署在 Azure 云该组件时所采取的步骤。

>[AZURE.NOTE] 当指定组件文件或命令行，请注意您的部署将会发布到 Windows 虚拟机时，因此必须有效为基于 Windows 的操作系统自定义步骤。 

组件具有以下属性︰

* **导入︰**指示如何组件将被导入项目时生成的项目的方法。 这可以是下列值之一︰
    * **副本︰**从**From**属性指定到该角色的**approot**目录的本地路径复制组件。
    * **耳︰**该组件是从**From**属性指定的本地路径在企业应用程序项目中导入一个 Java 企业存档 (EAR)。 （这是自动检测工具包根据该位置的项目的性质）。
    * **JAR:**组件是 Java 归档文件 (JAR)，并**从**属性指定的本地路径在 Java 项目中导入。 （这是自动检测工具包根据该位置的项目的性质）。
    * **无︰**不执行任何操作以导入组件。 这是适用的则假定该组件可能已存在于该角色的**approot**目录中，或者当组件是只是一个可执行文件的命令行语句，按**为**属性指定的**部署**方法时**执行**。
    * **战争︰**组件是一个 Java 的 web 应用程序归档 (WAR)，并从**From**属性指定的本地路径在一个动态 Web 项目导入。 （这是自动检测工具包根据该位置的项目的性质）。
    * **压缩︰**组件是一个 zip 文件，并通过压缩目录或文件**的**属性所指定的导入。
* **从︰**代表要导入到您的部署的项的文件的文件夹到本地计算机上的源路径。 在此属性中，可以使用 Windows 环境变量。 生成项目时，可导入的所有组件将导都入到该角色的**approot**目录。
    
    注意您可以部署中下载的组件部署到云 （不计算仿真程序） 时的能力。 请参阅下面有关添加组件的相关的信息。    
    
* **As:**在其下导入到该角色的**approot**目录并最终在 Azure 的云部署组件的文件名。 将此属性保留为空以保留名称相同是本地计算机上。 （对于可执行组件，即，那些其**部署**方法设置为**执行**，这可以是任意 Windows 命令行语句。

    >[AZURE.IMPORTANT] 如果此值使用空格字符，它们将进行处理以不同的方式取决于部署方法。 如果部署方法， **exec**空格将被用作命令行参数分隔符而不是文件名的一部分。 对于所有其他部署方法，空格将被解释为文件名的一部分。
    
* **部署︰**指示操作启动部署时应用到组件的方法。 这可以是下列值之一︰
    * **副本︰**该组件将被复制到**To**属性所指定的目标路径。
    * **执行︰**组件是路径的可执行 Windows 命令行语句的**To**属性中，指定在部署启动的时的上下文中执行。
    * **无︰**部署时，任何操作不应用到组件中。
    * **压缩︰**该组件是解压缩到**To**属性所指定的目标路径。 仅**导入**属性是**zip**时，可用此方法。
* **To:**在虚拟机上部署该组件的位置的目标路径。 可以在此属性中，使用 Windows 环境变量和文件路径是相对于**approot**。
    
若要添加新组件，请单击**添加**按钮**组件**属性页中的和**Azure 角色组件**对话框将打开。 上面描述的属性提供值。 

下面举例说明如何添加新的战争组件。

![][ic719503]

在部署到云 （不计算仿真程序），如果您想要将部署从一个下载组件时，请确保，**云，而不是在包中，包括在部署中**处于选中状态。 如果要下载从 Azure 存储帐户，选择存储帐户**存储帐户**下拉列表 （您可以单击要修改列表中的**帐户**链接），从部分将在**URL**字段中，将其中填满，然后填写该 URL 的其余部分。 如果您不想使用 Azure 存储，**存储帐户**下拉列表中，选择**（无）** ，您的组件在**URL**字段中输入的 URL。 指定下列方法之一︰

* **副本︰**下载组件将被复制到**的目录**路径指定的目标路径。
* **相同︰****从下载部署****程序包中的部署**与使用相同的方法。
* **压缩︰**下载组件是解压缩到**的目录**路径指定的目标路径。

要修改某个组件，请选择组件，然后单击**组件**属性页中的**编辑**按钮。 将打开一个对话框使您可以修改组件的属性。 请按**确定**以保存组件的值。

要删除组件，请选择该组件并单击**组件**属性页中的**删除**按钮然后单击**是**以确认删除。

将按列出的顺序处理组件。 使用**上移**和**下移**按钮排列顺序。

>[AZURE.NOTE] 服务器配置功能依赖组件。 不能删除或编辑而不删除相应的服务器配置这些组件。 将提示有关，当试图对这些组件进行更改。

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>调试属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单，单击**Azure**，，然后单击**调试**。 在此对话框中，您可以启用或禁用远程调试，以及创建调试配置，如下图中所示。

![][ic719504]

有关调试的相关信息，请参阅[调试 Eclipse 的 Azure 应用程序][]。

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>终结点属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单， **Azure**，请单击，然后单击**终结点**。 在此对话框中，您可以创建终结点，以及编辑或删除终结点，如下图中所示。

![][ic719505]

添加终结点，请单击**添加**按钮在**终结点**属性页，并**添加终结点**对话框将打开。

![][ic710897]

输入终结点的名称，选择类型 （**输入**、**内部**或**InstanceInput**），并指定公用和专用端口。 请按**确定**以保存新的终结点值。

具体取决于终结点的类型，您可以使用端口范围，如下所示︰

* 对于输入的实例终结点，该公用端口可以是一系列的端口 (例如**2000年-2010年**) 和专用端口是固定值的。
* 内部的终结点，不使用此公用端口，和专用端口可以是某一区域，或留的空或设置为一个星号以指示它将自动设置 Azure 的。
* 对于输入终结点，公用端口只能是固定值的，和专用端口可以是固定值的或留的空或设置为一个星号以指示 Azure 将自动设置。

如果您想要使用一个端口号而不是一个范围，保留文本框末尾的范围为空。

端口被设置为自动的如果您需要确定在运行时实际使用的端口，您的应用程序可以使用[com.microsoft.windowsazure.serviceruntime 软件包摘要][]介绍了 Azure 服务运行时 API。

若要查看如何使用实例输入终结点以帮助调试多实例部署，请参见[调试多实例部署中的特定角色实例][]。

要修改某个终结点，请选择该终结点并单击在**终结点**属性页中的**编辑**按钮。 使您可以修改终结点的名称、 类型和公用和专用端口将打开一个对话框。 请按**确定**以保存修改的端点值。

要删除某个终结点，请选择该终结点并单击在**终结点**属性页中的**删除**按钮然后单击****以确认删除。

为了正确配置的某些功能 （如缓存、 远程调试、 会话相似性或 SSL 卸载） 启用的角色中的用户，该工具包可能会自动配置特殊将列出用户定义终结点的终结点。 该工具包可以防止用户编辑或删除此类将自动生成终结点只要启用相关的功能。

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>环境变量属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单，单击**Azure**，，然后单击**环境变量**。 在此对话框中，您可以创建一个环境变量，以及修改或删除环境变量，如下图中所示。

![][ic719506]

环境变量是可用于启动脚本，在角色启动时。

>[AZURE.NOTE] 当指定的环境变量，请注意您的部署将会发布到 Windows 虚拟机时，因此必须有效为基于 Windows 的操作系统环境变量。

角色启动时的环境变量的示例中，通过单击**添加**按钮创建一个新的环境变量。 下图显示正在创建名为**MyRoleVersion**的环境变量并将其分配值**1.0**。

![][ic659268]

在 jsp 代码中，您可以显示值使用`System.getenv`方法︰

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

当应用程序运行时，导致此输出︰

![][ic552233]

若要修改环境变量，请选择环境变量，单击**环境变量**属性页中的**编辑**按钮。 将打开一个对话框使您可以修改环境变量属性。 请按**确定**以保存环境变量的值。

要删除的环境变量，请选择该环境变量和单击**环境变量**属性页中的**删除**按钮然后单击**是**以确认删除。

为了正确配置的某些功能 （例如，服务器配置，远程调试或本地存储区） 启用的角色中的用户，该工具包可能自动配置将用户定义的环境变量列出的特殊环境变量。 该工具包可防止用户编辑或删除此类将自动生成环境变量只要启用相关的功能。

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>负载平衡 / 会话关联 (a.k.a"粘滞会话") 属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单， **Azure**，请单击，然后单击**负载平衡**。 在此对话框中，必须能够启用或禁用会话关联，如下图中所示。

![][ic719492]

有关相关信息，请参阅[会话关联][]。 在[SSL 卸载][]所述，还请注意此功能行为的 SSL 卸载，上下文中。

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>本地存储属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单，单击**Azure**，，然后单击**本地存储**。 在此对话框中，可以创建、 修改或删除虚拟机正在运行您的应用程序的临时本地存储的能力。 可以大小的本地存储区，以及时的作用是将回收的如下图所示，是否保留内容设置特定的值。

![][ic719508]

您还可以指定对应于本地存储的环境变量。

默认情况下，一切都将部署到 Azure （并解压缩） 角色实例的**approot**文件夹中。 而最简单的部署将有适合即使在解压缩， **approot**目录分配的空间受到限制，而且不明确 （小于 1 GB 是一个合理的经验）。 因此，为了确保 Azure 为较大型的部署，可能不适合在**approot**文件夹中分配足够的磁盘空间，您应该设置使用**本地存储**对话框的本地存储资源。 若要执行此操作的简便方法，请参阅[部署大型部署][]。

您可以轻松地引用使用环境变量自动 Eclipse 工具包与资源相关联，如**本地存储**对话框中所示的存储资源从启动脚本 (例如，您**startup.cmd**)。 该环境变量将包含您在启动脚本执行的时间配置的本地资源的完整路径。 

要修改的本地存储资源，请选择本地存储资源，单击**本地存储**属性页中的**编辑**按钮。 将打开一个对话框，使您可以修改本地存储资源属性。 请按**确定**以保存的本地存储资源值。

若要删除本地存储资源时，选择的本地存储资源并单击**本地存储**属性页中的**删除**按钮然后单击**是**以确认删除。

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>服务器配置属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单，单击**Azure**，，然后单击**服务器配置**。 在此对话框中，可以添加、 删除和修改您的部署，使用的 JDK 和 Java 应用程序服务器以及添加或删除您的部署使用的应用程序 （如战争、 JAR 或 EAR 文件）。

### <a name="jdk-configuration"></a>JDK 配置 ###

此对话框允许您指定要用于部署的 JDK 包。 如果您在 Windows 上使用 Eclipse，您可以指定本地在 Azure 仿真程序中运行时使用的 JDK 包，您可以选择要将该本地安装部署到 Azure。 在非 Windows 操作系统，仿真器 JDK 设置不适用，由于无法与 Windows 兼容，不能将部署本地安装的 JDK。 但是，无论您正在使用的操作系统，可以始终选择第三方 JDK 包地部署到 Azure，或从其他下载位置指向 Windows 兼容 JDK 包之间。

以下是如何在 Windows 上指定的 JDK 的示例︰

![][ic780647]

如果您在 Windows 上使用 Eclipse，则可以指定 JDK 用于计算仿真;为此，请确保**使用 JDK 进行本地测试此文件路径中的**签入**仿真程序部署**部分。 然后，指定您的 JDK; 本地路径您可以浏览到不同的 JDK 如果未自动选择要使用的一个。 您还可以选择将您的 JDK 部署到 Azure 的云服务;若要执行此操作，选择**部署我本地的 JDK （自动上传到云存储）**的**云部署**节中。

注意︰ 在非 Windows 操作系统的系统上的**仿真程序部署**设置和**部署我本地的 JDK**选项不可用。 下面的示例阐释了在 Mac 或其他受支持的非 Windows 操作系统上指定的 JDK:

![][ic789643]

而不考虑所使用的操作系统，您有以下两个**云部署**选项源和 JDK 包类型︰

* **部署在 Azure 上可用的第三方 JDK 包** 
* **从自定义下载部署** 

如果您使用**第三方可以从 Azure 的 JDK 包的部署**选项︰

1. 选中名为**部署第三方 JDK 包可从 Azure**的复选框。
1. 从下拉列表中，选择在 Azure 可用的第三方 JDK 包。
1. 您的**JDK**选项卡将显示类似以下 Windows 上︰  ![][ic780648] 
   和它看起来类似于以下内容 Mac OS 或其他支持非 Windows 的操作系统︰ ![][ic789643]
1. 单击**确定**以保存所做的更改。
1. 当系统提示您接受来自第三方 JDK 程序包提供商许可协议，请查看许可条款。 假定您接受这些条款，请单击**是**以**接受许可协议**对话框关闭。
    请注意，可自定义的**第三方可以从 Azure 的 JDK 包的部署**选项的下拉列表中显示项目的基本逻辑。 若要自定义的项目，在**JDK**对话框中，单击**定制**链接。 这将关闭**JDK**属性页并在 Eclipse，然后可以根据需要打开**componentsets.xml**文件。 **Componentsets.xml**的文档将包含在**componentsets.xml**文件本身。

如果您使用的**从自定义下载 JDK 部署**选项︰

1. 创建 ZIP 的 JDK 安装目录，确保目录节点本身是 ZIP 结构，而不是其内容的子。 记下目录的名称，以及您将以后，需要牢记此 JDK 安装将部署到 Windows 虚拟机。
1. 作为一个 blob 上载到 Azure 存储帐户 ZIP。 您可以执行此使用上载到 Azure 存储 blob 外部可用的工具。 建议使用专用的 blob。 记下斑点压缩内容的 URL。
1. 选中名为**部署自定义下载 JDK**的复选框。
    如果要下载从 Azure 存储帐户，选择存储帐户**存储帐户**下拉列表 （您可以单击要修改列表中的**帐户**链接），从部分将在**URL**字段中，将其中填满，然后填写该 URL 的其余部分。 如果您不想使用 Azure 存储，**存储帐户**下拉列表中，选择**（无）**和 JDK 下载**URL**字段中输入的 URL。 如果使用 Azure 存储，在 URL 中的 blob 名称必须为小写。
1. 确保**JAVA_HOME**文本框指正确的目录名称。 默认情况下，它将引用相同的 JDK 目录名称选择供本地使用的值。 但如果 ZIP 中所包含的目录具有不同的名称 （例如，由于使用了不同的版本）， **JAVA_HOME**文本框中的目录名称相应地更新，因为将在云中 （不计算仿真程序） 中使用此设置。
1. 单击**确定**以保存所做的更改。

就是这样。 现在，当您针对云进行构建，您会注意到的包大小要小得多、 生成过程通常需要更少的时间，和部署自身发布到云时还应采取更少的时间。 请注意，**部署我本地的 JDK （自动上传到云存储）**或**从自定义下载 JDK 部署**选项实际上仅当在云中部署应用程序。 他们有没有影响计算的仿真程序经验;部署到仿真程序计算时，仍将使用组件的本地版本。 

### <a name="server-configuration"></a>服务器配置 ###

以下是如何指定应用程序服务器的示例。

![][ic796926]

验证**部署这种类型的服务器**复选框已选中，并且然后选择您要使用的应用程序服务器的类型。

用于指定要使用的云部署的服务器，您可以利用以下选项︰

1. **部署第三方服务器可以在 Azure 上**-这是开发/测试方案其中部署效率和简化的原则是一个优先级，服务器不需要自定义配置中尤其适用。 或者当您想要使用一台服务器作为起始点，但是包含相应服务器的自定义部署的启动逻辑中的步骤。
1. **从自定义下载部署**-当您有您想要使用云中的精心准备和配置服务器，这是在生产方案中尤其适用。
1. **我的本地服务器安装部署**-这是特别适用于如果您本地的服务器安装已供您使用的自定义配置。 如果您选择此选项，还必须在下面的**服务器本地路径**文本框中指定本地服务器的路径。

如果您使用**第三方服务器在 Azure 上可用的部署**选项︰

1. 选中名为**部署第三方服务器可以在 Azure 上**的复选框。
1. 从下拉菜单中，选择所需的服务器软件在云中部署中使用。 请注意，如果已经指定了一种使用更早版本的服务器，您将选择该服务器类型为同一系列中的云服务器限制。 但是，如果您不选择服务器类型，您可以从任何在 Azure 当前可用的服务器和服务器类型会自动为您选择。
1. 单击**确定**以保存所做的更改。

如果使用**自定义下载中的部署**选项︰

1. 请确保您已选择一个服务器类型，按照前面的步骤。 这会告诉插件如何部署服务器的自定义下载，因为它必须来自同一族与选定的服务器类型。
1. 选中名为**部署自定义下载中**的复选框。
    如果您想要下载从 Azure 存储帐户，则选择存储帐户**存储帐户**下拉列表 （您可以单击要修改列表中的**帐户**链接），从部分将在**URL**字段中，将其中填满，然后填写该 URL 的其余部分与您的服务器下载 ZIP （当使用 Azure 存储，在 URL 中的 blob 名称必须是小写）。 如果您不想使用 Azure 存储，**存储帐户**下拉列表中，选择**（无）**和服务器下载 ZIP **URL**字段中输入的 URL。 ZIP 包含一个表示应用程序服务器安装目录的子文件夹。 例如，如果您正在使用的压缩 zip 中的 Apache Tomcat 7.0.35，将代表的安装目录，如**apache 的 tomcat-7.0.35**的子文件夹。 
1. 指定的主目录环境变量的值。 它将默认为本地应用程序服务器，如果有的话，使用的值，但如果云应用程序服务器是不同于您的本地应用程序服务器，可以指定一个不同的值。 但是，您需要确保云中应用程序服务器与服务器同一家族的先前选择的类型。
    如果在将来更新云应用程序服务器邮政编码，您可以手动更改主目录设置或，以让它再次匹配本地设置 （如果太更改本地应用程序服务器）。
1. 单击**确定**以保存所做的更改。

可以自定义**服务器配置**属性页的**服务器**选项卡中显示的项的基本逻辑。 这是一项高级的功能，如果您需要超出默认值，或者如果您要添加其他服务器可能需要。 若要自定义的逻辑，在**服务器**对话框中，单击**定制**链接。 这将关闭**服务器配置**属性页，并在 Eclipse，然后可以根据需要扩展服务器配置模板中打开**componentsets.xml**文件。 **Componentsets.xml**的文档将包含在**componentsets.xml**文件本身。

如果您使用的**本地服务器 （自动上传到云存储） 部署**选项︰

1. 选中名为**部署本地服务器 （自动上传到云存储）**的复选框。
1. 使用**存储帐户**下拉列表，选择**（自动）**。 如果您指定在此处**（自动）** ，Eclipse 工具包与您的部署中的**发布到 Azure**对话框中选择的一个将相同的存储帐户为您的服务器使用。
1. 单击**确定**以保存所做的更改。

如果满足以下条件的任何一种情况，您计算机上的服务器安装路径选择的**本地服务器路径**文本框中︰

* 您想要在仿真器 （仅适用于 Windows） 中测试您的部署。
* 要将本地安装的服务器部署到云中。
* 您想要使用自定义服务器下载自己的云中，在这种情况下，也确保**我的本地服务器 （自动上传到云存储） 部署**选项上方。

如果没有上述选项将应用于您的具体情况，本地服务器设置是可选的。

### <a name="applications-configuration"></a>应用程序配置 ###

以下是如何指定应用程序的一个示例。

![][ic719512]

单击以添加另一个应用程序，**添加**或**删除**要删除的应用程序。 为了效率，如果想要用于下载源的应用程序部署到云时，使用[组件属性](#components_properties)指定 URL 存储帐户，等等。 

2014 年 4 月发行版开始，您的应用程序都会自动加载到同一存储帐户 （ **eclipsedeploy**容器） 下为您的部署选择。 您的部署的启动逻辑包含第一次从该存储帐户下载这些应用程序的步骤。 这意味着您可以将您的应用程序部署中升级而无需重新生成并通过手动上载较新版本的应用程序直接进入存储帐户 （例如使用 Azure 门户） 重新部署整个包，，替换 WAR 文件最初传人那里该工具包。 然后，只需启动回收使用 Azure 的管理门户，或通过命令行实用程序的所有这些角色实例。 （触发直接从回收 Eclipse 工具包内的角色是目前不支持。）

### <a name="notes-about-server-configuration"></a>有关服务器配置的注释 ###

通过**服务器配置**属性页所做的更改将反映在`<component>`的 package.xml 文件中的元素。

当您使用**自动上载...**或 JDK 或应用程序服务器，并且您**从下载...部署**选项生成的云 （不计算仿真程序），并且已连接到网络时，您可能会注意到生成消息如下所示在控制台输出中，如 Ant 生成器将验证下载的可用性︰

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

如果您选择**下载...从部署**选项，可能会显示下面的警告，但仍会生成︰

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

此警告是唯一下载的可用性尚未验证的指示。 因此如果由于某种原因在云中失败的部署，检查是否收到此警告。

如果您想要禁用下载验证 （例如，如果您觉得这会不必要地减慢生成），设置`verifydownloads`特性`false`在`<windowsazurepackage>`package.xml 的元素︰ 

`<windowsazurepackage verifydownloads="false" ...>` 

如果您选择**自动上载...**选项，然后在控制台窗口中您将看到生成消息报告上载每隔 5 秒钟，只要上载进度是必需的。

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL 卸载属性 ###

在 Eclipse 的项目资源管理器窗格中打开该角色的上下文菜单， **Azure**，请单击，然后单击**SSL 卸载**。 

![][ic719481]

在此对话框中，您可以启用 SSL 卸载，使您可以轻松地支持安全超文本传输协议 (HTTPS) 在 Java 部署在 Azure，而无需您在 Java 应用程序服务器中配置 SSL。 有关详细信息，请参见[SSL 卸载][]和[如何使用 SSL 卸载][]。

## <a name="see-also"></a>请参见 ##

[日蚀式的 azure Toolkit][]

[日蚀式安装 Azure Toolkit][]

[Azure 在 Eclipse 中创建一个 Hello World 应用程序][]

[Azure 项目属性][]

[Azure 存储帐户列表][]

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure 的管理门户]: http://go.microsoft.com/fwlink/?LinkID=512959
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure 项目属性]: http://go.microsoft.com/fwlink/?LinkID=699524
[Azure 存储帐户列表]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.microsoft.windowsazure.serviceruntime 包摘要]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Azure 在 Eclipse 中创建一个 Hello World 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699533
[调试多实例部署中的特定角色实例]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[在 Eclipse 中的调试 Azure 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699535
[部署大型部署]: http://go.microsoft.com/fwlink/?LinkID=699536
[如何使用并存的缓存]: http://go.microsoft.com/fwlink/?LinkID=699542
[如何使用 SSL 卸载]: http://go.microsoft.com/fwlink/?LinkID=699545
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546
[会话的相似性]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL 卸载]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png
