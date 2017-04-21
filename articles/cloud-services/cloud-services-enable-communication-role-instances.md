<properties 
pageTitle="在云服务中的角色的通信 |Microsoft Azure" 
description="在云服务中的角色实例可以有端点 http、 https、 tcp （udp） 为其定义其他角色实例之间或与外部进行通信。" 
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

# <a name="enable-communication-for-role-instances-in-azure"></a>允许在 azure 角色实例进行通信

云服务角色通过内部和外部连接进行通信。 内部连接称为**内部端点**时，外部连接被称为**输入终结点**。 本主题介绍如何修改[服务定义](cloud-services-model-and-package.md#csdef)来创建终结点。


## <a name="input-endpoint"></a>输入终结点
输入终结点时要公开向外部端口使用。 指定的协议类型和终结点，然后应用端点的内部和外部端口的端口。 如果需要，您可以指定终结点的不同的内部端口与[本地](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint)属性。

输入终结点可以使用下列协议︰ **http、 https、 tcp、 udp**。

要创建输入终结点，请添加到网页或辅助角色的**终结点**元素**InputEndpoint**子元素。

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>输入的实例的终结点
实例输入终结点是类似输入终结点但是允许您将为每个单独的角色实例特定的面向公众的端口映射使用负载平衡器上的端口转发。 您可以指定单个的面向公众的端口范围。

实例输入终结点只能使用**tcp**或**udp**作为协议。

要创建实例输入终结点，请添加到网页或辅助角色的**终结点**元素**InstanceInputEndpoint**子元素。

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>内部的终结点
为实例与实例通讯提供了内部端点。 端口是可选的如果省略，将动态端口分配到的终结点。 可以使用端口范围。 还有五个内部终结点，每个角色的限制。

内部的终结点可以使用下列协议︰ **http、 tcp、 udp，任何**。

要创建内部输入终结点，请添加到网页或辅助角色的**终结点**元素**InternalEndpoint**子元素。

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

您还可以使用端口范围。

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>辅助角色与 Web 角色

处理工作人员和 web 角色时一个微小的差别，具有终结点。 Web 角色必须至少使用**HTTP**协议的单个输入终结点。


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>使用.NET SDK 访问终结点
Azure 托管库提供了角色实例在运行时进行通信的方法。 从角色实例中运行的代码，您可以检索有关的其他角色实例及其终结点存在的信息以及有关当前角色实例信息。

> [AZURE.NOTE] 您只能检索角色实例运行在云服务中，并定义至少一个内部端点信息。 无法获取角色实例运行在一个不同的服务有关的数据。

[实例](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx)属性可用于检索角色的实例。 第一次使用[CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx)返回对当前角色实例的引用，然后使用[角色](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx)属性返回对本身的角色的引用。

连接到以编程方式通过.NET SDK 的角色实例时，时，相对容易地访问终结点信息。 例如，您已经连接到一个特定角色的环境后，可以得到具有此代码的特定终结点的端口︰

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

**实例**属性返回的**RoleInstance**对象的集合。 此集合通常包含当前实例。 如果角色没有定义的内部终结点，则集合将包括当前的实例，但没有其他实例。 集合中的角色实例数将始终为 1 中没有内部终结点为该角色定义的位置的情况。 如果该角色定义的内部终结点，其实例是在运行时，可以发现，集合中的实例数对应于为该角色的服务配置文件中指定的次数。

> [AZURE.NOTE] Azure 托管库不提供一种确定运行状况的其他角色实例，但您可以实现此类健康评估自己如果您的服务需要此功能。 [Azure 诊断程序](cloud-services-dotnet-diagnostics.md)可用于获取有关正在运行的角色实例的信息。

若要确定对角色的实例的内部终结点的端口号，可以使用[InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx)属性返回一个字典对象，该对象包含终结点名称及其相应的 IP 地址和端口。 [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx)属性返回的 IP 地址和端口指定的终结点。 **PublicIPEndpoint**属性返回一个负载平衡的端点的端口。 不使用**PublicIPEndpoint**属性中的 IP 地址部分。

下面是一个示例循环访问角色实例。

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

这里是一种获取通过服务定义的终结点公开和开始侦听连接的辅助角色。

> [AZURE.WARNING] 此代码仅适用于已部署的服务。 计算仿真程序中运行时，服务的配置元素的创建终结点直接端口 （**InstanceInputEndpoint**元素） 将被忽略。

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>网络通信的规则来控制角色的通信
定义内部终结点后，可以将网络通信量规则 （基于您创建的端点） 添加角色实例如何可以彼此进行通信的控制。 下图显示了用于控制角色通信的常用方案︰

![网络通信规则方案](./media/cloud-services-enable-communication-role-instances/scenarios.png "网络通信规则方案")

下面的代码示例显示在上图中所示的角色的角色定义。 每个角色定义包括至少一个定义的内部终结点︰

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
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
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] 角色之间的通信的限制可能出现的内部端点固定和自动分配端口。

默认情况下，内部终结点定义后，通信可以从流任何角色到角色不受任何限制的内部端点。 要限制通信，必须将**NetworkTrafficRules**元素添加到服务定义文件中的**ServiceDefinition**元素。

### <a name="scenario-1"></a>方案 1
只允许网络通信量从**WebRole1**到**WorkerRole1**。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>方案 2
只有对**WorkerRole1**和**WorkerRole2**允许从**WebRole1**的网络流量。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>方案 3
仅为**WorkerRole1**和**WorkerRole1**到**WorkerRole2**允许从**WebRole1**的网络流量。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>方案 4
仅为**WorkerRole1**， **WebRole1**到**WorkerRole2**和**WorkerRole1**到**WorkerRole2**允许从**WebRole1**的网络流量。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

上面的元素的 XML 架构引用可以找到[这里](https://msdn.microsoft.com/library/azure/gg557551.aspx)。

## <a name="next-steps"></a>下一步行动
阅读有关云服务[模型](cloud-services-model-and-package.md)的详细信息。