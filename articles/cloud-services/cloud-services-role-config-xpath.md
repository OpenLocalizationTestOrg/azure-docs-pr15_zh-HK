<properties 
pageTitle="云服务角色配置 XPath 作弊表 |Microsoft Azure" 
description="各种的 XPath 设置可用于在云服务角色配置公开为一个环境变量的设置。" 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>为一个环境变量，使用 XPath 公开角色配置设置

在云服务的工作人员或 web 角色服务定义文件中，您可以为环境变量公开运行时配置的值。 支持下面的 XPath 值 （分别对应于 API 值）。

此外可以通过[Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx)库这些 XPath 值中。 

## <a name="app-running-in-emulator"></a>在仿真程序中运行的应用程序

指示仿真程序正在运行该应用程序。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| 代码  | var x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>部署标识

检索部署的实例 ID。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| 代码  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>角色标识 

检索当前角色 ID 的实例。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| 代码  | var id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>更新域

检索该实例的更新域。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| 代码  | var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>容错域

检索实例的故障域。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| 代码  | var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>角色名称

检索实例的角色名称。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| 代码  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>配置设置

检索指定的配置设置的值。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| 代码  | var 设置 = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>本地存储的路径

检索实例的本地存储路径。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| 代码  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1")。根路径; |


## <a name="local-storage-size"></a>本地存储区大小

检索实例的本地存储区的大小。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| 代码  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1")。MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>终结点协议 

检索该实例的终结点协议。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| 代码  | var 端口是否 = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]。协议; |

## <a name="endpoint-ip"></a>终结点 IP

获取指定终结点的 IP 地址。

| 类型 | 示例 |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| 代码  | var 地址 = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]。IPEndpoint.Address |

## <a name="endpoint-port"></a>终结点的端口 

检索该实例的端点端口。

| 类型  | 示例 |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| 代码  | var 端口 = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]。IPEndpoint.Port; |





## <a name="example"></a>示例

下面是一个示例创建名为的环境变量与启动任务是辅助角色的`TestIsEmulated`设置为[@emulatedxpath 值](#app-running-in-emulator)。 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>下一步行动

了解更多关于[ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg)文件。

创建一个[ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg)包。

启用[远程桌面](cloud-services-role-enable-remote-desktop.md)的角色。
