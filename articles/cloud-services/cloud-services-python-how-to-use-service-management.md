<properties
    pageTitle="如何使用服务管理 API (Python)-功能指南"
    description="了解如何以编程方式从 Python 中执行公共服务管理任务。"
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>如何使用 Python 中的服务管理

> [AZURE.NOTE] 正在使用新的资源管理 API，当前可用的预览版本中更换服务管理 API。  使用新的资源管理 API 从 Python 看到[Azure 资源管理文档](http://azure-sdk-for-python.readthedocs.org/)了解详细信息。

本指南介绍了如何以编程方式从 Python 中执行公共服务管理任务。 在[Python 的 Azure SDK](https://github.com/Azure/azure-sdk-for-python)中的**ServiceManagementService**类支持大部分在[Azure 经典门户]中可用的服务与管理相关功能以编程方式访问[management-portal]（例如**创建、 更新和删除云服务、 部署、 数据管理服务和虚拟机**）。 此功能可用于构建应用程序需要以编程方式访问服务管理。

## <a name="WhatIs"></a>什么是服务管理
服务管理 API 提供了很多可通过[Azure 的传统门户网站]的服务管理功能的编程访问[management-portal]。 Python 的 Azure SDK 允许您管理您的云服务和存储帐户。

若要使用服务管理 API，您需要[创建一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="Concepts"></a>概念
Python 的 Azure SDK 包装[Azure 服务管理 API][svc-mgmt-rest-api]，这是 REST API。 API 的所有操作都是通过 SSL 和相互身份验证使用 X.509 v3 证书。 可能会从内部服务运行的任何应用程序可以发送 HTTPS 请求和接收 HTTPS 响应在 Azure，或直接通过 Internet 访问管理服务。

## <a name="Installation"></a>安装

本文中介绍的所有功能都均可在`azure-servicemanagement-legacy`包，您可以将安装使用骰。 安装 （例如，如果您不熟悉 Python），有关详细信息，请参阅这篇文章为︰[安装 Python 和 Azure SDK](../python-how-to-install.md)

## <a name="Connect"></a>如何︰ 连接到服务管理
要连接到的服务管理终结点，您需要 Azure 的订阅 ID 和有效管理证书。 您可以获得您的订阅 ID 通过[Azure 的传统门户网站][management-portal]。

> [AZURE.NOTE] 由于 Python v0.8.0 的 Azure SDK，现在是可以使用在 Windows 上运行时，使用 OpenSSL 创建的证书。  它要求 Python 2.7.4 或更高版本。 我们建议用户因为.pfx 将有可能在将来删除证书的支持而不是.pfx，使用 OpenSSL。

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>在 Windows/Mac Linux (OpenSSL) 上管理证书
您可以使用[OpenSSL](http://www.openssl.org/)来创建管理证书。  实际上，您需要创建两个证书，一个用于服务器 (`.cer`文件)，另一个用于客户端 (`.pem`文件)。 若要创建`.pem`文件，执行︰

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

若要创建`.cer`证书，执行︰

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

关于 Azure 的证书的详细信息，请参阅[证书 Azure 云服务概述](./cloud-services-certs-create.md)。 OpenSSL 参数的完整说明，请参阅文档，网址[http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)。

在创建这些文件之后，您需要上载`.cer`Azure 通过[Azure 的传统门户网站]的"设置"选项卡上的"上载"操作的文件[management-portal]，并且您需要记下保存`.pem`文件。

获得订阅 ID 之后，创建一个证书，并将上载`.cer`文件到 Azure，可以通过订阅 id 和路径连接的 Azure 管理端点`.pem`到**ServiceManagementService**文件︰

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

在前面的示例中，`sms`是一个**ServiceManagementService**对象。 **ServiceManagementService**类是用于管理 Azure 服务的主要类。

### <a name="management-certificates-on-windows-makecert"></a>在窗口 (MakeCert) 上管理证书

您可以在您的计算机使用创建自行签署式管理证书`makecert.exe`。  打开作为**管理员** **Visual Studio 命令提示符**并使用下面的命令，您想要使用的证书名称替换为*AzureCertificate* 。

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

命令创建`.cer`文件中，并将其安装在**个人**证书存储区。 有关更多详细信息，请参阅[证书 Azure 云服务的概述](./cloud-services-certs-create.md)。

创建证书之后，您需要上载`.cer`文件到[Azure 的传统门户网站]的"设置"选项卡上的"上载"操作通过 Azure[management-portal]。

获得订阅 ID 之后，创建一个证书，并将上载`.cer`文件到 Azure，您可以连接到 Azure 管理端点通过将订阅 id 和证书的位置在您的**个人**证书存储区中传递给**ServiceManagementService** （再次，替换*AzureCertificate*您的证书的名称）︰

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

在前面的示例中，`sms`是一个**ServiceManagementService**对象。 **ServiceManagementService**类是用于管理 Azure 服务的主要类。

## <a name="ListAvailableLocations"></a>如何︰ 列出可用位置

若要列出可用于承载服务的位置，请使用**列表\_位置**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

创建云服务或存储服务时，您需要提供一个有效的位置。 **列表\_位置**方法始终返回当前可用位置的最新列表。 截止到撰写本文时，可用的位置是︰

- 西欧
- 北欧
- 东南亚
- 东亚
- 美国中部
- 美国中北部
- 美国中南部
- 美国西部
- 东亚的美国
- 日本东
- 日本西部
- 巴西南部
- 澳大利亚东部
- 澳大利亚东南部

## <a name="CreateCloudService"></a>如何︰ 创建一个云服务

当您创建应用程序并运行在 Azure 中时，代码和配置在一起称为 （称为*承载服务*在早期版本中 Azure） Azure[云服务][]。 **创建\_承载\_服务**方法允许您通过提供的托管的服务名必须是唯一在 Azure）、 （自动为 base64 编码） 的标签，说明和一个位置创建新的托管的服务。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

您可以列出与您的订阅的所有托管的服务**列表\_承载\_服务**方法︰

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

如果您想要获取有关特定的托管服务的信息，您可以做到通过传递到托管的服务名称**获得\_承载\_服务\_属性**方法︰

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

创建云服务之后，可以将代码部署到具有服务**创建\_部署**方法。

## <a name="DeleteCloudService"></a>如何︰ 删除云服务

您可以通过传递给该服务名称删除云服务**删除\_承载\_服务**方法︰

    sms.delete_hosted_service('myhostedservice')

您可以删除服务之前，必须先删除所有部署服务。 (请参阅[如何︰ 删除部署](#DeleteDeployment)的详细信息。)

## <a name="DeleteDeployment"></a>如何︰ 删除部署

若要删除部署，请使用**删除\_部署**方法。 下面的示例演示如何删除名为部署`v1`。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>如何︰ 创建存储服务

[存储服务](../storage/storage-create-storage-account.md)使您可以访问到 Azure [Blob](../storage/storage-python-how-to-use-blob-storage.md)、[表](../storage/storage-python-how-to-use-table-storage.md)和[队列](../storage/storage-python-how-to-use-queue-storage.md)。 若要创建存储服务，需要服务 （介于 3 到 24 小写字符和 Azure 中唯一）、 说明、 标签 （最多 100 个字符，自动编码为 base64），以及一个位置的名称。 下面的示例演示如何通过指定一个位置，创建存储服务。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

注意在上面的示例中，状态的**创建\_存储\_帐户**操作可通过传递所返回的结果**创建\_存储\_帐户**到**获取\_操作\_状态**方法。  

您可以列出存储帐户和它们的属性与**列表\_存储\_帐户**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>如何︰ 删除存储服务

您可以通过传递到存储服务名称删除存储服务**删除\_存储\_帐户**方法。 删除存储服务将删除服务 （blob、 表和队列） 中存储的所有数据。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>如何︰ 列出可用操作系统

若要列出可用于承载服务的操作系统，请使用**列表\_操作系统\_系统**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

或者，您可以使用**列表\_操作系统\_系统\_系列**系列操作系统进行分组的方法︰

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>如何︰ 创建的操作系统映像

若要将操作系统映像添加到映像存储库，使用**添加\_os\_图像**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

若要列出操作系统映像可用，请使用**列表\_os\_图像**方法。 它包括所有平台映像和用户图像︰

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"></a>如何︰ 删除操作系统映像

若要删除用户图像，请使用**删除\_os\_图像**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>如何︰ 创建虚拟机

若要创建虚拟机，您首先需要创建一个[云服务](#CreateCloudService)。  然后，创建虚拟机的部署使用**创建\_虚拟\_机\_部署**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"></a>如何︰ 删除虚拟机

若要删除虚拟机，您先删除部署使用**删除\_部署**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

云服务可以删除使用**删除\_承载\_服务**方法︰

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>如何︰ 从捕获的虚拟机映像创建虚拟机

若要捕获虚拟机映像，首先调用**捕获\_虚拟机\_图像**方法︰

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

接下来，以确保您已成功捕获图像，使用**列表\_虚拟机\_图像**api，并确保您的图像将显示在结果中︰

    images = sms.list_vm_images()

最后创建虚拟机使用捕获的图像，使用**创建\_虚拟\_机\_部署**的方法，因为在这之前，但这次在 vm_image_name 改为传递

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

若要了解有关如何捕获 Linux 虚拟机的详细信息，请参阅[如何捕获 Linux 虚拟机。](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

若要了解有关如何捕获 Windows 虚拟机的详细信息，请参阅[如何捕获 Windows 虚拟机。](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"></a>下一步行动

现在，您已经学习了服务管理的基本知识，可以访问[完整的 API 参考文档的 Azure Python sdk](http://azure-sdk-for-python.readthedocs.org/)并执行复杂的任务，很容易以管理 python 应用程序。

有关详细信息，请参阅[Python 开发中心](/develop/python/)。

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[云服务]:/services/cloud-services/

