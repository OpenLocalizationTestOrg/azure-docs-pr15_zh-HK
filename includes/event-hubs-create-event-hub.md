## <a name="create-an-event-hub"></a>创建事件中心

1. 登录到[Azure 的门户网站][]，然后单击顶部的**新建**屏幕左下部。

2. 单击**数据 + 分析**，然后单击**事件集线器**。

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. 在**创建命名空间**刀片式服务器，请输入命名空间名称。 系统将立即检查该名称可用。

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. 确保命名空间名称是可用后，选择定价层 （基本或标准）。 此外，选择 Azure 订购、 资源组和要在其中创建该资源的位置。 

2. 单击**创建**以创建该命名空间。

6. 在事件集线器命名空间列表中，单击新创建的命名空间。      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. 在命名空间刀片式服务器，请单击**事件集线器**。

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. 在刀片式服务器的顶部，单击**添加事件中心**。

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. 为事件网络集线器，键入一个名称，然后单击**创建**。

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. 在事件集线器的列表中，单击新创建的事件中心名称。 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. 回到中命名空间刀片式服务器 （没有特定事件集线器刀片），单击**共享访问策略**，然后单击**RootManageSharedAccessKey**。

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. 单击复制按钮将**RootManageSharedAccessKey**连接字符串复制到剪贴板。 保存以后在本教程中使用此连接字符串。

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

现在创建事件中心，并具有您需要发送和接收事件的连接字符串。

[Azure 门户]: https://portal.azure.com/