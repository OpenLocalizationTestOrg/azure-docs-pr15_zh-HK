## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>通过单击要部署部署 ARM 模板

可以重复使用预定义的 ARM 模板上载到 Microsoft 维护的 github 资料库并打开到社区。 这些模板可以从 github，垂直部署或下载和修改以满足您的需要。 若要部署模板创建具有两个子网的 VNet，请按照下面的步骤。

1. 在浏览器中，定位到[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates)。
2. 模板列表中向下滚动并单击**101 vnet-两个子网**。 检查**README.md**文件，如下所示。

    ![在 github 的 READEME.md 文件](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. 单击**将部署到 Azure**。 如有必要，请输入您的 Azure 的登录凭据。 
4. 在**参数**刀片式服务器，输入您想要使用创建您新的 VNet 的值，然后单击**确定**。 下图显示了我们的方案的值。

    ![ARM 模板参数](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. 单击**资源组**并选择要添加到 VNet 的资源组或单击**新建**将 VNet 添加到新的资源组。 下图显示一个名为**TestRG**的新资源组的组设置的资源。

    ![资源组](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. 如果有必要，请更改您的 VNet 的**订阅**和**位置**设置。
6. 如果您不想看到 VNet **Startboard**在平铺视图，则禁用**附到 Startboard**。
5. 单击**Leagl 条款**、 阅读条款，并单击**购买**同意。 
6. 单击**创建**来创建 VNet。

    ![预览门户中提交的部署图块](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. 完成部署后，请单击**TestVNet** > **的所有设置** > **的子网**查看子网属性，如下所示。

    ![在预览门户创建 VNet](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)