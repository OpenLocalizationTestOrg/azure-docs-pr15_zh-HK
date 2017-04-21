## <a name="download-install-and-register-the-azure-backup-agent"></a>下载、 安装和注册 Azure 备份代理

创建后 Azure 备份存储库，应在每个备份的数据和应用程序到 Azure 使您 Windows 计算机 （Windows 服务器的 Windows 客户端，系统中心 Data Protection Manager 服务器、 Azure 备份服务器计算机） 上安装代理。

1. 登录到[管理门户](https://manage.windowsazure.com/)

2. 单击**恢复服务**，然后选择您想要向服务器注册备份存储库。 将出现该备份的电子仓库快速启动页。

    ![快速入门](./media/backup-install-agent/quickstart.png)

3. 在快速启动页上，单击下**下载代理****的 Windows 服务器或系统中心 Data Protection Manager 或 Windows 客户端**选项。 单击**保存**以将其复制到本地计算机。

    ![保存代理](./media/backup-install-agent/agent.png)

4. 安装代理后，双击 MARSAgentInstaller.exe 启动 Azure 备份代理程序的安装。 选择安装文件夹和暂存文件夹所需的代理。 指定的缓存位置必须有可用空间，即至少 5%的备份数据。

5.  如果您使用代理服务器连接到互联网，在**代理配置**屏幕中，输入代理服务器的详细信息。 如果使用经过身份验证的代理服务器，请在此屏幕中输入用户名称和密码的详细信息。

6.  Azure 备份代理安装.NET Framework 4.5 和 Windows PowerShell （如果还没有提供的话） 来完成安装。

7.  安装代理后，单击继续执行工作流**继续对注册**按钮。

    ![注册](./media/backup-install-agent/register.png)

8. 在存储库凭据屏幕中，浏览到并选择以前下载的存储库凭据文件。

    ![保险存储的凭据](./media/backup-install-agent/vc.png)

    存储库凭据文件是仅适用于 48 小时 （后从门户网站上下载）。 如果您遇到此屏幕 （如"保险存储凭据提供文件已过期"），登录到 Azure 的门户网站中的任何错误，并且再次下载存储库凭据文件。

    确保存储库凭据文件在安装应用程序可以访问这些位置可用。 如果您遇到访问相关的错误，将存储库凭据文件复制到此计算机中的临时位置并重试该操作。

    如果您遇到一个无效的存储库凭据错误 （例如"提供凭据无效的保险存储"） 该文件已损坏，或者不具有最新的凭据与恢复服务无关。 重试后从门户网站下载新的存储库凭据文件操作。 如果用户单击中的 Azure 门户，连续快速地**下载存储库凭据**选项，通常出现此错误。 在这种情况下，只有第二个存储库凭据文件是有效的。

9. 在**加密设置**屏幕中，可以生成密码短语，或提供一个密码短语 （16 个字符的最小值）。 请记住要将密码保存在安全的位置。

    ![加密](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] 如果密码丢失或忘记;Microsoft 不能帮助恢复备份的数据。 最终用户拥有的加密密码，Microsoft 不具有可视性由最终用户所使用的密码。 请在安全的位置保存该文件根据需要在恢复操作过程。

10. 当您单击**完成**按钮时，机器成功注册到该存储库，您现在就可以开始备份到 Microsoft Azure。

11. 使用 Microsoft Azure 备份独立时就可以注册工作流过程中通过单击在 Azure 备份 mmc 管理单元中的**更改属性**选项指定的设置。

    ![更改属性](./media/backup-install-agent/change.png)

    使用 Data Protection Manager，您可以修改在登记工作流过程中通过选择**联机****管理**选项卡下单击**配置**选项指定的设置。

    ![配置 Azure 的备份](./media/backup-install-agent/configure.png)
