## <a name="create-an-iot-hub"></a>创建 IoT 集线器

创建模拟设备连接到一个 IoT 集线器。 以下步骤显示了如何完成这项任务通过使用 Azure 的门户。

1. 登录到[Azure 的门户网站][lnk-portal]。

2. 在 Jumpbar 中，单击**新建** > **互联网的东西** > **Azure IoT 集线器**。

    ![Azure 门户网站 Jumpbar][1]

3. 在**IoT 中心**刀片式服务器，选择 IoT 集线器配置。

    ![IoT 集线器刀片][2]

    * 在**名称**框中，输入 IoT 中心的名称。 如果**名称**是有效和可用，在**名称**框中将显示一个绿色复选标记。
    * 选择[定价和刻度层][lnk-pricing]。 本教程不需要特定的层。 对于本教程，使用免费的 F1 层。
    * 在**资源组**中创建新的资源组，或选择一个现有。 有关详细信息，请参阅[使用资源组管理 Azure 资源][lnk-resource-groups]。
    * 在**位置**选择要承载 IoT 集线器的位置。 对于本教程，请选择最接近您的位置。

4. 如果您已选择 IoT 集线器配置选项，请单击**创建**。  它可以需要几分钟的 Azure 创建 IoT 集线器。 若要检查状态，您可以监视在 Startboard 或通知面板中的进度。

    ![新 IoT 集线器状态][3]

5. 当已成功创建 IoT 集线器，请单击新的拼贴在 Azure 门户打开新 IoT 集线器刀片 IoT 网络集线器的。 记下的**主机名**中，，然后单击**共享访问策略**。

    ![新 IoT 集线器刀片][4]

6. 在**共享访问策略**刀片式服务器，单击**iothubowner**策略，并再复制记**iothubowner**刀片式服务器中的连接字符串。 有关详细信息，请参阅[访问控制][lnk-access-control]在"Azure IoT 中心开发人员指南"。

    ![共享的访问策略刀片式服务器][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
