## <a name="send-messages-to-event-hubs"></a>将消息发送到事件集线器

事件集线器的 Java 客户端库使用[Maven 的中央存储库](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)，Maven 项目中并在 Maven 项目文件内使用下面的依赖项声明可以引用︰    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
为不同类型的生成环境，您可以显式获得最新发行的 JAR 文件从[Maven 的中央存储库](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)或从[GitHub 上发布的分发点](https://github.com/Azure/azure-event-hubs/releases)。  

对于简单的事件发布者，导入的事件集线器客户端类的*com.microsoft.azure.eventhubs*包和*com.microsoft.azure.servicebus*包，如常见的异常与 Azure 服务总线的消息传递客户端共享的实用程序类。 

对于下面的示例中，第一次在您最喜爱的 Java 开发环境中创建新的 Maven 项目控制台/shell 应用程序。 将调用类```Send```。     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

创建事件中心时使用的值替换命名空间和事件中心的名称。

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

然后，通过将一个字符串转变为其 utf-8 字节编码创建单一事件。 我们从连接字符串创建一个新事件集线器客户端实例，然后发送邮件。   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
