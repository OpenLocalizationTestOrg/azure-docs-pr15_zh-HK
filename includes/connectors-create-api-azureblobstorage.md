### <a name="prerequisites"></a>系统必备组件
- Azure 帐户;您可以创建[免费的帐户](https://azure.microsoft.com/free)
- 包括存储帐户名称和访问键[Azure Blob 存储帐户](../articles/storage/storage-create-storage-account.md)。 此信息已列在 Azure 门户中存储帐户的属性。 阅读有关[Azure 存储](../articles/storage/storage-introduction.md)详细信息。

逻辑应用程序中使用您的 Azure Blob 存储帐户之前, 连接到 Azure Blob 存储帐户。 在 Azure 的门户网站上的逻辑应用程序可以轻松地执行此操作。  

连接到 Azure Blob 存储帐户使用以下步骤︰  

1. 创建一个逻辑应用程序。 逻辑应用程序设计器中，添加触发器，然后再添加一个操作。 在下拉列表中，选择**显示 Microsoft 托管 Api** ，然后在搜索框中输入"blob"。 选择某项操作︰  

    ![Azure Blob 存储连接创建步骤](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. 如果以前没有创建任何连接到 Azure 存储，则会提示您有关连接的详细信息︰   

    ![Azure Blob 存储连接创建步骤](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. 输入存储帐户详细信息。 带星号的属性是必需的。

    | 属性 | 详细信息 |
|---|---|
| 连接名称 * | 输入您的连接的任何名称。 |
| Azure 存储帐户名称 * | 输入存储帐户名。 存储的帐户名称将显示在 Azure 门户中的存储属性。 |
| Azure 存储帐户访问键 * | 输入存储帐户密钥。 访问键将显示在 Azure 门户中的存储属性。 |

    这些凭据用于授权逻辑应用程序连接，并访问您的数据。 

4. 选择**创建**。

5. 注意到已创建的连接。 现在，继续逻辑应用程序中的其他步骤︰ 

    ![Azure Blob 存储连接创建步骤](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
