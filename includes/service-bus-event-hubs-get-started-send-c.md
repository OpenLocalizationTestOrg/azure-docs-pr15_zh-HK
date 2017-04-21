## <a name="send-messages-to-event-hubs"></a>将消息发送到事件集线器

在本节中，我们将编写一个 C 应用程序，将事件发送到事件中心。 我们将使用[Apache Qpid 项目](http://qpid.apache.org/)的质子 AMQP 库。 这是类似于使用服务总线队列和主题与 AMQP 从 C 作为显示[在此处](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504)。 有关详细信息，请参阅[Qpid 质子文档](http://qpid.apache.org/proton/index.html)。

1. 从[Qpid AMQP 信使页](http://qpid.apache.org/components/messenger/index.html)中，单击**安装 Qpid 质子**链接并按照说明进行操作，具体取决于您的环境。 我们将假定 Linux 环境;例如， [Azure Linux 虚拟机](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md)使用 Ubuntu 14.04。

2. 若要编译的质子库，安装以下软件包︰

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. 下载[Qpid 质子库](http://qpid.apache.org/proton/index.html)的库中，并进行解压缩，如︰

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. 创建生成目录、 编译和安装︰

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. 在您的工作目录中创建名为**sender.c**使用以下内容的新文件。 替换为事件中心的名称和命名空间名称的值，请务必 (后者通常是`{event hub name}-ns`)。 此外必须替换前面创建的**SendRule**键的 URL 编码的版本。 您可以进行 URL 编码它[这里](http://www.w3schools.com/tags/ref_urlencode.asp)。

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. 编译该文件， **gcc**假设︰

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] 在此代码中，我们使用 1 传出窗口尽可能快地强制出的消息。 一般情况下，您的应用程序应尝试批邮件来增加吞吐量。 有关如何使用 Qpid 质子库和其他环境中，并从平台为其提供了绑定的详细信息，请参阅[Qpid AMQP 信使页面](http://qpid.apache.org/components/messenger/index.html)（目前 Perl PHP，Python，以及拼音）。
