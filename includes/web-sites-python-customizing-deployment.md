Azure 将确定应用程序使用 Python**如果满足两个条件**︰

- requirements.txt 在根文件夹中的文件
- 根文件夹中的任何.py 文件或指定 python 的 runtime.txt

当这种情况下时，它将使用 Python 特定部署脚本，执行标准同步的文件，以及其他的 Python 操作，如︰

- 自动虚拟环境的管理
- 列出在使用 pip 的 requirements.txt 包的安装
- 根据所选的 Python 版本相应 web.config 的创建。
- 收集 Django 应用程序静态的文件

无需自定义脚本，您可以控制默认部署步骤的某些方面。

如果您想要跳过所有的 Python 具体的部署步骤，可以创建空文件︰

    \.skipPythonDeployment

为部署更多的控制，可以通过创建以下文件来覆盖默认部署脚本︰

    \.deployment
    \deploy.cmd

可以使用[Azure 的命令行界面][]来创建文件。  从项目文件夹中使用以下命令︰

    azure site deploymentscript --python

如果这些文件不存在，Azure 创建临时部署脚本并运行。  它等同于使用上面的命令创建一个。

[Azure 的命令行界面]: http://azure.microsoft.com/downloads/
