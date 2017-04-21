<properties
    pageTitle="什么是云服务模型和包 |Microsoft Azure"
    description="介绍了云服务模型 （.csdef、.cscfg） 和 Azure 中的软件包 (.cspkg)"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>什么是云服务模型，如何包装？
云服务由三个组件、 服务定义_(.csdef)_、 服务配置_(.cscfg)_和服务包_(.cspkg)_。 **ServiceDefinition.csdef**和**ServiceConfig.cscfg**文件是基于 XML 的和描述的结构的云服务和配置方式;统称为模型。 **ServicePackage.cspkg**是一个 zip 文件，从**ServiceDefinition.csdef** ，还有其他一些功用生成包含所有必需的基于二进制的依存关系。 Azure 创建从**ServicePackage.cspkg**和**ServiceConfig.cscfg**的云服务。

一旦在 Azure 中运行云服务时，您可以重新配置它通过**ServiceConfig.cscfg**文件，但不能变更的定义。

## <a name="what-would-you-like-to-know-more-about"></a>您想详细了解什么？

* 我想要了解更多关于[ServiceDefinition.csdef](#csdef)和[ServiceConfig.cscfg](#cscfg)文件。
* 我已经了解的给我上我可以配置的[一些示例](#next-steps)。
* 我想要创建[ServicePackage.cspkg](#cspkg)。
* 我使用 Visual Studio 的和我想...
    * [创建一个新的云服务][vs_create]
    * [重新配置现有的云服务][vs_reconfigure]
    * [部署一个云服务项目][vs_deploy]
    * [到云服务实例的远程桌面][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
**ServiceDefinition.csdef**文件指定使用 Azure 的云服务配置的设置。 [Azure 服务定义架构 (.csdef 文件)](https://msdn.microsoft.com/library/azure/ee758711.aspx)提供的服务定义文件允许格式。 下面的示例演示可用于 Web 和辅助角色定义的设置︰

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

您可以参考 [服务定义架构] [] 对于更好地了解在此处使用的 XML 架构，但这里的一些元素的快速说明︰

**站点**  
包含 IIS7 中承载的网站或 web 应用程序的定义。

**InputEndpoints**  
包含用于与云服务的终结点的定义。

**InternalEndpoints**  
包含角色实例用于相互通信的终结点的定义。

**ConfigurationSettings**  
包含功能的设置定义的特定角色。

**证书**  
包含证书所需的角色定义。 上面的代码示例演示的 Azure 将连接配置为使用证书。

**LocalResources**  
包含本地存储资源的定义。 本地存储资源是角色的在其中实例正在运行的虚拟机文件系统上保留的目录。

**导入**  
包含导入的模块的定义。 上面的代码示例显示用于远程桌面连接和 Azure 连接模块。

**启动**  
包含角色启动时要运行的任务。 在.cmd 或可执行文件中定义的任务。



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
**ServiceConfiguration.cscfg**文件中的值取决于云服务的设置的配置。 指定要用于部署该文件中每个角色的实例数。 服务定义文件中定义的配置设置的值添加到服务配置文件中。 与云服务相关联的任何管理证书的指纹也添加到文件中。 [Azure 服务配置架构 (.cscfg 文件)](https://msdn.microsoft.com/library/azure/ee758710.aspx)提供的服务配置文件允许的格式。

服务配置文件不打包应用程序，但上载到 Azure 作为一个单独的文件，用于将云服务配置。 无需重新部署您的云服务，您可以上载新的服务配置文件。 云服务正在运行时，可以更改在云服务的配置值。 下面的示例显示可以为 Web 和辅助角色定义的配置设置︰

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

您可以参考[服务配置架构](https://msdn.microsoft.com/library/azure/ee758710.aspx)更好地理解此处使用的 XML 架构，但是，这里是元素的快速说明︰

**实例**  
配置运行角色的实例的数目。 为了防止有可能变得不可用在升级过程中云服务，则建议您部署的面向 web 的角色的多个实例。 这样做之后，都坚持在[Azure 计算服务级别协议 (SLA)](http://azure.microsoft.com/support/legal/sla/)，它保证 99.95%面向 Internet 的角色的外部连接两个或多个角色实例部署服务时的指导原则。

**ConfigurationSettings**  
配置角色的正在运行的实例的设置。 名称`<Setting>`元素必须与服务定义文件中的设置定义相匹配。

**证书**  
配置服务使用的证书。 上面的代码示例演示如何定义远程访问模块的证书。 *指纹*属性的值必须设置为证书的指纹来使用。

<p/>

 >[AZURE.NOTE] 证书指纹可以通过使用文本编辑器，添加到配置文件，也可以在 Visual Studio 中的作用**属性**页的**证书**选项卡上添加值。



## <a name="defining-ports-for-role-instances"></a>定义角色实例的端口
Azure 允许 web 角色只有一个入口点。 这意味着所有的通信都通过一个 IP 地址。 您可以配置您的网站来共享一个端口配置的主机标头，以将请求指向正确的位置。 您还可以配置到已知端口的 IP 地址上侦听的应用程序。

下面的示例显示了网站和 web 应用程序的 web 角色配置。 该网站被配置为在端口 80 上的默认条目位置和 web 应用程序被配置为接收来自被称为"mail.mysite.cloudapp.net"的备用主机头请求。

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>更改角色的配置
而离线获取该服务在 Azure，运行时，您可以更新云服务的配置。 若要更改配置信息，可以上传新的配置文件，或可以编辑的配置文件中的位置，并将其应用到您正在运行的服务。 对服务配置，可以进行以下更改︰

- **更改配置设置的值**  
当配置设置的更改，角色实例可以选择实例处于联机状态，或妥善回收实例和应用实例时更改处于脱机状态时应用更改。

- **更改角色实例的服务拓扑**  
拓扑的更改不影响正在运行的实例，实例移除位置除外。 所有剩余的实例通常不需要进行回收;但是，您可以选择回收响应拓扑变化的角色实例。

- **更改证书指纹**  
当角色实例处于离线状态时，您只能更新证书。 如果证书是添加、 删除或更改角色实例处于联机状态时，Azure 正常所需实例脱机更新证书并将其重新联机更改完毕后。

### <a name="handling-configuration-changes-with-service-runtime-events"></a>处理配置更改服务运行时事件
[Azure 运行时库](https://msdn.microsoft.com/library/azure/mt419365.aspx)包括互动的角色实例中运行的代码将 Azure 环境提供类的[Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx)命名空间。 [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx)类定义之前和之后的配置更改会引发以下事件︰

- **[更改](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx)事件**  
出现这种情况的配置更改应用于角色使您有机会摧毁角色实例，如果所需的指定实例之前。
- **[已更改](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx)事件**  
配置更改应用于一个角色的指定实例后发生。

> [AZURE.NOTE] 证书的更改始终将角色的实例脱机，因为它们不会引发的 RoleEnvironment.Changing 或 RoleEnvironment.Changed 事件。

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
若要部署应用程序作为在 Azure 的云服务，必须首先打包应用程序以适当的格式。 可以使用**CSPack**命令行工具 （使用[Azure SDK](https://azure.microsoft.com/downloads/)安装） 除了 Visual Studio 创建软件包文件。

**CSPack**使用服务定义文件，服务配置文件中的内容来定义包的内容。 **CSPack**生成可以使用[Azure 门户](cloud-services-how-to-create-deploy-portal.md#create-and-deploy)上载到 Azure 应用程序软件包文件 (.cspkg)。 默认情况下，包命名为`[ServiceDefinitionFileName].cspkg`，但您可以通过指定一个不同的名称`/out`的**CSPack**选项。

**CSPack**是通常位于  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
通过运行与 SDK 安装**Microsoft Azure 命令提示符**快捷，CSPack.exe （windows) 上的即可。  
>  
>运行 CSPack.exe 程序本身若要查看有关所有可能的开关和命令的文档。

<p />

>[AZURE.TIP]
在**Microsoft 计算仿真程序**中本地运行云服务，使用此选项会将复制到目录布局，从中他们可以运行计算仿真程序中应用程序的二进制文件的**/copyonly**选项。

### <a name="example-command-to-package-a-cloud-service"></a>若要打包云服务的示例命令
下面的示例创建一个包含 web 角色的信息的应用程序包。 该命令指定服务定义文件来使用，在其中可以找到二进制文件，目录和包文件的名称。

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

如果应用程序包含一个 web 角色和辅助角色，则使用下面的命令︰

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

其中变量定义如下︰

| 变量                  | 值 |
| ------------------------- | ----- |
| \[目录名称\]         | 在 Azure 项目的.csdef 文件所在项目根目录下的子目录。|
| \[ServiceDefinition\]     | 服务定义文件的名称。 默认情况下，此文件命名为 ServiceDefinition.csdef。  |
| \[OutputFileName\]        | 生成的包文件的名称。 通常情况下，此设置为应用程序的名称。 如果未不指定任何文件名，以创建应用程序包\[应用程序名称\].cspkg。|
| \[角色名\]              | 服务定义文件中定义的角色的名称。|
| \[RoleBinariesDirectory] | 角色的二进制文件的位置。|
| \[VirtualPath\]           | 在服务定义中的网站部分中定义的每个虚拟路径的物理目录。|
| \[PhysicalPath\]          | 为服务定义的网站节点中定义的每个虚拟路径的内容的物理目录。|
| \[RoleAssemblyName\]      | 角色的二进制文件的名称。| 


## <a name="next-steps"></a>下一步行动

我将创建一个云服务包，我想...

* [云服务实例设置远程桌面][remotedesktop]
* [部署一个云服务项目][deploy]

我使用 Visual Studio 的和我想...

* [创建一个新的云服务][vs_create]
* [重新配置现有的云服务][vs_reconfigure]
* [部署一个云服务项目][vs_deploy]
* [云服务实例设置远程桌面][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
