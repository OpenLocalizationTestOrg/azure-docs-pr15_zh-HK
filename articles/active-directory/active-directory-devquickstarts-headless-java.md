<properties
    pageTitle="Azure AD Java 入门 |Microsoft Azure"
    description="如何生成登录用户访问 API 的 Java 命令行应用程序。"
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>使用 Java 命令行应用程序访问 API 加装了 Azure 的广告

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure 的广告使简单和容易外包的 web 应用程序的标识管理，提供单一登录和注销只有几行代码。  在 Java web 应用程序，可以完成此操作使用 Microsoft 的社区驱动的 ADAL4J 实现。

  这里我们将使用 ADAL4J 为︰
- 登录到使用 Azure 广告为身份标识提供程序的应用程序的用户。
- 显示有关该用户的某些信息。
- 对用户在该应用程序进行签名。

为此，您需要︰

1. 向应用程序注册 Azure 的广告
2. 将您的应用程序设置为使用 ADAL4J 库。
3. 使用 ADAL4J 库到 Azure AD 颁发登录和注销请求。
4. 打印出与用户有关的数据。

若要开始，[下载应用程序骨架](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip)或[下载已完成的示例](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip)。  也需要在其中应用程序注册 Azure AD 租户。  如果您还没有，[学习如何获得一个](active-directory-howto-tenant.md)。

## <a name="1--register-an-application-with-azure-ad"></a>1.使用 Azure AD 注册应用程序
若要使您的应用程序对用户进行身份验证，您首先需要在您组织中注册新的应用程序。

- 登录到 Azure 的管理门户。
- 在左侧导航中，单击**活动目录**。
- 选择租户想要注册的应用程序。
- 单击**应用程序**选项卡，然后单击添加在底部抽屉里。
- 按照提示进行操作并创建新**的 Web 应用程序和/或 WebAPI**。
    - 应用程序**名称**将介绍您给最终用户的应用程序
    - **登录 URL**为您的应用程序的基 URL。  框架的默认值是`http://localhost:8080/adal4jsample/`。
    - **应用程序 ID URI**是您的应用程序的唯一标识符。  约定是使用`https://<tenant-domain>/<app-name>`，例如`http://localhost:8080/adal4jsample/`
- 完成注册后，AAD 会将您的应用程序分配一个唯一的客户机标识符。  您将需要此值在下一节中，所以将其复制从配置选项卡。

一次在门户中为您的应用程序创建应用程序的**应用程序密码**，复制下来。  您将很快需要它。


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2.设置了您的应用程序使用 ADAL4J 库和使用 Maven 的先决条件
在这里，我们将配置 ADAL4J 使用 OpenID 连接的身份验证协议。  ADAL4J 将用于颁发登录和注销请求，管理用户的会话并获得用户有关的信息，在其他事情。

-   在项目的根目录下，打开/创建`pom.xml`，找到`// TODO: provide dependencies for Maven`并替换为以下︰

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3.创建 java PublicClient 文件

如上所示，我们将使用 Graph API 来获取有关登录的用户的数据。 为了使它更便于我们我们应创建一个文件来表示一个**目录对象**和一个单独的文件，以便可以使用 Java 的 OO 模式表示**用户**。

1. 创建名为的文件`DirectoryObject.java`我们将用来存储有关任何 DirectoryObject （您可以随意使用这以后为可执行任何其他图形查询） 的基本数据。 您可以剪切/粘贴这从下面︰

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>编译并运行示例

改回根目录下，运行以下命令以生成此示例只是放在一起使用`maven`。 这将使用`pom.xml`编写的依赖项的文件。

`$ mvn package`

您现在应该有`adal4jsample.war`文件中您`/targets`目录。 您可能修补程序部署 Tomcat 容器中并访问的 URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
它是很容易部署最新的 Tomcat 服务器与战争。 只需浏览到`http://localhost:8080/manager/`并遵照指示上载您 adal4jsample.war 文件。 它将会自动部署您使用正确的终结点。

##<a name="next-steps"></a>下一步行动

祝贺您 ！ 现在，您可以使用 Java 应用程序能够验证用户身份，安全地调用 Web Api 使用 OAuth 2.0，并获取有关用户的基本信息。  如果还没有的话，现在是时间来填充某些用户与您租户。

为了便于参考，[作为.zip 此处提供](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip)（无需您配置的值） 的完整的示例，或者您可以克隆它从 GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

