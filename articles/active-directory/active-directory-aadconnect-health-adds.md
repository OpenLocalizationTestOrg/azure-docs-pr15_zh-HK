
<properties
    pageTitle="使用 Azure AD 连接运行状况与 AD DS |Microsoft Azure"
    description="这是将讨论如何监视 AD DS Azure AD 连接健康页。"
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>与 AD DS 使用 Azure AD 连接运行状况
以下文档是特定于 Active Directory 域服务使用 Azure AD 连接运行状况监视。 AD DS 的受支持的版本是︰ Windows Server 2008 R2、 Windows Server 2012，以及 Windows Server 2012 R2。

监视使用 Azure AD 连接健康的 AD FS 的详细信息，请参阅[使用 Azure AD 连接健康 AD FS](active-directory-aadconnect-health-adfs.md)。 此外，有关监视使用 Azure AD 连接健康的 Azure AD 连接 （同步） 的信息请参阅[使用 Azure AD 同步连接状况](active-directory-aadconnect-health-sync.md)。

![Azure AD 连接为 AD DS 的健康](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>警报的 Azure 的广告为 AD DS 连接运行状况
在 Azure AD 连接健康为 AD DS，通知部分提供的活动和已解决的警报，与相关的域控制器的列表。 选择一个活动或已解决的警报的其他信息，以及解决方法步骤，打开新的刀片和支持文档的链接。 每个警报类型可以具有一个或多个实例，分别对应于每个域控制器受该特定警报。 靠近底部的警报刀片式服务器，您可以双击一个受影响的域控制器以打开该警报实例有关的更多详细信息附加的刀片式服务器。

在此刀片，可以启用电子邮件警报通知和视图中更改的时间范围。 时间范围扩展允许您查看以前已解决的警报。

![Azure AD 连接同步错误](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>域控制器控制板
此仪表板提供了您的环境，以及主要运营指标和每个受监视的域控制器的运行状况的拓扑视图。 提供的度量有助于快速识别可能需要进一步调查的任何域控制器。 默认情况下，将显示的列的子集。 但是，您可以通过双击列命令找到可用的列的整组。 选择的列，您最关心的事情，打开到一个简便此仪表板将以查看您的 AD DS 环境的运行状况。

![域控制器](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

域控制器将按其各自域或站点，对于了解环境拓扑很有帮助。 最后，如果您双击刀片头，控制板最大化利用可用的屏幕房地产。 显示多个列时，此大视图非常有用。

## <a name="replication-status-dashboard"></a>复制状态仪表板
此仪表板提供了复制状态的视图和受监视的域控制器的复制拓扑。 列出最新的复制尝试的状态，以及找到的任何错误的帮助文档。 您可以双击错误，若要打开新刀片的信息，如有的域控制器︰ 有关错误，建议解决方法步骤和链接为文档的疑难解答的详细信息。

![复制状态](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>监视
此功能提供不同的性能计数器，从每个受监视的域控制器连续收集的图形化趋势。 域控制器的性能很容易可以在林中的所有其他受监视的域控制器之间进行比较。 此外，您可以看到不同的性能计数器，并排在您环境中的问题进行故障排除时，这是很有帮助。

![监视](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

默认情况下，我们已事先选择四个性能计数器;但是，可以通过单击筛选命令并选择或取消选择任何所需的性能计数器包括其他人。 此外，您还可以双击性能计数器图形，以打开新的刀片，其中包括为每个受监视的域控制器的数据点。

## <a name="related-links"></a>相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接健康代理安装](active-directory-aadconnect-health-agent-install.md)
* [Azure AD 连接健康操作](active-directory-aadconnect-health-operations.md)
* [与 AD FS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adfs.md)
* [对于同步使用 Azure AD 连接运行状况](active-directory-aadconnect-health-sync.md)
* [Azure AD 连接健康常见问题解答](active-directory-aadconnect-health-faq.md)
* [Azure AD 连接健康版本历史记录](active-directory-aadconnect-health-version-history.md)
