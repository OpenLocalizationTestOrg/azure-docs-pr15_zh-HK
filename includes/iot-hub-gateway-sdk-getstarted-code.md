## <a name="typical-output"></a>典型输出

下面是输出的为日志文件编写的 Hello World 示例示例。 已添加新行和制表符可读性较高︰

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>代码段

本部分讨论 Hello World 示例中的代码的关键部分。

### <a name="gateway-creation"></a>网关创建

开发人员必须编写该*网关过程*。 此程序创建内部基础结构 （代理）、 加载模块，并将一切都设置才能正确工作。 SDK 提供的**Gateway_Create_From_JSON**函数，使您能够引导从 JSON 文件网关。 可以使用**Gateway_Create_From_JSON**函数必须指定要加载的模块的 JSON 文件中将其传递路径。 

您可以查找的代码[main.c]中的 Hello World 示例中的网关过程[lnk-main-c]文件。 为提高清晰度，下面的代码段显示了网关过程代码的缩写的版本。 该程序创建一个网关，然后等待用户按**ENTER**键之前它销毁该网关。 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

JSON 设置文件包含要加载的模块的列表。 每个模块必须指定答︰

- **模块名**︰ 模块的唯一名称。
- **module_path**︰ 包含模块的库的路径。 Linux 是一个.so 文件，在 Windows 上，这是一个.dll 文件。
- **参数**︰ 模块需要的任何配置信息。

JSON 文件还包含将传递到该中介模块之间的链接。 链接具有两个属性︰
- **来源**︰ 从一个模块名称`modules`部分，或"\*"。
- **接收器**︰ 从一个模块名称`modules`节

每个链接定义消息路由和方向。 消息从模块`source`是被传递到该模块`sink`。 `source`可能设置为"\*"，表示任何模块中的邮件，就会收到`sink`。

下面的示例显示用于配置 Linux 上的 Hello World 示例 JSON 设置文件。 每封邮件生成模块的`hello_world`由模块`logger`。 是否有一个模块需要参数取决于模块的设计。 在此示例中，该记录器模块采用参数是输出文件的路径和 Hello World 模块不带任何参数︰

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>大家好世界模块消息发布

您可以找到"你好世界"模块用于在[hello_world.c]发布消息的代码[lnk-helloworld-c]文件。 下面的代码段显示了带有附加注释和某些错误处理代码可读性较高删除已修正的版本︰

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>问候世界消息处理模块

Hello World 模块永远不需要处理任何其他模块发布到该中介的消息。 这样，消息回调的实现，Hello World 模块中的 no op 函数。

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>记录器模块消息发布和处理

记录器模块从代理接收消息，并将其写入到文件。 它永远不会发布任何消息。 因此，记录器模块的代码永远不会调用**Broker_Publish**函数。

[Logger.c]中的**Logger_Recieve**函数[lnk-logger-c]文件是代理程序调用以将邮件传递到记录器模块的回调。 下面的代码段显示了带有附加注释和某些错误处理代码可读性较高删除已修正的版本︰

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>下一步行动

若要了解有关如何使用 IoT 网关 SDK，请参阅以下内容︰

- [IoT 网关 SDK-发送设备到云与模拟设备使用 Linux][lnk-gateway-simulated]。
- [Azure IoT 网关 SDK] [lnk-gateway-sdk]在 GitHub 上。

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md