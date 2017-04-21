<properties 
    pageTitle="将 SSL 配置为云服务 （经典） |Microsoft Azure" 
    description="了解如何指定 HTTPS 终结点的 web 角色以及如何上载一个 SSL 证书，以便保护您的应用程序。" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>在 Azure 应用程序配置 SSL

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-configure-ssl-certificate-portal.md)
- [Azure 的传统门户网站](cloud-services-configure-ssl-certificate.md)

安全套接字层 (SSL) 加密是保护通过 internet 发送的数据的最常用的方法。 此项常规任务讨论如何指定 HTTPS 终结点的 web 角色以及如何上载一个 SSL 证书，以便保护您的应用程序。

> [AZURE.NOTE] 在此任务中的过程适用于 Azure 云服务;应用程序服务，请参阅[这](../app-service-web/web-sites-configure-ssl-certificate.md)篇文章。

此任务使用生产部署。 在本主题的末尾提供了有关使用临时部署信息。

如果您还没有，请先阅读[这](cloud-services-how-to-create-deploy.md)篇文章还创建了云服务。

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]


## <a name="step-1-get-an-ssl-certificate"></a>步骤 1︰ 获取 SSL 证书

要将 SSL 配置为应用程序，首先需要获取 SSL 证书的已签名的证书颁发机构 (CA)，可信第三方为此目的发出的证书。 如果您没有一个，您需要获取一个从公司中销售的 SSL 证书。

证书必须符合 Azure 中的 SSL 证书的以下要求︰

-   证书必须包含私钥。
-   个人信息交换 (.pfx) 文件导出的密钥交换，必须创建证书。
-   证书的接受方名称必须与用来访问云服务的域相匹配。 您不能从 cloudapp.net 域的证书颁发机构 (CA) 获取 SSL 证书。 您必须获得时，要使用一个自定义域名访问您的服务。 当从 CA 申请证书时，证书的接受方名称必须与用来访问您的应用程序的自定义域名匹配。 例如，如果您的自定义域名**contoso.com**您将从 CA 请求证书您为***。 contoso.com**或* *www.contoso.com**。
-   证书必须使用至少为 2048年位加密。

出于测试目的，您可以[创建](cloud-services-certs-create.md)并使用自我签署的证书。 自行签署式证书 CA 通过未经身份验证，可以使用 cloudapp.net 域作为网站的 URL。 例如，下面的任务将使用自签名的证书的证书中所使用的公用名 (CN) 是**sslexample.cloudapp.net**。

接下来，必须在服务定义和服务配置文件中包括有关证书的信息。

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>步骤 2︰ 修改服务定义和配置文件

必须将应用程序配置为使用该证书，并必须添加 HTTPS 终结点。 因此，服务定义和服务配置文件需要更新。

1.  在开发环境中，打开服务定义文件 (CSDEF)、 添加**证书**一节中的**WebRole**部分中，并包括有关的证书和中间证书） 的以下信息︰

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    **证书**部分定义我们证书、 其位置和它所在的存储的名称的名称。
    
    权限 (`permisionLevel`属性) 可以设置为以下值之一︰

  	| 权限值  | 说明 |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **（默认值）**所有角色进程可以都访问的专用密钥。 |
  	| 提升          | 已提升的进程可以访问的专用密钥。|

2.  在服务定义文件中，添加要启用 HTTPS 的**终结点**节中的**InputEndpoint**元素︰

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  在服务定义文件中，添加**网站**部分中的**绑定**元素。 这一节中添加 HTTPS 绑定将该终结点映射到您的网站︰

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    已完成服务定义文件的所有必要的更改，但您仍需要将证书信息添加到服务配置文件。

4.  在服务配置文件 (CSCFG) ServiceConfiguration.Cloud.cscfg，添加替换您的证书与如下所示的示例指纹值的**角色**部分中的一个**证书**部分︰

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

（前面的示例中为微缩图算法使用**sha1** 。 指定适当的值对于您的证书指纹算法）。

既然服务定义和服务配置文件已更新，包您的部署将上载到 Azure。 如果您使用的**cspack**，不要使用**/generateConfigurationFile**标志，因为它会覆盖您插入的证书信息。

## <a name="step-3-upload-a-certificate"></a>步骤 3︰ 将上载证书

部署程序包已经更新，使用该证书，并得到了 HTTPS 终结点。 现在您可以上载程序包和证书到 Azure Azure 的传统门户网站。

1. 登录到[Azure 的传统门户网站][]。 
2. 单击左侧导航窗格中的**云服务**。
3. 单击所需的云服务。
4. 单击**证书**选项卡。

    ![单击证书选项卡](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. 请单击**上载**按钮。

    ![将上载](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. 提供该**文件****的密码**，然后单击**完成**（复选标记）。

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>步骤 4︰ 使用 HTTPS 连接角色实例

现在，您的部署已在 Azure 中启动并运行，您可以连接到使用 HTTPS。

1.  在 Azure 经典门户中，选择您的部署，然后在**网站 URL**下单击链接。

    ![确定站点 URL][2]

2.  在 web 浏览器中，修改为使用而不是**http**、 **https**链接，然后再访问该页面。

    >[AZURE.NOTE] 如果您使用自签名的证书，当您浏览到与自签名的证书的 HTTPS 终结点可能会看到在浏览器中的证书错误。 使用由受信任的证书颁发机构签名的证书可消除此问题;在此期间，您可以忽略该错误。 （另一种方法是将自签名的证书添加到用户的受信任的证书颁发机构证书存储区。

    ![SSL 示例 web 站点。][3]

如果您想要的而不是生产部署的临时部署使用 SSL，您首先需要确定用于临时部署的 URL。 部署到临时环境的云服务，而不包括证书或证书的任何信息。 在部署后，您可以确定 Azure 传统门户**网站 URL**字段中列出的 GUID 基于 URL。 请创建证书的公用名 (CN) 与等于基于 GUID 的 URL (例如， **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**)。 使用 Azure 的传统门户网站将证书添加到您的分阶段的云服务。 然后，将证书信息添加到您的 CSDEF 和 CSCFG 文件、 重新打包您的应用程序和更新分阶段的部署使用新的软件包。

## <a name="next-steps"></a>下一步行动

* [云服务的常规配置](cloud-services-how-to-configure.md)。
* 了解如何[部署云服务](cloud-services-how-to-create-deploy.md)。
* 配置[自定义的域的名称](cloud-services-custom-domain-name.md)。
* [管理您的云服务](cloud-services-how-to-manage.md)。


  [Azure 的传统门户网站]: http://manage.windowsazure.com
  [0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
  [1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
  [2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
  [3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
  [4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
