<properties
   pageTitle="使用数据湖存储 Java SDK 来开发应用程序 |Microsoft Azure"
   description="使用 Azure 数据湖存储 Java SDK 来开发应用程序"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>学习如何使用 Azure 存储使用 Java 数据湖泊区

> [AZURE.SELECTOR]
- [门户网站](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API，](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

了解如何使用 Azure 数据湖存储 Java SDK 来执行基本的操作，如创建文件夹，上载和下载的数据文件，等等。有关数据湖的详细信息，请参阅[Azure 数据湖存储区](data-lake-store-overview.md)。

您可以访问 Java SDK API 文档的 Azure 数据湖在[Azure 数据湖存储 Java API 文档](https://azure.github.io/azure-data-lake-store-java/javadoc/)存储区。

## <a name="prerequisites"></a>系统必备组件

* Java 开发工具包 （JDK 7 或更高版本，并使用 Java 1.7 或更高版本）
* Azure 数据湖存储帐户。 按照在[学习如何使用 Azure 数据湖存储区使用 Azure 门户](data-lake-store-get-started-portal.md)的说明。
* [Maven](https://maven.apache.org/install.html)。 本教程使用 Maven 构建和项目依赖项。 虽然有可能不使用像 Maven 或 Gradle 生成系统生成，这些系统将是更容易管理的依赖关系。
* （可选）和像[IntelliJ 的想法](https://www.jetbrains.com/idea/download/)或[Eclipse](https://www.eclipse.org/downloads/)的 IDE 或类似。

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>如何验证使用 Azure Active Directory？

在本教程中我们使用 Azure AD 应用程序客户端密码检索 Azure Active Directory 令牌 （身份验证服务的服务）。 我们使用该标记来创建数据湖存储客户端对象执行操作的文件和目录操作。 有关说明如何使用 Azure 数据湖商店使用客户端密码进行身份验证，我们执行以下高级步骤︰

1. 创建 web 应用程序 Azure 的广告
2. 检索客户端 ID、 客户端密钥，随机密钥和令牌 Azure 广告 web 应用程序的终结点。
3. 配置数据湖存储区文件/文件夹要从您创建的 Java 应用程序中访问 Azure 广告 web 应用程序的访问。

有关如何执行这些步骤的说明，请参阅[创建活动目录应用程序](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application)。

Azure 的 Active Directory 提供了其他选项，也可以检索令牌。 您可以选择从多个不同的身份验证机制，以适合您的方案，例如，浏览器、 桌面应用程序，为分布式应用程序或运行内部部署的服务器应用程序中或在 Azure 的虚拟机中运行的应用程序。 您还可以选择从不同类型的凭据，如密码、 证书、 2 双因素身份验证等。此外，Azure Active Directory 可以与云同步内部部署 Active Directory 用户。 有关详细信息，请参阅[Azure Active Directory 的身份验证方案](../active-directory/active-directory-authentication-scenarios.md)。 

## <a name="create-a-java-application"></a>创建一个 Java 应用程序

该代码示例您逐步完成创建过程的文件在存储中，串联文件、 下载文件，并删除一些文件存储区中的可用[在 GitHub 上](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)行走。 本文的此部分将指导您完成的代码的主要部分。

1. 创建一个 Maven 项目使用[mvn 原型](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)从命令行或使用 IDE。 有关如何创建使用 IntelliJ 的 Java 项目的说明，请参见[此处](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html)。 有关如何创建使用 Eclipse 项目的说明，请参阅[此处](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm)。 

2. Maven **pom.xml**文件中添加以下依存关系。 添加下面的代码段的文本之间**\</version >**标记和**\</项目 >**标记︰

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    第一个依赖项是使用数据湖商店 SDK (`azure-datalake-store`) 从 maven 存储库。 第二个依赖项 (`slf4j-nop`) 是指定要用于此应用程序的日志记录框架。 数据湖商店 SDK 中使用[slf4j](http://www.slf4j.org/)日志记录外观，使您可以选择从大量流行的日志记录框架，类似 log4j，Java 日志记录，logback 等，或者没有日志记录。 对于本示例，我们将禁用日志记录，因此我们使用**slf4j nop**绑定。 若要在您的应用程序中使用其他日志记录选项，请参见[此处](http://www.slf4j.org/manual.html#projectDep)。

### <a name="add-the-application-code"></a>将应用程序的代码添加

有三个主要部分的代码。

1. 获取 Azure Active Directory 的标记

2. 使用该标记来创建数据湖存储客户端。

3. 使用数据湖存储客户端执行的操作。

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>步骤 1︰ 获取 Azure Active Directory 的标记。

数据湖商店 SDK 提供了方便的方法，使您可以获取与数据湖存储帐户所需的安全令牌。 但是，SDK 并不要求这些方法仅能使用。 您可以使用任何其他手段获取令牌，如使用[Azure 活动目录 SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)中或自定义代码。

若要使用数据湖商店 SDK 来获取您在前面创建的活动目录 Web 应用程序的标记，使用中的静态方法`AzureADAuthenticator`类。 Azure 活动目录 Web 应用程序的实际值替换**填写在这里**。

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>步骤 2︰ 创建一个 Azure 数据湖存储客户端 (ADLStoreClient)

创建一个[ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/)对象要求您指定数据湖存储帐户名和最后一步中生成的 Azure Active Directory 标记。 请注意，数据湖存储帐户名称必须是完全限定的域名。 例如，对于像**mydatalakestore.azuredatalakestore.net**替换**填写在这里**。

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>步骤 3︰ 使用 ADLStoreClient 来执行文件操作和目录操作

下面的代码中包含一些常用操作的示例代码的段。 您可以查看完整[数据湖存储 Java SDK API 文档](https://azure.github.io/azure-data-lake-store-java/javadoc/)的**ADLStoreClient**对象以查看其他操作。
 
请注意文件的读取和写入到使用标准 Java 流。 这意味着您可以分层任何 Java 流数据湖存储流将受益于标准的 Java 功能 （例如，打印流格式化的输出，或任何压缩或加密流为顶部等附加功能。） 之上。

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>第 4 步︰ 生成并运行应用程序

1. 要在 IDE 中运行运行，找到并按下**运行**按钮。 若要从 Maven 运行，使用[exec︰ 执行](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html)。

2. 独立 jar，您可以从命令行运行以生成包括，使用该[程序集的 Maven 插件](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html)的所有依赖项与生成 jar。 在[github 上的示例源代码](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml)pom.xml 已举例说明如何执行此操作。


## <a name="next-steps"></a>下一步行动

- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)
- [与数据湖商店使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
