## <a name="view-device-telemetry-in-the-dashboard"></a>在仪表板中查看设备遥测

远程监视解决方案中的仪表板使您能够查看您的设备发送到 IoT 集线器的遥测数据。

1. 在浏览器中，返回到远程监控解决方案面板，单击左侧面板要导航到的**设备列表**中的**设备**。

2. 在**设备列表**中，您应看到您的设备的状态现**运行**。

    ![][18]

3. 单击**仪表板**以返回到仪表板，在**设备连接到视图**下拉列表以查看其遥测中选择您的设备。 示例应用程序从遥测是 50 个单位的内部温度、 55 单位外部温度和湿度为 50 个单位。 请注意，默认情况下显示的仪表板仅温度和湿度值。

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>将命令发送到您的设备

远程监视解决方案中的仪表板，您可以将命令发送到您通过 IoT 集线器的设备。 例如，在远程监视解决方案可以发送命令来设置一个设备的内部温度。

1. 远程监视解决方案在仪表板中，在导航到的**设备列表**左侧面板中单击**设备**。

2. 单击您的**设备列表**中的设备的**设备 ID** 。

3. 在**设备详细信息**面板中，单击**命令**。

    ![][13]

4. 在**命令**下拉列表，选择**SetTemperature**，并在**温度**输入新的温度值。 单击**发送命令**将命令发送到设备。

    ![][14]

    > [AZURE.NOTE] 命令历史记录最初显示为**挂起**的命令状态。 当设备确认该命令时，状态将变为**成功**。

5. 在仪表板，请验证设备现在新的温度值作为发送 75。

## <a name="next-steps"></a>下一步行动

[自定义预配置解决方案]的文章[lnk-customize]描述了您可以扩展此示例的一些方法。 可能的扩展包括使用真实的传感器和执行其他命令。

您可以了解更多有关[在 azureiotsuite.com 网站上的权限][lnk-permissions]。

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
