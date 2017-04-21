## <a name="create-a-device-management-enabled-iot-hub"></a>创建设备启用 IoT 中心管理

由于 IoT 集线器设备管理是在预览中，您需要创建一个设备启用管理 IoT 集线器。 当 IoT 集线器设备管理达到一般的可用性时，本教程将被更新。 以下步骤显示了如何完成此任务使用 Azure 的门户。

1.  登录到[Azure 的门户]。
2.  在 Jumpbar 中，单击**新建**，然后单击**Internet 的事情**，，然后单击**Azure IoT 中心**。

    ![][img-new-hub]

3.  在**IoT 中心**刀片式服务器，选择 IoT 集线器配置。

    ![][img-configure-hub]

  -   在**名称**框中，输入 IoT 中心的名称。 如果**名称**是有效和可用，在**名称**框中将显示一个绿色复选标记。
  -   选择一个**价格和刻度层**。 本教程不需要特定的层。
  -   在**资源组**中创建新的资源组，或选择一个现有。 有关详细信息，请参阅[使用资源组来管理 Azure 的资源]。
  -   选定复选框**启用设备管理-预览**。
  -   在**位置**选择要承载 IoT 集线器的位置。 IoT 集线器设备管理才可用在美国东部、 北部欧洲和东南亚期间公共预览。

    > [AZURE.NOTE]如果不选中**启用设备管理**到框中，示例将不起作用。<br/>通过检查**使设备管理**，您可以创建的预览 IoT 中心仅支持在美国东部、 北部欧洲和东南亚，和不适合于生产方案。 不能迁移设备进出设备启用管理集线器。

4.  如果您已选择 IoT 集线器配置选项，请单击**创建**。 它可以需要几分钟的 Azure 创建 IoT 集线器。 若要检查状态，您可以监视在**Startboard**或**通知**面板中的进度。

    ![][img-monitor]

5.  当 IoT 中心已成功创建时，您的集线器刀片将自动打开。 记下的**主机名**中，，然后单击**共享访问策略**。

    ![][img-keys]

6.  单击**iothubowner**策略，然后复制并记**iothubowner**刀片式服务器中的连接字符串。 将其复制到您可以访问的位置以后因为您需要使用它来完成本教程。

    > [AZURE.NOTE] 在生产方案中，请确保不要使用**iothubowner**凭据。

    ![][img-connection]

您现在已经创建了一个设备启用 IoT 中心管理。 您需要要完成本教程中的连接字符串。

## <a name="create-a-device-identity"></a>创建设备标识

在本节中，您可以使用称为[IoT 中心资源管理器中]的节点工具[iot-hub-explorer]为本教程创建一个设备标识。

1. 在命令行环境中运行以下命令︰

    npm 安装 giothub-explorer@latest

2. 然后，登录到网络集线器，记住要替换运行下面的命令`{service connection string}`与以前复制 IoT 集线器连接字符串︰

    iothub 浏览器登录"{服务连接字符串}"

3. 最后，创建新设备标识调用`myDeviceId`的命令︰

    iothub 资源管理器中创建的 myDeviceId-连接字符串

记下设备连接字符串的结果。 此连接字符串为设备应用程序用于连接到作为设备 IoT 网络集线器。

![][img-identity]

请参阅[入门 IoT 中心][lnk-getstarted]若要以编程方式创建设备的标识。

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure 门户]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[使用资源组来管理 Azure 资源]: ../articles/azure-portal/resource-group-portal.md
