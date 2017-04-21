 （备份存储库<properties
   页面标题 ="Azure Backup limits table"
   描述 ="描述的系统限制 Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


以下限制适用于 Azure 备份。

| 限制标识符 | 默认限制 |
|---|---|
|可以针对每个存储库注册/服务器计算机的数目|对于 Windows 服务器/客户端/SCDPM 50 <br/> IaaS Vm 的 200|
|在 Azure 存储库存储中存储的数据的数据源的大小|54400 GB 的最大值<sup>1</sup>|
|备份可以在 Azure 的每个订阅中创建的电子仓库的数量|25 （备份电子仓库） <br/> 每个地区保险存储恢复服务 25|
|可以每日计划备份的次数|对于 Windows 服务器/客户端每天三次 <br/> 每个工作日 SCDPM 2 <br/> IaaS 虚拟机一天一次|
|附加到 Azure 的虚拟机的备份数据磁盘|16|

- <sup>1</sup>54400 GB 限制不适用于 IaaS VM 备份。

