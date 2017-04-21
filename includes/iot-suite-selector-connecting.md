> [AZURE.SELECTOR]
- [在 Windows 上的 C](../articles/iot-suite/iot-suite-connecting-devices.md)
- [在 Linux 上的 C](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [在 mbed C](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>方案概述

在这种情况下，您可以创建到远程监控[预配置的解决方案]发送以下遥测设备[lnk-what-are-preconfig-solutions]:

- 外部温度
- 内部温度
- 湿度

为简单起见，在设备上的代码生成示例值，但我们建议您通过连接到您的设备的实际传感器和发送真实的遥测扩展此示例。

若要完成本教程，您需要一个活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版][lnk-free-trial]。

## <a name="before-you-start"></a>在开始之前

为设备编写任何代码之前，必须设置远程监控预配置的解决方案，然后设置在该解决方案中的自定义新设备。

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>提供远程监控预配置的解决方案

您在本教程中创建的设备将数据发送到的[远程监控]的实例[lnk-remote-monitoring]预配置的解决方案。 如果您还没有已经提供远程监控预配置的解决方案在 Azure 帐户中的，请按照下面的步骤︰

1. 在<https://www.azureiotsuite.com/>页面上，单击**+**创建一个新的解决方案。

2. 要创建新解决方案的**远程监控**面板上，单击**选择**。

3. 在**创建远程监视解决方案**页上，输入您选择的**解决方案名称**，选择要部署到的**地区**，选择想要使用 Azure 订阅。 然后，单击**创建解决方案**。

4. 等待，直到完成设置过程。

> [AZURE.WARNING] 预配置的解决方案使用 Azure 服务的收费。 请务必从订阅中移除预配置的解决方案，以避免任何不必要的费用与其完成后。 完全可以通过访问<https://www.azureiotsuite.com/>页面从订阅中移除一个预配置的解决方案。

远程监视解决方案的资源调配过程完成后，单击**启动**以在您的浏览器中打开解决方案的仪表板。

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>设置设备上的远程监视解决方案中

> [AZURE.NOTE] 如果您已经有您的解决方案中提供设备，您可以跳过此步骤。 您需要知道的设备凭据，当您创建客户端应用程序。

为了使设备能连接到预配置的解决方案，则必须标识本身 IoT 集线器使用有效的凭据。 从解决方案面板，您可以检索设备凭据。 您在本教程的后面的客户端应用程序中包括的设备凭据。 

若要将新设备添加到远程监控解决方案，完成解决方案仪表板中的下列步骤︰

1.  在仪表板的左下角，单击**添加设备**。

    ![][1]

2.  在**自定义设备**面板中，单击**新加**。

    ![][2]

3.  选择**让我定义我自己的设备 ID**、 输入设备 ID，如**mydevice**、**检查项 ID** ，然后单击**创建**以提供设备并验证该名称不在使用中，请单击。

    ![][3]

5. 请记下的设备凭据 （设备 ID、 IoT 中心主机名和设备键），客户端应用程序需要连接到远程监视解决方案。 然后单击**完成**。

    ![][4]

6. 请确保您的设备将显示在设备部分。 设备状态为**挂起**，直到设备建立连接到远程监视解决方案。

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/