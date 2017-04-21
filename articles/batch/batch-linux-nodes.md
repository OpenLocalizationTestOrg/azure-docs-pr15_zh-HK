<properties
    pageTitle="Linux 节点在 Azure 批池 |Microsoft Azure"
    description="了解如何处理您并行计算的工作负载在 Azure 批 Linux 虚拟机池。"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>设置 Azure 批池中的 Linux 计算节点

Azure 批可用于在 Windows 和 Linux 虚拟机上运行并行计算的工作负载。 本文详细介绍了如何使用这两个[批次 Python]批服务中创建的 Linux 计算节点池[py_batch_package]和[批处理.NET] [api_net]客户端库。

> [AZURE.NOTE] [应用程序包](batch-application-packages.md)是当前不受支持的 Linux 计算节点上。

## <a name="virtual-machine-configuration"></a>虚拟机配置

在批处理中创建池的计算节点时，您有两个选项可供选择的节点大小和操作系统︰ 云服务配置和虚拟机配置。

**云服务配置**提供 Windows 计算节点*只*。 [云服务的大小](../cloud-services/cloud-services-sizes-specs.md)，列出了可用的计算节点大小[Azure 来宾操作系统版本和 SDK 兼容性列表](../cloud-services/cloud-services-guestos-update-matrix.md)中列出了可用的操作系统。 当您创建池包含 Azure 云服务节点时，您需要指定仅节点大小和其"OS 系列"，在前面提到的文章中找到。 有关池的 Windows 计算节点，最常用于云服务。

**虚拟机配置**为计算节点提供 Linux 和 Windows 映像。 [Azure 中的虚拟机大小](../virtual-machines/virtual-machines-linux-sizes.md)(Linux) 和[Azure 中的虚拟机大小](../virtual-machines/virtual-machines-windows-sizes.md)(Windows) 中列出了可用的计算节点大小。 创建池包含虚拟机配置节点时，必须指定节点、 虚拟机图像引用和批次节点代理安装在节点上的 SKU 的大小。

### <a name="virtual-machine-image-reference"></a>虚拟机映像的引用

批次服务使用[虚拟机比例设置](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)提供 Linux 计算节点。 这些虚拟机的操作系统映像数由[Azure 市场][vm_marketplace]。 配置虚拟机图像引用时，您指定的市场上虚拟机映像的属性。 以下属性是必需的当您创建的虚拟机映像的引用︰

| **图像的引用属性** | **示例** |
| ----------------- | ------------------------ |
| 发布服务器         | 规范                |
| 提供             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| 版本           | 最新                   |

> [AZURE.TIP] 您可以了解更多有关这些属性以及如何列出市场中[导航和选择与 CLI 或 PowerShell Azure 中的 Linux 虚拟机映像](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md)的映像。 请注意，并非所有市场图像与批处理当前兼容。 有关详细信息，请参阅[节点代理的 SKU](#node-agent-sku)。

### <a name="node-agent-sku"></a>节点代理 SKU

批次节点代理是池中的每个节点上运行，提供节点和批服务之间的命令和控制界面的程序。 有不同的实现，对于不同的操作系统称为 Sku 的节点代理。 实质上，创建一个虚拟机配置时，首先指定虚拟机图像引用，而后又指定节点代理安装在图像上。 通常情况下，每个节点代理时 SKU 是与多个虚拟机映像兼容。 下面是几个节点代理 Sku:

* batch.node.ubuntu 14.04
* batch.node.centos 7
* batch.node.windows amd64

> [AZURE.IMPORTANT] 在市场上的并不是所有虚拟机映像都可以与目前批节点代理兼容。 必须使用批处理 Sdk 以列出可用的节点代理 Sku 和兼容它们是虚拟机映像。 请参阅下文中的详细信息[列表中的虚拟机映像](#list-of-virtual-machine-images)。

## <a name="create-a-linux-pool-batch-python"></a>创建一个 Linux 池︰ 批 Python

下面的代码段显示了如何使用[Python 的 Microsoft Azure 批客户端库]的示例[py_batch_package]计算节点创建的 Ubuntu 服务器池。 参考文档的批处理 Python 模块可以找到在[azure.batch 包][py_batch_docs]上阅读这些文档。

此代码段创建[ImageReference] [py_imagereference]显式，并指定每个属性 （发行商，优惠，SKU、 版本）。 在生产代码中，但是，我们建议使用[list_node_agent_skus] [py_list_skus]方法来确定，并从可用图像和节点代理 SKU 组合在运行时选择。

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

如上文所述，我们建议，而不是创建[ImageReference] [py_imagereference]您显式地使用[list_node_agent_skus] [py_list_skus]方法来动态选择从当前受支持的节点代理/市场图像组合。 下面的 Python 代码段显示了使用此方法。

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>创建一个 Linux 池︰ 批.NET

下面的代码段演示如何使用[批处理.NET]的[nuget_batch_net]客户端库来创建计算节点的 Ubuntu 服务器池。 您可以找到[批.NET 参考文档][api_net]在 MSDN 上。

下面的代码段使用[PoolOperations][net_pool_ops]。[ListNodeAgentSkus][net_list_skus]方法来选择列表中当前支持市场上图像和节点代理 SKU 组合。 该技术是理想的因为可能会随时更改的受支持的组合列表。 通常情况下，添加受支持的组合。

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

虽然前一段使用[PoolOperations][net_pool_ops]。[ListNodeAgentSkus][net_list_skus]方法来动态列表，并选择从支持图像和节点代理 SKU 组合 （建议使用），您还可以配置[ImageReference] [net_imagereference]明确︰

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>虚拟机映像的列表

下表列出了与兼容的可用批次节点代理上次更新这篇文章时市场上虚拟机映像。 请务必注意，此列表并不最终因为图像和节点代理可能添加或删除任何时候。 我们建议您的批处理应用程序和服务始终使用[list_node_agent_skus] [ py_list_skus] (Python) 和[ListNodeAgentSkus] [ net_list_skus] (批处理.NET) 确定和选择从当前可用 Sku。

> [AZURE.WARNING] 下面的列表可能会随时更改。 始终使用批处理 Api 提供的**列表节点代理 SKU**方法列表，然后运行批处理作业时选择兼容的虚拟机和节点代理 Sku。

| **发布服务器** | **提供** | **图像的 SKU** | **版本** | **节点代理 SKU ID** |
| ------- | ------- | ------- | ------- | ------- |
| 规范 | UbuntuServer | 14.04.0-LTS | 最新 | batch.node.ubuntu 14.04 |
| 规范 | UbuntuServer | 14.04.1-LTS | 最新 | batch.node.ubuntu 14.04 |
| 规范 | UbuntuServer | 14.04.2-LTS | 最新 | batch.node.ubuntu 14.04 |
| 规范 | UbuntuServer | 14.04.3-LTS | 最新 | batch.node.ubuntu 14.04 |
| 规范 | UbuntuServer | 14.04.4-LTS | 最新 | batch.node.ubuntu 14.04 |
| 规范 | UbuntuServer | 14.04.5-LTS | 最新 | batch.node.ubuntu 14.04 |
| 规范 | UbuntuServer | 16.04.0-LTS | 最新 | batch.node.ubuntu 16.04 |
| Credativ | Debian | 8 | 最新 | batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | 最新 | batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | 最新 | batch.node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | 最新 | batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | 最新 | batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | 最新 | batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | 最新 | batch.node.opensuse 13.2 |
| SUSE | openSUSE 飞跃 | 42.1 | 最新 | batch.node.opensuse 42.1 |
| SUSE | SLES HPC | 12 | 最新 | batch.node.opensuse 42.1 |
| SUSE | SLES | 12 SP1 | 最新 | batch.node.opensuse 42.1 |
| microsoft 广告 | 标准数据科学 vm | 标准数据科学 vm | 最新 | batch.node.windows amd64 |
| microsoft 广告 | linux 数据科学 vm | linuxdsvm | 最新 | batch.node.centos 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | 最新 | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 数据中心 | 最新 | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 R2 数据中心 | 最新 | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows 服务器技术预览 | 最新 | batch.node.windows amd64 |

## <a name="connect-to-linux-nodes"></a>连接到 Linux 节点

在开发期间或在解决问题时，您可能会发现需要登录到您的池中的节点。 与 Windows 计算节点，您不能使用远程桌面协议 (RDP) 连接到 Linux 节点。 相反，批服务启用 SSH 访问每个节点上的远程连接。

下面的 Python 代码段在池中，这是个远程连接的每个节点上创建一个用户。 然后，它输出的每个节点的安全外壳 (SSH) 连接信息。

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

以下是前面的代码中包含四个 Linux 节点池的示例输出︰

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

请注意，而不是密码，您可以指定 SSH 公钥在一个节点上创建用户。 在 Python SDK，这通过在[ComputeNodeUser]上使用**ssh_public_key**参数[py_computenodeuser]。 在.NET 中，这通过使用[ComputeNodeUser][net_computenodeuser]。[SshPublicKey][net_ssh_key]属性。

## <a name="pricing"></a>定价

在 Azure 云服务和 Azure 虚拟机技术建立了 azure 的批次。 批次服务本身会提供免费，这意味着您进行计费的计算资源只批解决方案使用。 当您选择**云服务配置**时，将收取取决于[云服务定价]的[cloud_services_pricing]结构。 选择**虚拟机配置**时，将收取根据[虚拟机定价][vm_pricing]结构。

## <a name="next-steps"></a>下一步行动

### <a name="batch-python-tutorial"></a>批次 Python 教程

有关如何处理批次使用 Python 的更深入教程，请检查出[入门 Azure 批 Python 客户端](batch-python-tutorial.md)。 [代码示例]及其助理[github_samples_pyclient]包括一个 helper 函数， `get_vm_config_for_distro`，显示另一种方法来获取虚拟机配置。

### <a name="batch-python-code-samples"></a>批次 Python 代码示例

签出其他的[Python 代码示例][ github_samples_py] [批次的 azure 示例]中[github_samples]在 GitHub 上的几个脚本，说明如何执行常见的批处理操作，例如池、 作业和任务创建的存储库。 [自述文件][ github_py_readme] ，伴随 Python 样本包含有关如何安装所需的程序包的详细信息。

### <a name="batch-forum"></a>批次论坛

[Azure 批论坛][forum]在 MSDN 上是很好的讨论批和询问服务有关的问题。 阅读很有帮助的"stickied"的帖子，并出现时生成批处理解决方案发布您的问题。

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
