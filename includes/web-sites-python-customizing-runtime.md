Azure 将确定用于其虚拟环境中使用以下优先级的 Python 版本︰

1. 在根文件夹中的 runtime.txt 中指定的版本
1. Python 在 web 应用程序配置的设置中指定的版本 (**设置** > 在 Azure 门户 web 应用程序的**应用程序设置**刀片)
1. python 2.7 是默认值，如果未指定任何上述

有效值的内容 

    \runtime.txt

包括︰

- python 2.7
- python 3.4

如果微版本指定 （第三位），它将被忽略。
