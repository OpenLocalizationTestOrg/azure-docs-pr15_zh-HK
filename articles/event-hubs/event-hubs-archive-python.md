<properties
    pageTitle="Azure 集线器存档事件演练 |Microsoft Azure"
    description="使用 Azure Python SDK 来演示如何使用事件集线器存档功能的示例。"
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>事件集线器存档演练︰ Python

事件集线器存档是可以自动发送到您选择的 Azure Blob 存储帐户您事件中心的流数据的事件集线器提供的新功能。 这更便于执行批处理操作实时流数据。 本文介绍如何使用 Python 的事件集线器存档。 有关事件集线器存档的详细信息，请参阅[概述文章](event-hubs-archive-overview.md)。

此示例使用 Azure Python SDK 来演示使用存档功能。 Sender.py 模拟环境遥测事件集线器到 JSON 格式发送。 事件中心被配置为使用存档功能来编写此数据 blob 存储在批处理中。 Archivereader.py 然后读取这些 blob 和创建的每个设备的附加文件并将该数据写入到.csv 文件。

什么将完成

1.  创建 Azure Blob 存储帐户和 blob 容器中，使用 Azure 门户

2.  创建事件中心的命名空间，使用 Azure 门户

3.  使用存档功能启用时，使用 Azure 门户创建事件中心

4.  将数据发送到事件中心使用 Python 脚本

5.  从存档中读取文件和另一个 Python 脚本对其进行处理

系统必备组件

1.  Python 2.7.x

2.  Azure 的订阅

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>创建一个 Azure 存储帐户

1.  登录到[Azure 的门户][]。

2.  在入口的左侧的导航窗格中，单击新建，然后数据 + 存储，请单击，然后单击存储帐户。

3.  填写中的字段存储帐户刀片式服务器，然后单击**创建**。

    ![][1]

4.  请参见**部署成功**的消息，请单击新的存储帐户和**重点**刀片式服务器中后，请单击**Blob**。 当**Blob 服务**刀片式服务器打开时，请单击**+ 容器**顶部。 命名容器**存档**，然后关闭**Blob 服务**刀片式服务器。

5.  单击左边的刀片式服务器中的**访问键**并复制存储帐户的名称和值为**key1**。 将这些值保存到记事本或其他临时位置。

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>创建要将事件发送到事件中心的 Python 脚本

1.  打开您喜欢 Python 编辑器，例如[Visual Studio 代码][]。

2.  创建名为**sender.py**的脚本。 此脚本将发送到事件中心的 200 个事件。 他们是在 JSON 中发送的简单环保读数。

3.  将以下代码粘贴到 sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  更新前面的代码中使用的命名空间名称和创建事件集线器命名空间时所获取的键值。

## <a name="create-a-python-script-to-read-your-archive-files"></a>创建一个 Python 脚本来读取存档文件

1.  填写刀片式服务器，然后单击**创建**。

2.  创建名为**archivereader.py**的脚本。 该脚本将读取存档文件，创建的每个设备仅对该设备将数据写入一个文件。

3.  将以下代码粘贴到 archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  请务必将您的存储帐户名称和键的相应值粘贴到调用中`startProcessing`。

## <a name="run-the-scripts"></a>运行脚本

1.  打开具有 Python 在其路径中，命令提示符窗口，然后运行以下命令以安装 Python 的必备组件包︰

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    如果您是较早版本的 azure 存储空间或 azure 您可能需要使用**-升级**选项

    您可能还需要运行以下命令 （在大多数系统上不需要）︰

    ```
    pip install cryptography
    ```

2.  将目录更改到地方保存 sender.py 和 archivereader.py，然后运行以下命令︰

    ```
    start python sender.py
    ```
    
    这将启动一个新的 Python 进程运行该发件人。

3. 现在等待几分钟来运行存档。 然后到您原始的命令窗口中键入下面的命令︰

    ```
    python archivereader.py
    ```

此存档处理器使用本地目录来存储帐户/容器从下载所有 blob。 它将处理所有不为空且为.csv 文件的结果写入到本地目录。

## <a name="next-steps"></a>下一步行动

您可以了解有关事件集线器通过访问以下链接︰

- [存档事件集线器的概述][]
- 完整的[示例应用程序，它使用事件集线器][]。
- 该[事件处理事件集线器扩展][]示例。
- [事件集线器概述][]
 

[Azure 门户]: https://portal.azure.com/
[存档事件集线器的概述]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Visual Studio 代码]: https://code.visualstudio.com/
[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[扩展事件处理事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
