某些软件包可能使用 pip 在 Azure 上运行时无法安装。  它可能只是包不可用 Python 包索引上。  它可能是一个编译器是必需 （编译器不是 Azure 应用程序服务中运行 web 应用程序的计算机上可用的）。

在本节中，我们将看方法可以解决此问题。

### <a name="request-wheels"></a>请求的车轮

如果软件包安装需要编译器，您应尝试联系包所有者请求轮可供包。

有最新的[Microsoft Visual C++ 编译器，对于 Python 2.7][]，现容易生成对于 Python 2.7 具有本机代码的包。

### <a name="build-wheels-requires-windows"></a>建立车轮 （需要 Windows）

注意︰ 使用此选项时，请确保编译使用 Python 环境版本相匹配的平台/体系结构/在 Azure 应用程序服务 web 应用程序上用于包 （Windows/32-bit/2.7 或 3.4）。

如果因为它需要编译器，没有安装包，可以在您的本地计算机上安装编译器和生成轮的包，然后将包含在您的存储库中。

Mac/Linux 用户︰ 如果您没有访问权限的 Windows 计算机，请参阅如何在 Azure 上创建一个虚拟机[创建虚拟机运行 Windows][] 。  可以使用它来生成车轮、 将其添加到存储库，和如果您愿意放弃 VM。 

对于 Python 2.7，您可以安装[Microsoft Visual C++ 编译器，对于 Python 2.7][]。

对于 Python 3.4 可以安装[Microsoft Visual C++ 2010年表达][]。

若要生成车轮，您将需要轮包︰

    env\scripts\pip install wheel

您将使用`pip wheel`进行编译依赖项︰

    env\scripts\pip wheel azure==0.8.4

这将创建的 \wheelhouse 文件夹中的.whl 文件。  将 \wheelhouse 文件夹和轮文件添加到您的存储库。

编辑您要添加的 requirements.txt`--find-links`顶部选项。 这会告诉 pip 转到 python 包索引之前精确匹配的本地文件夹中查找。

    --find-links wheelhouse
    azure==0.8.4

如果您想要包括在 \wheelhouse 文件夹中的所有依赖关系和根本不使用 python 包索引，您可以强制 pip 忽略包索引加上`--no-index`为您 requirements.txt 的顶部。

    --no-index

### <a name="customize-installation"></a>自定义安装

您可以自定义部署脚本，在虚拟环境中轻松如使用备用安装程序，安装程序包\_安装。  Deploy.cmd 被注释掉的示例，请参阅。  请确保此类软件包不列入 requirements.txt，以防止 pip 安装它们。

添加到部署脚本︰

    env\scripts\easy_install somepackage

您可能还能够使用容易\_从 exe 安装程序分发安装到安装 (一些是 zip 兼容，那么容易\_安装支持它们)。  将安装程序添加到您的存储库，并方便地调用\_通过将路径传递给可执行文件安装。

添加到部署脚本︰

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>在存储库中 （需要 Windows） 中包含虚拟环境

注︰ 使用此选项时，请确保使用虚拟环境版本相匹配的平台/体系结构/在 Azure 应用程序服务 web 应用程序上使用 （Windows/32-bit/2.7 或 3.4）。

如果存储库中包含的虚拟环境，您可以防止部署脚本在 Azure 上的虚拟环境管理通过创建一个空文件︰

    .skipPythonDeployment

我们建议您删除现有的虚拟环境应用程序，以防止自动管理虚拟环境中的剩余文件。


[创建一个运行 Windows 虚拟机]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Python 2.7 的 Microsoft Visual C++ 编译器]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010年速成版]: http://go.microsoft.com/?linkid=9709949
