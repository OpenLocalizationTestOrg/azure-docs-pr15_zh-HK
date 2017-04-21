<properties 
    pageTitle="连接到具有 MongoDB 的协议支持的 DocumentDB 帐户 |Microsoft Azure" 
    description="了解如何连接到一个 DocumentDB 帐户使用 MongoDB，现在可供预览的协议支持。 通过使用 MongoDB 连接字符串连接。" 
    keywords="mongodb 连接字符串"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>如何连接到 DocumentDB 帐户使用 MongoDB 的协议支持

了解如何连接到使用标准 MongoDB 连接字符串的 URI 格式的 MongoDB 的协议支持的 Azure DocumentDB 帐户。  

## <a name="get-the-accounts-connection-string-information"></a>获得该帐户的连接字符串信息

1. 在新窗口中，登录到[Azure 门户](https://portal.azure.com)。
2. 在**左侧导航**栏上的帐户刀片式服务器，单击**连接字符串**。 要导航到**刀片式服务器帐户**，请在 Jumpbar 上单击**更多服务**，单击**DocumentDB (NoSQL)** ，，然后选择使用 MongoDB 的协议支持的 DocumentDB 帐户。

    ![设置所有刀片式服务器的屏幕抓图](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. **连接字符串信息**刀片式服务器打开并已连接到该帐户使用 MongoDB，包括预先构造的连接字符串的驱动程序所需的所有信息。

    ![连接字符串刀片式服务器的屏幕抓图](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>连接字符串要求

值得注意的是 DocumentDB 支持标准 MongoDB 连接字符串的 URI 的格式，除了几个具体的要求︰ DocumentDB 帐户需要身份验证，通过 SSL 的安全通信。  因此，连接字符串的格式为︰

    mongodb://username:password@host:port/[database]?ssl=true

其中此字符串的值是在如上所示的连接字符串刀片式服务器中可用。

- 用户名 （必需）
    - DocumentDB 的帐户名
- 密码 （必填）
    - DocumentDB 帐户密码
- 主机 （必需）
    - FQDN 的 DocumentDB 帐户
- 端口 （必需）
    - 10250
- 数据库 （可选）
    - 使用连接的默认数据库
- ssl = true （必需）

例如，请考虑上面的连接字符串信息中所示的帐户。  有效的连接字符串是︰
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>MongoDB 的 C# 驱动程序连接
如上所述，所有的 DocumentDB 帐户需要身份验证和通过 SSL 的安全通信。 尽管 MongoDB 连接字符串的 URI 格式支持 ssl = true 查询字符串参数，使用使用 MongoDB C# 驱动程序需要使用 MongoClientSettings 对象时创建的 MongoClient。  有了以上的帐户信息，下面的代码段显示了如何使用"任务"数据库并连接到该帐户。

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>下一步行动


- 了解如何[使用 MongoChef](documentdb-mongodb-mongochef.md)协议与 DocumentDB 帐户到支持 MongoDB 的。
- DocumentDB 使用 MongoDB[样本](documentdb-mongodb-samples.md)的协议支持。

 
