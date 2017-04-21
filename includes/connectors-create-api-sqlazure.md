### <a name="prerequisites"></a>系统必备组件
- Azure 帐户;您可以创建[免费的帐户](https://azure.microsoft.com/free)
- [SQL Azure 数据库](../articles/sql-database/sql-database-get-started.md)与它连接的信息，包括服务器名称、 数据库名称和用户名/密码。 SQL 数据库的连接字符串中包括此信息︰
  
    服务器 = tcp:*yoursqlservername*。 database.windows.net,1433;Initial 目录 =*yourqldbname*;持久性安全信息为 False;用户 ID = {your_username};密码 = {your_password};MultipleActiveResultSets = False;加密 = True;TrustServerCertificate = False;连接超时值 = 30;

    阅读有关[Azure SQL 数据库](https://azure.microsoft.com/services/sql-database)的详细信息。

> [AZURE.NOTE] 当您创建一个 Azure 的 SQL 数据库时，您还可以创建包含 SQL 的示例数据库。 



逻辑应用程序中使用 SQL Azure 数据库，然后连接到 SQL 数据库。 在 Azure 的门户网站上的逻辑应用程序可以轻松地执行此操作。  

连接到 Azure SQL 数据库使用以下步骤︰  

1. 创建一个逻辑应用程序。 逻辑应用程序设计器中，添加触发器，然后再添加一个操作。 在下拉列表中，选择**显示 Microsoft 托管 Api** ，然后在搜索框中输入"sql"。 选择某项操作︰  

    ![SQL Azure 连接创建步骤](./media/connectors-create-api-sqlazure/sql-actions.png)

2. 如果您以前没有创建任何连接到 SQL 数据库，则会提示您有关连接的详细信息︰  

    ![SQL Azure 连接创建步骤](./media/connectors-create-api-sqlazure/connection-details.png) 

3. 输入 SQL 数据库的详细信息。 带星号的属性是必需的。

    | 属性 | 详细信息 |
|---|---|
| 通过网关连接 | 保留未选中状态。 这用于连接到本地 SQL Server 时。 |
| 连接名称 * | 输入您的连接的任何名称。 | 
| SQL Server 名称 * | 输入服务器的名称;这是类似于*servername.database.windows.net*。 服务器名称是显示在 SQL 数据库属性在 Azure 的门户中，并还显示在连接字符串中。 | 
| SQL 数据库名称 * | 请输入您的 SQL 数据库的名称。 在连接字符串中的 SQL 数据库属性中列出了这些︰ Initial Catalog =*yoursqldbname*。 | 
| 用户名 * | 输入您创建时创建 SQL 数据库的用户名。 Azure 门户中的 SQL 数据库属性中列出了这些。 | 
| 密码 * | 输入创建 SQL 数据库时创建的密码。 | 

    这些凭据用于授权逻辑应用程序连接，并访问您的 SQL 数据。 完成后，您连接的详细信息与以下类似︰  

    ![SQL Azure 连接创建步骤](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. 选择**创建**。 

5. 注意到已创建的连接。 现在，继续逻辑应用程序中的其他步骤︰ 

    ![SQL Azure 连接创建步骤](./media/connectors-create-api-sqlazure/table.png)