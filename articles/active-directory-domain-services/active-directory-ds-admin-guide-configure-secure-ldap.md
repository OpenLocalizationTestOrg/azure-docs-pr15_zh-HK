<properties
    pageTitle="在 Azure AD 域服务配置安全 LDAP (LDAPS) |Microsoft Azure"
    description="Azure AD 域服务托管域配置安全 LDAP (LDAPS)"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Azure AD 域服务托管域配置安全 LDAP (LDAPS)
本文介绍如何为您的 Azure AD 域服务托管域启用安全轻型目录访问协议 (LDAPS)。 安全 LDAP 即所谓轻量目录访问协议 (LDAP) 通过安全套接字层 (SSL) / 传输层安全 (TLS)。

## <a name="before-you-begin"></a>在开始之前
要执行本文中列出的任务，您需要︰

1. 有效**Azure 的订阅**。

2. **Azure 的广告目录**中的任何一个与内部目录或仅云目录同步。

3. Azure 的广告目录，必须启用**azure AD 域服务**。 如果您还没有这样做，请按照[入门指南](./active-directory-ds-getting-started.md)中列出的所有任务。

4. **用于启用安全 LDAP 证书**。
    - **推荐**的从企业 CA 或公共证书颁发机构获取证书。 此配置选项将更安全。
    - 或者，您也可以选择创建[自行签署式证书](#task-1---obtain-a-certificate-for-secure-ldap)如本文后面所示。

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>安全 LDAP 证书要求
在启用安全 LDAP 之前获得下面的准则，每一个有效的证书。 如果尝试启用安全 LDAP/错误无效的证书与托管域，则会遇到失败。

1. **受信任的颁发者**的证书必须由受信任的计算机连接到使用安全 LDAP 域所需要的证书颁发机构颁发。 该颁发机构可能是您的组织的企业证书颁发机构或通过这些计算机受信任公共证书颁发机构。

2. **生存期**-该证书必须包含至少未来 3-6 个月有效。 在证书过期时中断向您的托管域安全 LDAP 访问。

3. **使用者名称**的证书使用者名称必须是托管域通配符。 例如，如果您的域的名称为 contoso100.com，必须是证书的使用者名称 *。 contoso100.com。 设置为此通配符名称的 DNS 名称 （接受方备用名称）。

3. **密钥用法**的证书必须配置以下使用的数字签名和密钥加密。

4. **证书目的**的证书必须是有效的 SSL 服务器身份验证。

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>任务 1-获取安全 LDAP 证书
第一项任务涉及到获取证书用于对托管域安全 LDAP 访问。 有两种选择︰

- 从证书颁发机构获取证书。 颁发机构可能是您的组织的企业 CA 或公共证书颁发机构。

- 创建自签名的证书。


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>选项 （推荐）-从证书颁发机构获取安全 LDAP 证书
如果您的组织部署企业公钥基础结构 (PKI)，您需要从您的组织的企业证书颁发机构 (CA) 获取证书。 如果您的组织从公共证书颁发机构获得的证书，您需要从该公用证书颁发机构获取安全 LDAP 证书。

当请求证书，确保遵循[安全 LDAP 证书要求](#requirements-for-the-secure-ldap-certificate)中列出的需求。

> [AZURE.NOTE] 需要到托管域使用安全 LDAP 连接的客户端计算机必须信任 LDAPS 证书的颁发者。


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>选项 B-为安全 LDAP 创建自行签署式证书
您可以选择创建自行签署式证书安全 LDAP，如果︰

- 您的组织中的证书由不颁发企业证书颁发机构或
- 您不希望使用从公共证书颁发机构的证书。

**创建自行签署式证书使用 PowerShell**

在 Windows 计算机上，打开一个新的 PowerShell 窗口以**管理员的身份**并键入以下命令，以创建新的自签名的证书。

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

在前面的示例中，替换为 contoso100.com 使用的 DNS 域名的 Azure AD 域服务托管域。

![选择 Azure 的广告目录](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

新创建的自签名的证书放在本地计算机的证书存储区中。


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>任务 2-导出安全 LDAP 证书到。PFX 文件
在开始此任务之前，请确保您已从企业证书颁发机构或公共证书颁发机构获得安全 LDAP 证书，或已创建的自签名的证书。

执行以下步骤，以 LDAPS 将证书导出到。PFX 文件。

1. 按下**开始**按钮，然后键入**R**。在**运行**对话框中，键入**mmc** ，然后单击**确定**。

    ![启动 MMC 控制台](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. 在**用户帐户控制**提示下，单击**是**启动 MMC （Microsoft 管理控制台） 作为管理员。

3. 从**文件**菜单上，单击**添加/删除管理单元-...**。

    ![管理单元添加到 MMC 控制台](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. 在**添加或删除管理单元**对话框中，选择**证书**管理单元中，然后单击**添加 >**按钮。

    ![证书管理单元中添加到 MMC 控制台](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. 在**证书管理单元中**向导中，选择**计算机帐户**，然后单击**下一步**。

    ![为计算机帐户添加证书管理单元](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. 在**选择计算机**页面中，选择**本地计算机: （运行此控制台的计算机）** ，然后单击**完成**。

    ![添加证书管理单元的选择计算机](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. 在**添加或删除管理单元**对话框中，单击**确定**以向 MMC 添加证书管理单元。

    ![证书管理单元中添加到 MMC-完成](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. 在 MMC 窗口中，单击以展开**控制台根节点**。 您应该看到证书管理单元中加载。 单击**证书 （本地计算机）**展开。 单击以展开**个人**节点，跟**证书**节点。

    ![打开个人证书存储区](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. 您应该看到我们所创建的自签名的证书。 您可以检查以确保指纹匹配报告在 PowerShell windows 创建证书时，证书的属性。

10. 自签名的证书并**单击鼠标右键**选择。 从右击菜单中，选择**所有任务**，然后都选择**导出...**。

    ![导出证书](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. 在**证书导出向导**中，单击**下一步**。

    ![导出证书向导](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. 在**导出私钥**页中，选择**是，导出私钥**，并单击**下一步**。

    ![导出证书的专用密钥](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] 您必须导出证书和私钥。 如果提供不包含私钥的证书 PFX，启用托管域安全 LDAP 无法正常工作。

13. 在**导出文件格式**页中，选择**个人信息交换的 PKCS #12 (。PFX)**为导出的证书的文件格式。

    ![导出证书文件格式](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] 只。PFX 文件格式都支持。 不导出的证书。CER 文件格式。

14. 在**安全**页中，选择**密码**选项并键入密码以保护。PFX 文件。 因为在下一步的任务将需要它，请记住此密码。 单击**下一步**继续。

    ![证书导出密码 ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] 记下此密码。 需要时启用安全 LDAP[任务 3-启用托管域安全 LDAP](#task-3---enable-secure-ldap-for-the-managed-domain)中此管理域

15. 在**要导出的文件**页上指定的文件的名称和您想要导出的证书的位置。

    ![证书导出路径](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. 在以下页面上，单击**完成**将证书导出到 PFX 文件。 在导出证书时，您会看到确认对话框。

    ![完成导出证书](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>任务 3-启用安全的 LDAP 管理域
若要启用安全 LDAP，请执行以下配置步骤︰

1. 导航到**[Azure 的传统门户网站](https://manage.windowsazure.com)**。

2. 选择左边窗格中的**活动目录**节点。

3. 选择已经启用了 Azure AD 域服务的 Azure 的广告目录 （也称为租户）。

    ![选择 Azure 的广告目录](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. 单击**配置**选项卡。

    ![配置目录选项的卡](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. 向下滚动到标题**域服务**部分。 您应该看到一个标题为**安全 LDAP (LDAPS)** ，如下面的屏幕快照中所示的选项︰

    ![域服务配置节](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. 单击**配置证书...**按钮以显示**配置安全 LDAP 证书**对话框。

    ![为安全的 LDAP 配置证书](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. 单击文件夹图标下方**使用证书 PFX 文件**来指定该 PFX 文件，其中包含您想要使用的托管域安全 LDAP 访问该证书。 指定在证书导出到 PFX 文件时的密码输入。 然后，单击完成按钮的底部。

    ![指定安全 LDAP PFX 文件和密码](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. **配置**选项卡的**域服务**部分应获取灰色，在**挂起...**状态几分钟。 在此期间，LDAPS 证书验证的准确性和安全 LDAP 配置为您的托管域。

    ![安全 LDAP 的挂起状态](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] 大约 10 到 15 分钟，需启用托管域安全 LDAP。 如果提供的安全 LDAP 证书不符合要求的标准，未启用安全的 LDAP 目录，请参阅故障。 例如，域名不正确，该证书已过期或等即将到期。

9. 启用安全 LDAP 后成功地为您的托管域，**挂起...**消息将会消失。 您应该会看到显示的证书的指纹。

    ![启用安全 LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>任务 4-通过互联网启用安全 LDAP 访问
**可选的任务**-如果您不打算访问 internet 上使用 LDAPS 托管的域，请跳过此配置任务。

在开始此任务之前，请确保您已完成[任务 3](#task-3---enable-secure-ldap-for-the-managed-domain)中列出的步骤。

1. 您应该看到**启用安全 LDAP 访问通过 INTERNET** **配置**页中的**域服务**部分中的一个选项。 由于对托管域安全 LDAP 通过互联网访问禁用默认情况下，此选项默认设置为**否**中。

    ![启用安全 LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. 切换为**是****启用 INTERNET 上的安全 LDAP 访问**。 单击底部面板上的**保存**按钮。
    ![启用安全 LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. **配置**选项卡的**域服务**部分应获取灰色，在**挂起...**状态几分钟。 一些时间后，对托管域安全 LDAP 通过互联网访问已启用。

    ![安全 LDAP 的挂起状态](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] 大约 10 分钟需启用托管域安全 LDAP 通过互联网访问。

4. 已成功启用安全 LDAP 访问 internet 的托管域时，**挂起...**消息将会消失。 您应该看到可以用于通过 LDAPS 字段**的外部 IP 地址为 LDAPS 访问**中访问您的目录的外部 IP 地址。

    ![启用安全 LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>任务 5-配置 DNS 以从 internet 访问托管的域
**可选的任务**-如果您不打算访问 internet 上使用 LDAPS 托管的域，请跳过此配置任务。

在开始此任务之前，请确保您已完成[任务 4](#task-4---enable-secure-ldap-access-over-the-internet)中列出的步骤。

一旦启用了安全 LDAP 访问 internet 的托管域，您需要更新，以便客户端计算机可以找到此管理的域的 DNS。 任务 4 结尾，**外部 IP 地址为 LDAPS 访问**中的**配置**选项卡上显示外部 IP 地址。

配置外部 DNS 提供商联系，以便管理域 (例如，contoso100.com) 的 DNS 名称指向这个外部的 IP 地址。 在我们的示例中，我们需要创建以下 DNS 条目︰

    contoso100.com  -> 52.165.38.113

就是这样-您现在可以连接到在 internet 上使用安全 LDAP 的托管域。

> [AZURE.WARNING] 请记住，客户端计算机必须信任能够成功连接到托管域使用 LDAPS LDAPS 证书的颁发者。 如果您使用的企业证书颁发机构或公开受信任的证书颁发机构，您不需要执行任何操作，因为客户端计算机信任这些证书的颁发者。 如果您正在使用自签名的证书，您需要安装到客户端计算机上的受信任的证书存储的自签名证书的公钥部分。

<br>

## <a name="related-content"></a>相关的内容

- [管理 Azure AD 域服务管理的域](active-directory-ds-admin-guide-administer-domain.md)
