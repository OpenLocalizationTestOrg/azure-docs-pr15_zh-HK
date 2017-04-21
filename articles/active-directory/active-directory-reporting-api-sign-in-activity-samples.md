<properties
    pageTitle="Azure Active Directory 签到活动报告 API 示例 |Microsoft Azure"
    description="如何开始使用 Azure 活动目录报告 API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory 签到活动报告 API 示例

本主题是有关报告 API Azure Active Directory 的主题集合的一部分。  
Azure AD 报告为您提供一个 API，使您可以访问使用代码或相关的工具的登录活动数据。  
本主题的范围是**登录活动 API**为您提供代码示例。

请参见︰

- [审核日志](active-directory-reporting-azure-portal.md#audit-logs)有关的概念信息
- [开始使用 Azure 活动目录报告 API](active-directory-reporting-api-getting-started.md)报告的 API 的详细信息。

疑问、 问题或反馈，请与[帮助 AAD 报告](mailto:aadreportinghelp@microsoft.com)。


## <a name="prerequisites"></a>系统必备组件
您可以使用本主题中的示例之前，需要完成[访问报告 API Azure AD 的先决条件](active-directory-reporting-api-prerequisites.md)。  


##<a name="powershell-script"></a>PowerShell 脚本

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>执行脚本
一旦您完成编辑该脚本、 运行和验证预期的数据从审核日志报告，则返回。

该脚本从登录报告以 JSON 格式返回输出。 它还将创建`SigninActivities.json`具有相同的输出文件。 您可以通过修改该脚本以从其他报告和注释掉不需要的输出格式返回数据进行试验。



## <a name="next-steps"></a>下一步行动

- 若要自定义此主题中的示例吗？ 签出[签入活动 Azure Active Directory API 参考](active-directory-reporting-api-sign-in-activity-reference.md)。 

- 如果您想要查看使用 Azure Active Directory 报告 API 的完整概述，请参阅[开始使用 Azure Active Directory 报告 API](active-directory-reporting-api-getting-started.md)。

- 如果您想要找出有关 Azure Active Directory 报告的详细信息，请参阅[Azure 活动目录报告指南](active-directory-reporting-guide.md)。  