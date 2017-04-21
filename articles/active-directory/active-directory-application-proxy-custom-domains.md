<properties
    pageTitle="使用自定义 Azure AD 应用程序代理域 |Microsoft Azure"
    description="介绍如何使用 Azure AD 应用程序代理中自定义的域。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>使用自定义的域的 Azure AD 应用程序代理

使用默认域，可以将相同的 URL 设置为用于访问应用程序，以便您的用户只需一个 URL，要记住，若要访问该应用程序，无论他们所访问的内部和外部 URL。 这还允许您在访问面板中为应用程序创建一个快捷方式。 如果您使用 Azure AD 应用程序代理服务器提供的默认域，没有任何附加的配置需要启用您的域。 中，您使用自定义的域时，有几件事情需要做，以确保应用程序代理服务器能够识别您的域，并验证其证书。

## <a name="selecting-your-custom-domain"></a>选择您自定义的域

1. 在[发布应用程序与应用程序代理](active-directory-application-proxy-publish.md)发布应用程序根据的说明。
2. 应用程序出现在应用程序列表后，请选择它并单击**配置**。
3. 在**外部 URL**，请输入您自定义的域。
4. 如果您的外部 URL 是 https，将提示您要上载证书，所以该 Azure 可以验证应用程序的 URL。 您还可以将与该应用程序的外部 URL 匹配的通配符证书。 此域必须是您的[Azure 验证域](https://msdn.microsoft.com/library/azure/jj151788.aspx)列表中。 Azure 必须有证书的应用程序或通配符证书相匹配的应用程序的外部 URL 的域 URL。
5. 请确保要添加的 DNS 记录，将该内部 URL 路由到的应用程序使您可以拥有相同的 URL，以及内部和外部访问某个快捷方式为应用程序用户的应用程序列表中。

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>有关使用自定义的域的常见问题

问︰ 可以选择无需再次上载它已上载证书？  
答︰ 以前上载的证书会自动绑定到应用程序，并且没有一个证书匹配应用程序的主机名。  

问︰ 如何添加证书和哪种格式应导出的证书要上载中？  
答︰ 证书应该从应用程序配置页上载。 该证书应 PFX 文件。  

问︰ 可以使用 ECC 证书吗？  
答︰ 签名方法上的没有明确限制。  

问︰ 可以使用 SAN 证书吗？  
答︰ 是的。  

问︰ 可以使用通配符证书吗？  
答︰ 是的。  

问︰ 可以在每个应用程序使用不同的证书吗？  
答︰ 是的除非两个应用程序共享同一个外部主机。  

问︰ 如果我在注册一个新域，可以使用此域？  
答︰ 是的域的列表由租户的已验证的域列表。  

问︰ 如果证书过期怎么办？  
答︰ 您将在应用程序配置页中的证书部分得到警告。 当用户尝试访问该应用程序时，将弹出一条安全警告。  

问︰ 如果我想要替换为给定的应用程序证书怎么办？  
答︰ 上传一个新的证书从应用程序配置页。  

问︰ 可以删除证书并将其替换？  
答︰ 当您上载一个新证书，如果旧的证书未被另一个应用程序使用，它将被自动删除。  

问︰ 如果证书已被吊销怎么办？  
答︰ 吊销检查不会对证书执行。 当用户试图访问该应用程序，这取决于浏览器中，可能会出现安全警告。  

问︰ 可以使用自签名的证书？  
答︰ 是的允许使用自签名的证书。 请注意，是否您正在使用的私有证书颁发机构，证书的 CDP （证书吊销点分发点） 应公用。  

问︰ 有一个位置，以便查看我组织的所有证书吗？  
答︰ 这不是支持当前版本。  


## <a name="see-also"></a>请参见

- [发布应用程序与应用程序代理](active-directory-application-proxy-publish.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [启用条件访问](active-directory-application-proxy-conditional-access.md)
- [将您的自定义域名添加到 Azure 的广告](active-directory-add-domain.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
