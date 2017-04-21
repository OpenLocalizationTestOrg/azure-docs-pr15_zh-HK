# <a name="azure-technical-documentation-contributor-guide"></a>Azure 的技术文档参与者指南

您发现房屋将被发布到 Azure 文档中心的[http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)的技术文档的源的 GitHub 存储库。

该存储库还包含指导以帮助您参与我们的技术文档。  贡献者指南 》 中的文章的列表，请参见[索引](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md)。

## <a name="contribute-to-azure-documentation"></a>参与到 Azure 的文档

感谢您有兴趣 Azure 文档 ！

* [参与的方法](#ways-to-contribute)
* [行为准则导论](#code-of-conduct)
* [关于 Azure 内容做出的贡献](#about-your-contributions-to-azure-content)
* [存储库中组织](#repository-organization)
* [使用 GitHub Git，以及此存储库](#use-github-git-and-this-repository)
* [如何使用减价格式化您的主题](#how-to-use-markdown-to-format-your-topic)
* [反馈、 注释和支持](./contributor-guide/feedback-and-comments.md)
* [更多的资源](#more-resources)
* [所有贡献者指南文章索引](./contributor-guide/contributor-guide-index.md)（打开新页面）

## <a name="ways-to-contribute"></a>参与的方法 

可以在多种不同的方式为[Azure 文档](http://azure.microsoft.com/documentation/)做出贡献︰

* 参与[论坛讨论](http://social.msdn.microsoft.com/Forums/windowsazure/home)。
* 提交文章底部 Disqus 评论。
* 您可以轻松地到 GitHub 用户界面中的技术文章做出贡献。 请在此存储库中查找文章或者[http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) ，请访问文章并单击转到文章的 GitHub 来源的文章中的链接。
* 若要对现有项目进行大量更改，添加或更改图像，或造成新的文章，您需要分叉此资料库、 安装 Git 大扫除，减价板和学习某些 git 命令。

##<a name="code-of-conduct"></a>行为准则导论

此项目采用了[Microsoft 打开源的行为准则](https://opensource.microsoft.com/codeofconduct/)。 有关更多信息，请参见[执行常见问题解答代码](https://opensource.microsoft.com/codeofconduct/faq/)或联系人[opencode@microsoft.com](mailto:opencode@microsoft.com)与任何其他问题或意见。

##<a name="about-your-contributions-to-azure-content"></a>关于 Azure 内容做出的贡献

###<a name="minor-corrections"></a>次要更正

[Azure 网站的使用条款 (ToU)](http://azure.microsoft.com/support/legal/website-terms-of-use/)覆盖次要更正或您提交此 repo 中的文档和代码示例的说明。


###<a name="larger-submissions"></a>更大的提交

如果提交的新的或重大更改文档和代码示例拉请求，我们会向发送注释在 GitHub 要求您提交联机发布内容的许可证协议 (CLA)，如果您在两个组︰

* 打开的 Microsoft 技术组的成员。
* 不适用于 Microsoft 的参与者。

我们需要您填写联机表单之前我们可以接受您请求的请求。

完整细节，请访问[http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla)。

## <a name="repository-organization"></a>存储库中组织

Azure 内容存储库中的内容跟上[Azure.Microsoft.com](http://azure.microsoft.com)文档的组织结构。 该存储库包含两个根文件夹︰

### <a name="articles"></a>\articles

*\Articles*文件夹中包含的格式设置为具有*.md*扩展名的减价文件文档文章。

路径中的根目录中的文章发布到 Azure.Microsoft.com *http://azure.microsoft.com/documentation/articles/ {文章-名称-不-md} /*。

* **项目文件名︰**请参阅[我们的文件命名指南](./contributor-guide/file-names-and-locations.md)。

他们自己服务文件夹中的项目路径中发布到 Azure.Microsoft.com *http://azure.microsoft.com/documentation/articles/service-folder/ {文章-名称-不-md} /*

* **媒体子文件夹︰***\Articles*文件夹包含根目录文章的媒体文件， *\media*文件夹内的子文件夹使用的图像的每个项目。  服务文件夹包含服务的每个文件夹中的项目的单独的介质文件夹。 文章图像文件夹同名文章文件，没有*.md*文件扩展名。

### <a name="includes"></a>\includes

您可以创建可重用内容的部分包含在一个或多个项目。 请参阅[我们的技术内容中使用的自定义扩展](./contributor-guide/custom-markdown-extensions.md)。

### <a name="markdown-templates"></a>\markdown 的模板

此文件夹包含与基本减价格式所需的一篇文章我们标准的减价的模板。

### <a name="contributor-guide"></a>\contributor-guide

此文件夹包含属于我们贡献者指南 》 的文章。  

## <a name="use-github-git-and-this-repository"></a>使用 GitHub Git，以及此存储库

有关如何做出贡献、 如何使用 GitHub UI 贡献很小的更改，以及如何分叉和克隆存储库中的更大的贡献的信息，请参阅[安装和设置在 GitHub 创作工具](./contributor-guide/tools-and-setup.md)。

如果您安装 GitBash，并选择本地工作， [Git 命令创建新文章或更新现有项目](./contributor-guide/git-commands-for-master.md)中列出的步骤创建新的本地工作分支，进行更改，提交更改回主分支

### <a name="branches"></a>分支机构

我们建议您创建本地更改的特定的作用域为目标的工作分支。 每个分支应限于单一的概念/文章既能够优化工作流程并降低合并冲突的可能性。  以下工作有一个新的分支的适当范围︰

* 新的文章 （和关联的图像）
* 一篇文章的拼写和语法的编辑。
* 大量的文章 （如新版权页脚） 跨应用的单个格式设置更改。

## <a name="how-to-use-markdown-to-format-your-topic"></a>如何使用减价格式化您的主题

此存储库中的所有项目都使用 GitHub flavored 的减价。  这是资源的列表。

- [减价基础知识](https://help.github.com/articles/markdown-basics/)

- [可打印减价速查表](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- 我们的减价编辑器的列表，请参阅[工具和安装主题](./contributor-guide/tools-and-setup.md#install-a-markdown-editor)。

## <a name="article-metadata"></a>文章元数据

文章元数据实现了 azure.microsoft.com 网站上的某些功能，例如作者归属、 参与者归属、 痕迹、 文章描述和 SEO 优化，以及报告 Microsoft 用来评估内容的性能。 因此，元数据是重要的 ！ [下面是用于确保元数据指导得当](./contributor-guide/article-metadata.md)。

### <a name="labels"></a>标签

自动的标签分配中提取请求以帮助我们管理纳入请求工作流，并可帮助让您知道这怎么回事在您请求的请求︰

* 贡献与相关的许可协议
    * cla 不需要︰ 更改是相对较小，并且不需要您登录 CLA。
    * cla 要求︰ 更改范围相对较大，需要您登录 CLA。
    * cla 签名︰ 参与者签名 CLA，这样拉请求现在可以向前移动候审。
* 支柱标签︰ 标签如 PnP、 新型应用程序和 TDC 帮助分类拉请求需要审阅拉请求的内部组织。
* 更改发送创作︰ 作者已挂起拉请求的通知。

## <a name="more-resources"></a>更多的资源

请参阅[我们参与者手册的索引](./contributor-guide/contributor-guide-index.md)的所有我们的指导主题。
