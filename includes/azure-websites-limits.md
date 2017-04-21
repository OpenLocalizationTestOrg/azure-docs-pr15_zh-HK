资源|免费|共享 （预览）|基本|标准|高级 （预览）</th>
---|---|---|---|---|---
[Web、 移动设备、 或 API 应用程序](https://azure.microsoft.com/services/app-service/)每个[应用程序服务计划](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|不限的<sup>2</sup>|不限的<sup>2</sup>|不限的<sup>2</sup>
每个[应用程序服务计划](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)的[逻辑应用程序](https://azure.microsoft.com/services/app-service/logic/)</a><sup>1</sup>|10|10|10|每个内核的 20|每个内核的 20
[应用程序服务计划](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|每个地区的 1|每个资源组 10|每个资源组 100|每个资源组 100|每个资源组 100
计算实例类型|共享|共享|专用的<sup>3</sup>|专用的<sup>3</sup>|专用的<sup>3</sup></p>
[向外扩展](../articles/app-service-web/web-sites-scale.md)（最大值的实例）|1 共享|1 共享|3 专用的<sup>3</sup>|10 个专用的<sup>3</sup>|20 专用 (50 ASE)<sup>3、 4</sup>
存储<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU 时间 （短）<sup>6</sup>|3 分钟|3 分钟|不受限制，按标准[费率](https://azure.microsoft.com/pricing/details/app-service/)的付薪</a>|不受限制，以标准的比率的付薪|不受限制，以标准的比率的付薪
CPU 时间 （天）<sup>6</sup>|60 分钟|240 分钟|不受限制，按标准[费率](https://azure.microsoft.com/pricing/details/app-service/)的付薪</a>|不受限制，以标准的比率的付薪|不受限制，以标准的比率的付薪
内存 （1 小时）|每个应用程序服务计划 1024 MB|每个应用程序的 1024 MB|N/A|N/A|N/A
带宽|165 MB|不受限制，[数据传输速率](https://azure.microsoft.com/pricing/details/data-transfers/)应用|不受限制，数据传输速率应用|不受限制，数据传输速率应用|不受限制，数据传输速率应用
应用程序体系结构|32 位|32 位|32 位 64 位|32 位 64 位|32 位 64 位
每个实例<sup>7</sup> web 套接字|5|35|350|无限制|无限制
每个应用程序并发[调试程序连接](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)|1|1|1|5|5
[azurewebsites.net 与 FTP/S 和 SSL 的子域](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
[自定义的域](../articles/app-service-web/web-sites-custom-domain-name.md)支持||X|X|X|X
[SSL 支持](../articles/app-service-web/web-sites-configure-ssl-certificate.md)自定义的域|||无限制|无限制、 5 SNI SSL 和 1 包括的 IP SSL 连接|无限制、 5 SNI SSL 和 1 包括的 IP SSL 连接
集成的负载平衡器||X|X|X|X
[始终打开](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[定时的备份](../articles/app-service-web/web-sites-backup.md)||||每天一次|每隔 5 分钟<sup>8</sup>次
[自动缩放](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md)<sup>9</sup>|X|X|X|X|X
[Azure 计划](https://azure.microsoft.com/services/scheduler/)支持||X|X|X|X
[终结点监视](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[暂存槽 （预览）](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
自定义每个应用程序域</a>||500|500|500|500
SLA||<p>|99.9%|99.95<sup>10</sup>|99.95<sup>10</sup>

<sup>1</sup>应用程序和存储配额是每个应用程序服务计划，除非另外说明。  
<sup>2</sup>可以在这些计算机承载的应用程序的实际数目取决于活动的应用程序、 计算机实例中和相应的资源利用率的大小。  
<sup>3</sup>专用的实例可以是不同的大小。 有关详细信息，请参阅[应用程序服务的定价](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/)。  
<sup>4</sup>特优层最多允许 50 计算实例 （如有提供） 和 500 GB 的磁盘空间时使用的应用程序服务的环境中，和 20 否则计算实例和 250 GB 存储。  
<sup>5</sup>存储限制是总的内容大小跨所有应用程序在同一应用程序服务计划。 在[应用程序服务环境](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)中有更多的存储选项  
<sup>6</sup>这些资源都受物理资源上的专用实例 （实例的大小和数量的实例）。  
<sup>7</sup>如果两个实例的基本层中缩放应用程序时，您有 350 并发连接的两个实例的每个。  
<sup>8</sup>使用应用程序服务的环境中，和 50 次每日否则为特优层使备份间隔向下的，最多每隔 5 分钟。  
<sup>9</sup>根据需要，按计划，还是持续作为后台任务的应用程序服务实例中运行自定义可执行文件和/或脚本。 始终是连续的 WebJobs 执行必需的。 Azure 计划程序可用或标准时需要计划 WebJobs。 可以在应用程序服务实例，运行的 WebJobs 号上没有预定义的限制，但有取决于应用程序代码正在尝试执行的实际限制。   
<sup>10</sup>SLA 为 99.95%使用使用 Azure 流量管理器中的多个实例的部署提供故障切换配置。  
