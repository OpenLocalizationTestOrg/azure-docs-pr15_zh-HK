<properties
 pageTitle="获取 Azure 工具 (macOS 10.10) |Microsoft Azure"
 description="在 macOS 上安装 Python 和 Azure 命令行界面 (Azure CLI)。"
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="21-get-azure-tools-macos-1010"></a>2.1 获得 Azure 工具 (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 将做什么

安装 Azure 命令行界面 (CLI Azure)。 如果您遇到任何问题，寻求解决方案的[故障排除页](iot-hub-raspberry-pi-kit-node-troubleshooting.md)。

## <a name="212-what-you-will-learn"></a>2.1.2 将学到的

- 如何安装 Azure CLI。
- 如何添加 Azure CLI IoT 子群。

## <a name="213-what-you-need"></a>2.1.3 所需

- Mac 与互联网连接
- 有效的 Azure 预订。 如果您没有帐户，您可以在几分钟创建一个[免费帐户](https://azure.microsoft.com/free/)。

## <a name="214-install-python"></a>2.1.4 安装 Python

虽然 macOS 有现成的 Python 2.7，建议安装 Homebrew 通过 Python。 请参阅[安装在 macOS 的 Python](http://docs.python-guide.org/en/latest/starting/install/osx/)。

通过运行以下命令来安装 Python 和 pip:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 安装 Azure CLI

Azure CLI 提供多平台命令行体验 Azure，使您可以直接从命令行使用资源调配和管理资源。 

若要安装最新的 Azure CLI，请执行以下步骤︰

1. 在终端窗口中运行以下命令。 它可能需要花费 5 分钟才能安装 Azure CLI。

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. 通过运行以下命令来验证安装︰

    ```bash
    az iot -h
    ```
  
如果安装不成功，您应看到下面的输出。

![az iot-h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 摘要

您已经安装了 Azure CLI。 请转到下一节，创建使用 Azure CLI Azure IoT 中心和设备标识。

## <a name="next-steps"></a>下一步行动

[2.2 创建 IoT 集线器并注册您 Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
