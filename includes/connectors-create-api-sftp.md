### <a name="prerequisites"></a>系统必备组件

- [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)帐户  


逻辑应用程序中使用 SFTP 帐户之前，您必须授权逻辑应用程序连接到您的 SFTP 帐户。 幸运的是，你可以很容易地从 Azure 门户上的逻辑应用程序内。  

以下是授权逻辑应用程序连接到您的 SFTP 帐户的步骤︰  
1. 若要创建连接到 SFTP，逻辑应用程序设计器中，在下拉列表中选择**显示 Microsoft 托管 Api** ，然后在搜索框中输入*SFTP* 。 选择**SFTP-添加或修改文件时**触发器︰  
![SFTP 连接联机映像 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. 如果您没有创建任何连接到之前的 SFTP，您将得到提示您提供 SFTP 凭据。 这些凭据将用于授权您的逻辑应用程序连接到并访问 SFTP 帐户的数据︰  
![SFTP 连接联机映像 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. 注意到已创建的连接，您现在可以自由地逻辑应用程序中的其他步骤︰   
 ![SFTP 联机连接图像 3](./media/connectors-create-api-sftp/sftp-3.png) 
