1. 在 Visual Studio**解决方案资源管理器**中右击该项目，然后选择**添加 > Docker 支持**从上下文菜单。

    ![添加 Docker 支持上下文菜单](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Web 项目添加到 ASP.NET 核心 Docker 支持导致 Docker 相关的几个文件添加到项目中，包括 Docker 撰写文件、 部署 Windows PowerShell 脚本和 Docker 属性文件添加。 

    ![Docker 文件添加到项目中](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]如果使用[为 Windows Docker beta 版](https://beta.docker.com)，打开 Properties\Docker.props，删除默认值并重新启动 Visual Studio 的值才会生效。
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
