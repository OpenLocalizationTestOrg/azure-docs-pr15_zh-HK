<properties
    pageTitle="教程-入门 Azure 批 Python 客户端 |Microsoft Azure"
    description="了解 Azure 批以及如何开发一个简单的方案与批服务的基本概念"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>开始使用 Azure 批 Python 客户端

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

学习基础知识的[Azure 批][azure_batch]和[批处理 Python] [py_azure_sdk]客户端我们讨论用 Python 编写一个小的批处理应用程序。 我们看了两个示例脚本使用批处理服务处理云，和它们的交互方式使用[Azure 存储](./../storage/storage-introduction.md)临时文件和检索中的 Linux 虚拟机上的并行工作负荷。 将学习通用的批处理应用程序工作流批处理作业、 任务、 池的主要组件的基本理解和计算节点。

![批次解决方案工作流 （基本）][11]<br/>

## <a name="prerequisites"></a>系统必备组件

本文假定您具有 Python 和熟悉 Linux 的工作知识。 它还假定您能够满足下面的 Azure 和批处理和存储服务指定的帐户创建要求。

### <a name="accounts"></a>帐户

- **Azure 帐户**︰ 如果还没有[创建免费的 Azure 帐户]的 Azure 订阅[azure_free_account]。
- **批次帐户**︰ 一旦有了 Azure 的订阅，[创建一个批处理 Azure 帐户](batch-account-create-portal.md)。
- **存储帐户**︰ 请参阅[有关 Azure 存储帐户](../storage/storage-create-storage-account.md)中[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)。

### <a name="code-sample"></a>代码示例

Python 教程[代码示例][github_article_samples]许多批处理代码示例之一在[azure 的批次的样本]中找到[github_samples]在 GitHub 上的存储库。 您可以通过单击下载所有样本**克隆或都下载 > 都下载 ZIP**在存储库中的主页上，或通过单击[azure 批样本 master.zip] [github_samples_zip]直接都下载链接。 一旦您解压缩 ZIP 文件的内容，本教程中的两个脚本位于`article_samples`目录︰

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python 的环境

若要在本地工作站上运行的*python_tutorial_client.py*示例脚本，您需要**Python 解释器**兼容版本**2.7**或**3.3 +**。 Linux 和 Windows 上，已经过测试脚本。

### <a name="cryptography-dependencies"></a>加密技术的依赖关系

[加密技术]的依赖项，您必须安装[crypto]库，所需的`azure-batch`， `azure-storage` Python 包。 执行下列操作适用于您的平台之一，或请参阅[加密安装][crypto_install]的详细信息详细信息︰

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* 窗口

    `pip install cryptography`

>[AZURE.NOTE] 如果安装 Python 3.3 + 在 Linux 上，使用 Python 相关性 python3 等效项。 例如，在 Ubuntu 中︰`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure 包

接下来，将**Azure 批处理**和**Azure 存储**Python 软件包安装。 你可以**骰**和*requirements.txt*以下位置找到︰

`/azure-batch-samples/Python/Batch/requirements.txt`

**Pip**命令安装批处理和存储包后面的问题︰

`pip install -r requirements.txt`

或者，您可以安装[azure 批][pypi_batch]和[azure 存储][ pypi_storage] Python 手动程序包︰

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] 您可能需要前缀与命令`sudo`如果您正在使用非特权的帐户。 例如， `sudo pip install -r requirements.txt`。 安装 Python 包的详细信息，请参阅[安装程序包][ pypi_install] readthedocs.io 上。

## <a name="batch-python-tutorial-code-sample"></a>以批处理方式 Python 教程代码示例

批次 Python 教程代码示例由两个 Python 脚本和几个数据文件组成。

- **python_tutorial_client.py**︰ 批处理和存储服务，若要在计算节点 （虚拟机） 上执行并行的工作负荷与交互。 在本地工作站上运行*python_tutorial_client.py*脚本。

- **python_tutorial_task.py**︰ 在运行该脚本计算节点在 Azure 来执行实际的工作中。 在示例中， *python_tutorial_task.py*会分析从 Azure 存储 （输入文件） 下载文件中的文本。 然后它生成一个文本文件 （输出文件） 包含在输入文件中出现的前三个单词的列表。 它创建的输出文件之后， *python_tutorial_task.py*将文件上载到 Azure 存储中。 这样，就可以下载到您的工作站上运行的客户端脚本。 在批处理服务的多个计算节点上同时运行*python_tutorial_task.py*脚本。

- **./data/taskdata\*.txt**︰ 这些三个文本文件提供在计算节点上运行的任务的输入。

下面的关系图说明了客户端与任务脚本执行的主要操作。 此基本工作流是典型的很多都使用批创建的计算解决方案。 虽然它并不演示每个批次服务中可用的功能，几乎每个批次的方案包括︰ 此工作流的一部分。

![工作流批处理示例][8]<br/>

[**第 1 步。**](#step-1-create-storage-containers) 在 Azure Blob 存储创建的**容器**。<br/>
[**第 2 步。**](#step-2-upload-task-script-and-data-files) 将任务脚本和输入文件上载到容器中。<br/>
[**第 3 步。**](#step-3-create-batch-pool) 创建一个批处理**池**。<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a。** 他们加入了池，池**StartTask**节点下载任务脚本 (python_tutorial_task.py)。<br/>
[**第 4 步。**](#step-4-create-batch-job) 创建批处理**作业**。<br/>
[**第 5 步。**](#step-5-add-tasks-to-job) 将**任务**添加到该作业中。<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a。** 计划任务在节点上执行。<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b。** 每个任务从 Azure 存储下载其输入的数据，然后开始执行。<br/>
[**第 6 步。**](#step-6-monitor-tasks) 监视任务。<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a。** 完成任务后，他们将其输出数据上载到 Azure 存储。<br/>
[**第 7 步。**](#step-7-download-task-output) 从存储下载任务输出。

如提到的不是每个批处理解决方案执行这些具体步骤，并可能包括很多更多，但此示例演示批处理解决方案中找到的通用流程。

## <a name="prepare-client-script"></a>准备客户端脚本

运行此示例之前，请添加*python_tutorial_client.py*批次和存储帐户凭据。 如果您有没试过，在自己喜爱的编辑器中打开该文件并使用您的凭据更新以下行。

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

在[Azure 门户网站]可以找到批处理和存储帐户凭据中的每个服务帐户刀片式服务器[azure_portal]:

![批处理在门户中的凭据][9]
![在门户中的存储凭据][10]<br/>

在下面的章节中，我们分析脚本用于处理批次服务中的工作负荷的步骤。 我们鼓励您定期请参阅您的编辑器中的脚本即可浏览文章的其余部分。

定位到**python_tutorial_client.py**开始与步骤 1 中的以下行︰

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>步骤 1︰ 创建存储容器

![在 Azure 存储中创建容器][1]
<br/>

批次包括与 Azure 存储进行交互的内置支持。 在您的存储帐户的容器将提供在批帐户下运行的任务所需的文件。 容器还提供一个位置来存储任务生成的输出数据。 *Python_tutorial_client.py*脚本的作用的第一件事是[Azure Blob 存储](../storage/storage-introduction.md#blob-storage)在创建的三个容器︰

- **应用程序**︰ 此容器将存储运行的任务， *python_tutorial_task.py*的 Python 脚本。
- **输入**︰ 任务将下载要从*输入*容器处理的数据文件。
- **输出**︰ 当任务完成时输入的文件的处理时，他们会将结果上传到*输出*容器。

为了与存储帐户进行交互并创建容器时，我们使用[azure 存储][pypi_storage]软件包创建[BlockBlobService] [py_blockblobservice]对象--"blob 客户"。 然后，我们使用 blob 客户端的存储帐户中创建三个容器。

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

一旦创建容器应用程序现在可以上载任务将使用的文件。

> [AZURE.TIP] [如何使用 Python 从 Azure Blob 存储](../storage/storage-python-how-to-use-blob-storage.md)提供了很好地使用 Azure 存储容器和 blob 的概括。 随着您开始使用批处理，它应该是阅读材料列表的顶部附近。

## <a name="step-2-upload-task-script-and-data-files"></a>步骤 2︰ 将任务脚本和数据文件上载

![将任务应用程序和输入 （数据） 的文件上载到容器][2]
<br/>

在文件上载操作中， *python_tutorial_client.py*首先定义**应用程序**和**输入**文件路径的集合存在本地计算机上。 然后它将这些文件上载到您在上一步中创建的容器。

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

使用列表理解`upload_file_to_container`集合和两个[ResourceFile]中每个文件中调用函数[py_resource_file]填充集合。 `upload_file_to_container`函数如下︰

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [py_resource_file]提供下载到计算节点运行该任务之前的 Azure 存储中的文件的 URL 与批处理中的任务。 [ResourceFile][py_resource_file]。**blob_source**属性指定文件的完整 URL，因为它在 Azure 存储中存在。 该 URL 还可以包括提供安全访问该文件的共享的访问签名 (SAS)。 批中的大多数任务类型包括一个*ResourceFiles*属性，包括︰

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

此示例不使用 JobPreparationTask 或 JobReleaseTask 任务类型，但您可以阅读更多有关它们在[Azure 批次运行作业准备工作和完成任务计算节点](batch-job-prep-release.md)。

### <a name="shared-access-signature-sas"></a>共享的访问签名 (SAS)

共享的访问签名是提供安全访问容器和 Azure 存储中的 blob 的字符串。 *Python_tutorial_client.py*脚本使用两个 blob 和容器共享访问签名，并演示如何从存储服务中获取这些共享的访问签名字符串。

- **Blob 共享访问签名**︰ 池的 StartTask 使用 blob 共享的访问签名时从存储下载任务脚本和输入数据文件 （请参阅下面的[步骤 3](#step-3-create-batch-pool) ）。 `upload_file_to_container` *Python_tutorial_client.py*中的函数包含获取每个 blob 共享的访问签名的代码。 它是通过调用[BlockBlobService.make_blob_url] [py_make_blob_url]存储模块中。

- **容器的共享的访问签名**︰ 为每个任务完成后它在计算节点上的工作，它将其输出文件上载到 Azure 存储中的*输出*容器。 为此， *python_tutorial_task.py*使用提供写访问容器的容器共享的访问签名。 `get_container_sas_token`在*python_tutorial_client.py*函数获取容器的共享的访问，签名，然后传递给任务作为命令行参数。 步骤 5，[将任务添加到作业](#step-5-add-tasks-to-job)，讨论 SAS 的容器的使用情况。

> [AZURE.TIP] 签出共享的访问签名两部分的系列[第 1 部分︰ 了解 SAS 模型](../storage/storage-dotnet-shared-access-signature-part-1.md)和[第 2 部分︰ 创建和使用 SAS 的 Blob 服务](../storage/storage-dotnet-shared-access-signature-part-2.md)，以了解更多关于提供安全地访问您的存储帐户中的数据。

## <a name="step-3-create-batch-pool"></a>步骤 3︰ 创建批处理池

![创建一个批处理池][3]
<br/>

一个批次**池**是计算节点 （虚拟机），在其批次执行作业的任务的集合。

它将任务脚本和数据文件上载到的存储帐户后， *python_tutorial_client.py*将开始通过使用批处理 Python 模块批服务与它交互。 为此，请[BatchServiceClient] [py_batchserviceclient]将创建︰

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

接下来，调用的批处理帐户中创建池的计算节点`create_pool`。

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

在创建池时，您定义[PoolAddParameter] [ py_pooladdparam] ，它指定池的多个属性︰

- **ID** (*id* -所需) 的池<p/>与批处理中的大多数实体，新的池必须有批帐户内唯一的 ID。 您的代码是指使用其 ID，该池和它是您如何识别在 Azure[门户]池[azure_portal]。

- **计算节点数**(*target_dedicated* -所需)<p/>此属性指定数量的虚拟机应部署在池中。 值得注意的所有批次帐户具有默认**配额**限制的**核心**，因而计算节点） 数批帐户中。 您可以找到默认配额及说明 （如最大批处理帐户中的内核数）[增加配额](batch-quota-limit.md#increase-a-quota)的[配额和限制的 Azure 批服务](batch-quota-limit.md)中。 如果您发现自己在问"为什么不能我池达到多 X 节点吗？" 此核心配额可能就是原因。

- **操作系统**的节点 (*virtual_machine_configuration* **或** *cloud_service_configuration* -所需)<p/>在*python_tutorial_client.py*，我们将创建池的 Linux 节点上使用[VirtualMachineConfiguration][py_vm_config]。 `select_latest_verified_vm_image_with_node_agent_sku`在`common.helpers`简化了使用[Azure 虚拟机市场][vm_marketplace]图像。 有关使用市场上映像的详细信息，请参阅[设置 Linux 计算节点在 Azure 批池](batch-linux-nodes.md)。

- **计算节点的大小**(*vm_size* -所需)<p/>因为我们 Linux 节点指定为我们[VirtualMachineConfiguration][py_vm_config]，我们指定的 VM 大小 (`STANDARD_A1`在此示例中) 从[Azure 中的虚拟机大小](../virtual-machines/virtual-machines-linux-sizes.md)。 有关，请参见[设置 Linux 计算节点在 Azure 批池](batch-linux-nodes.md)的详细信息。

- **启动任务**(*start_task* -不需要)<p/>连同上述物理节点的属性，您还可以指定[StartTask] [ py_starttask] （不需要） 的池。 该节点加入池，以及每次重新启动节点，每个节点上执行 StartTask。 StartTask 是计算节点准备执行任务，如安装任务运行的应用程序特别有用。<p/>在此示例应用程序中，将 StartTask 复制从下载 （这通过使用 StartTask 的**resource_files**属性指定） 的存储*工作目录*StartTask 与在该节点上运行的所有任务可以都访问该*共享*目录的文件。 实质上，此操作将复制`python_tutorial_task.py`为每个节点上的共享目录节点连接池，以便在该节点上运行的所有任务可以访问它。

您可能会注意到在调用`wrap_commands_in_shell`helper 函数。 此函数接受单独的命令的集合，并创建一个单一的命令行适用于任务的命令行属性。

上述代码段中的值得一提也是在 StartTask 的**command_line**属性的两个环境变量的使用︰ `AZ_BATCH_TASK_WORKING_DIR` ， `AZ_BATCH_NODE_SHARED_DIR`。 在批处理池中每个计算节点会自动获配特定于批处理的几个环境变量。 执行任务的任何进程有权访问这些环境变量。

> [AZURE.TIP] 若要了解有关在批处理池以及任务工作目录信息中计算节点可用的环境变量的详细信息，请参见**任务的环境设置**和**文件和目录**在[Azure 批处理功能概述](batch-api-basics.md)

## <a name="step-4-create-batch-job"></a>步骤 4︰ 创建批处理作业

![创建批处理作业][4]<br/>

批处理**作业**是集合的任务，并与池的计算节点相关联。 在关联的池的计算节点上执行作业中的任务。

您可以用来组织和跟踪相关工作负荷，而且还为强加某些约束条件 — 例如最大运行时库的作业 （并通过扩展，其任务） 的任务使用作业和作业优先级相对于批帐户中的其他作业。 但是，在此示例中，该作业是仅与在步骤 3 中创建的池相关联的。 没有附加属性进行配置。

所有批处理作业都都与一个特定的池关联。 这个关联表示哪些节点执行该作业的任务。 通过使用[PoolInformation]中指定的池[py_poolinfo]属性，如以下代码片段中所示。

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

既然已创建一个作业，任务添加要执行的工作。

## <a name="step-5-add-tasks-to-job"></a>步骤 5︰ 将任务添加到作业

![将任务添加到作业][5]<br/>
*（1） 任务添加到该作业、 （2） 任务安排为在节点上，运行和 （3） 任务下载要处理的数据文件*

批次**的任务**是工作的在计算节点执行单独单元。 任务已命令行并运行脚本或可执行文件的命令行中指定的。

实际执行工作，必须将任务添加到作业。 每个[CloudTask] [py_task]配置命令行属性和[ResourceFiles] [ py_resource_file] （如池 StartTask） 之前自动执行命令行任务下载到的节点。 在示例中，每个任务处理只有一个文件。 因此，其 ResourceFiles 集合包含单个元素。

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] 当他们访问环境变量如`$AZ_BATCH_NODE_SHARED_DIR`或执行应用程序的节点中找不到`PATH`，任务命令行必须调用 shell 明确，如与`/bin/sh -c MyTaskApplication $MY_ENV_VAR`。 这一要求是不必要的如果您的任务在该节点中执行应用程序`PATH`并不引用任何环境变量。

在`for`上面代码段中的循环，您可以看到具有五个命令行参数传递给*python_tutorial_task.py*的构造时用于该任务的命令行︰

1. **文件路径**︰ 这是在节点上存在该文件的本地路径。 ResourceFile 中的对象时`upload_file_to_container`是在上面的步骤 2 中的文件的名称已用于此属性 ( `file_path` ResourceFile 构造函数中的参数)。 这表明可以在节点上与*python_tutorial_task.py*相同的目录中找到该文件。

2. **numwords**: top *N*词应写入的输出文件。

3. **storageaccount**︰ 拥有任务输出应该上载到其容器的存储帐户的名称。

4. **storagecontainer**︰ 输出文件应上载到的存储容器的名称。

5. **sastoken**︰ 提供到 Azure 存储中的**输出**容器的写访问权限的共享的访问签名 (SAS)。 *Python_tutorial_task.py*脚本使用此共享的访问签名时创建其 BlockBlobService 的引用。 这样到容器的写访问权限，而无需存储帐户的访问键。

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>第 6 步︰ 监视任务

![监视任务][6]<br/>
*脚本 (1) 监视任务的完成状态，和 （2） 任务将结果数据上载到 Azure 存储*

当任务被添加到作业时，它们是自动排队，计划中与此作业关联的池的计算节点上执行。 根据您指定的设置，批次处理所有任务队列、 调度、 重试，和其他任务管理职责为您。

有许多方法监视执行任务。 `wait_for_tasks_to_complete` *Python_tutorial_client.py*中的函数提供了监视的一个简单示例任务特定的状态，在这种情况下，[完成][py_taskstate]状态。

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>第 7 步︰ 下载任务输出

![从存储下载任务输出][7]<br/>

现在，作业完成后，可以从 Azure 存储下载任务的输出。 这是通过调用`download_blobs_from_container` *python_tutorial_client.py*中︰

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] 对`download_blobs_from_container`在*python_tutorial_client.py*中指定的文件，应下载到您的主目录。 随意修改此输出位置。

## <a name="step-8-delete-containers"></a>步骤 8︰ 删除容器

负责驻留在 Azure 存储中的数据，因为它总是最好删除任何不再需要的批处理作业的 blob。 在*python_tutorial_client.py*，这通过三个调用[BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>步骤 9︰ 删除作业和池

在最后一步，提示您删除该作业，而且由*python_tutorial_client.py*脚本创建的池。 虽然您不负责的作业和任务本身，您**收取计算节点。 因此，我们建议您仅在需要时分配节点。 删除未使用的池，可以是维护过程中的一部分。

BatchServiceClient [JobOperations] [py_job]和[PoolOperations] [py_pool]都有相应的删除方法，如果您确认删除操作被称为︰

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] 请记住，您进行计费的计算资源，删除未使用的池将成本降到最低。 还要注意，删除一个池删除此池中的所有计算节点和，在删除该池之后，所有的节点上的数据将无法恢复。

## <a name="run-the-sample-script"></a>运行示例脚本

当您运行*python_tutorial_client.py*脚本教程[的代码示例]从[github_article_samples]，控制台输出将与以下内容类似。 会在暂停`Monitoring all tasks for 'Completed' state, timeout in 0:20:00...`在创建池的计算节点，启动，并在池的开始任务命令都被执行。 使用[Azure 的门户网站][azure_portal]要监视您的池，计算节点、 作业和任务期间和之后执行。 使用[Azure 的门户网站][azure_portal]或[Microsoft Azure 存储浏览器][storage_explorer]来查看应用程序创建的存储资源 （容器和 blob）。

>[AZURE.TIP] 从中运行*python_tutorial_client.py*脚本`azure-batch-samples/Python/Batch/article_samples`目录。 它使用相对路径的`common.helpers`模块导入，因此，您可能会看到`ImportError: No module named 'common'`如果不运行此目录中的脚本。

典型的执行时间是**大约 5-7 分钟**，在其默认配置运行示例时。

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>下一步行动

随意更改*python_tutorial_client.py*和*python_tutorial_task.py*尝试各种不同的计算方案。 例如，尝试将执行延迟添加到*python_tutorial_task.py*来模拟长时间运行的任务，并监视它们的门户。 尝试添加更多的任务或调整计算节点的数目。 添加逻辑以检查并允许加快执行现有池的使用。

既然您已熟悉一个批次的解决方案的基本工作流程，就应该深入了解到批服务的其他功能。

- 查看[概述的 Azure 批处理功能](batch-api-basics.md)文章中，我们建议如果你新到的服务。
- 对其他批次开发文章，**开发深入**[批学习路径]下启动[batch_learning_path]。
- 签出处理与[TopNWords]中的批次的"前 N 位词"工作负荷的不同实现[github_topnwords]的示例。

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "在 Azure 存储中创建容器"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "将任务应用程序和输入 （数据） 的文件上载到容器"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "创建批处理池"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "创建批处理作业"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "将任务添加到作业"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "监视任务"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "从存储下载任务输出"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "批次解决方案工作流 （完整的关系图）"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "在门户网站中的批次凭据"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "存储在门户网站中的凭据"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "批次解决方案工作流 （小图）"
