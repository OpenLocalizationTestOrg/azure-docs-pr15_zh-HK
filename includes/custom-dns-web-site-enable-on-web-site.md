传播您的域名的记录后，必须将其与您的 Web 应用程序中关联。 使用以下步骤启用 web 浏览器使用的域名。

> [AZURE.NOTE] 它可能需要一些时间 TXT 记录在 DNS 系统中传播前面的步骤中创建的。 直到 TXT 记录传播，不能添加到您的 web 应用程序的域名称。 如果您使用的一条 A 记录，不能到您的 web 应用程序添加 A 记录的域名称，直到上一步中创建的 TXT 记录传播。
>
> 类似<a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a>的服务可用于验证 TXT 记录是可用的。

1. 在浏览器中，打开[Azure 门户](https://portal.azure.com)。

2. 在**Web 应用程序**选项卡，单击您的 web 应用程序的名称，然后选择**自定义的域**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. 在**自定义的域**刀片式服务器，请单击**添加主机名**。
    
4. 使用**主机名**文本框中输入此 web 应用程序相关联的域名。

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  单击**验证**。

7.  在单击**验证**Azure 的启动时间域验证工作流。 这将检查域所有权以及主机可用性并报告成功或与规范性 guidence 如何修复该错误的详细的错误。    

此时，您应该能够在浏览器中输入自定义域名并查看，它成功地将带您到您的 web 应用程序。
