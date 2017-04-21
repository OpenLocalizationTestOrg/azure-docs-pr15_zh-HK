<properties 
    pageTitle="如何保护后端服务使用客户端证书身份验证在 Azure API 管理" 
    description="了解如何确保后端服务在 Azure API 管理中使用客户端证书身份验证的安全。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>如何保护后端服务使用客户端证书身份验证在 Azure API 管理

API 管理让您能够安全地访问到后端服务 API 的使用客户端证书。 本指南介绍了如何管理证书在 API 发布门户中，以及如何配置使用证书来访问它的后端服务的 API。

有关管理证书使用 API 管理 REST API 的信息，请参阅[Azure API 管理 REST API 证书实体][]。

## <a name="prerequisites"></a>系统必备组件

本指南介绍了如何配置您的 API 管理服务实例，以使用客户端证书身份验证来访问后端服务 API。 本主题中的步骤之前，应该有您的后端服务配置为客户端证书身份验证 （[要配置在 Azure 网站证书身份验证，请参阅这篇文章][]），并有权证书和密码 API 管理发布门户中上传的证书。

## <a name="step1"></a>上载的客户端证书

若要开始，请单击 Azure 经典门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![API 发布门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

从**API 管理**菜单的左侧，单击**安全**，然后单击**客户端证书**。

![客户端证书][api-management-security-client-certificates]

若要上载一个新证书，请单击**上载证书**。

![将上载证书][api-management-upload-certificate]

浏览到您的证书，然后输入该证书的密码。

>证书必须是以**.pfx**格式。 允许使用自签名的证书。

![将上载证书][api-management-upload-certificate-form]

请单击**上载**上载证书。

>这一次验证的证书密码。 如果它不正确显示一条错误消息。

![上载的证书][api-management-certificate-uploaded]

一旦上载证书，它将出现在**客户端证书**选项卡上。 如果您有多个证书，请记下的该主题或指纹，后四个字符，用来根据下[一个 API，用于使用客户端证书用于身份验证网关配置][]节中的介绍配置要使用的证书，API 时选择的证书。

>若要关闭证书链验证时使用，例如，一个自签名的证书，按照此常见问题解答[项](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)所述的步骤。

## <a name="step1a"></a>中删除客户端证书

若要删除证书，请单击所需的证书旁边的**删除**。

![删除证书][api-management-certificate-delete]

单击**是，删除它**以确认。

![确认删除][api-management-confirm-delete]

如果证书是由 API 的使用中，将显示警告屏幕。 要删除的证书必须首先从配置为使用它的任何 Api 删除证书。

![确认删除][api-management-confirm-delete-policy]

## <a name="step2"> </a>API 配置为使用客户端证书用于身份验证网关

单击左侧**API 管理**菜单上的**Api**单击所需的 API 中，名称，单击**安全**选项卡。

![API 的安全][api-management-api-security]

从**凭据**下拉列表中选择**客户端证书**。

![客户端证书][api-management-mutual-certificates]

从**客户端证书**下拉列表中选择所需的证书。 如果有多个证书，您可以查看的主题或指纹上一节中所述的最后四个字符来确定正确的证书。

![选择证书][api-management-select-certificate]

单击**保存**以将配置更改保存到该 API。

>此更改会立即生效，并对该 API 的操作的调用将使用证书进行身份验证后端服务器上。

![保存 API 更改][api-management-save-api]

>指定证书后的 API 网关的后端服务的身份验证，它成为策略的一部分，该 API，并可以在策略编辑器中查看。

![证书策略][api-management-certificate-policy]

## <a name="next-steps"></a>下一步行动

有关详细信息的其他方法来保护您的后端服务，如 HTTP 基本或共享密钥身份验证，请参见下面的视频。

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API 管理入门]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[Azure 的 API 管理 REST API 证书实体]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[要配置在 Azure 网站证书身份验证，请参阅这篇文章]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[API 客户端证书用于身份验证网关配置]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
