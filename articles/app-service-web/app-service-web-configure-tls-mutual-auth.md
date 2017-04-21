<properties 
    pageTitle="如何配置 TLS 相互验证的 Web 应用程序" 
    description="了解如何配置您的 web 应用程序要使用 TLS 客户端证书身份验证。" 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>如何配置 TLS 相互验证的 Web 应用程序

## <a name="overview"></a>概述 ##
您可以通过支持不同类型的身份验证为其于 Azure 的 web 应用程序限制访问。 若要执行此操作的一种方法是通过 TLS/SSL 请求时使用客户端证书进行身份验证。 这种机制称为 TLS 相互身份验证或身份验证和这篇文章将详细介绍如何设置您的 web 应用程序使用客户端证书身份验证的客户端证书。

> **注意︰**如果您通过 HTTP 和不 HTTPS 访问您的站点，您不会收到任何客户端证书。 因此如果您的应用程序需要客户端证书不应允许请求向您的应用程序通过 HTTP。


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>为客户端证书身份验证配置 Web 应用程序 ##
若要设置您的 web 应用程序需要客户端证书，您需要添加的 clientCertEnabled 网站设置为您的 web 应用程序并将其设置为 true。 此设置不是目前通过在门户中，管理经验和 REST API 将需要用来实现此目的。

您可以使用[ARMClient 工具](https://github.com/projectkudu/ARMClient)轻松地创建定制的 REST API 调用。 登录该工具之后，您需要发出以下命令︰

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
{} 中的所有内容替换为您的 web 应用程序的信息，并创建一个文件调用 enableclientcert.json 以下的 JSON 与内容︰

> {"位置":"我 Web 应用程序的位置"，   
>   "属性": {  
>     "clientCertEnabled": 真}}  

请务必将"位置"值更改为您的 web 应用程序所在的地方如美国中北部或西部美国等。

> **注意︰**如果从 Powershell 运行 ARMClient 时，则需要进行转义@反勾号的 JSON 文件的符号。

## <a name="accessing-the-client-certificate-from-your-web-app"></a>从您的 Web 应用程序中访问该客户端证书 ##
如果使用的 ASP.NET 配置您的应用程序使用客户端证书身份验证，则证书将可通过**HttpRequest.ClientCertificate**属性。 对于其他应用程序的堆栈，将"X-ARR-ClientCert"的请求标头中的 base64 编码值通过您的应用程序中的可用客户端证书。 您的应用程序可以从此值创建一个证书，然后将其用于身份验证和授权应用程序中。

## <a name="special-considerations-for-certificate-validation"></a>证书验证的特殊注意事项 ##
发送到应用程序的客户端证书不会任何验证通过 Azure Web 应用程序平台。 验证此证书是使 web 应用程序的责任。 下面是示例 ASP.NET 代码，以验证身份验证目的的证书属性。

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
