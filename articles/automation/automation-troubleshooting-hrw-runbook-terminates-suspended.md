<properties
   pageTitle="混合的 Runbook 工作人员︰ runbook 作业终止状态为挂起 |Microsoft Azure"
   description="症状的原因和解决方法混合 Runbook 工作人员作业终止错误。"
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>混合的 Runbook 工作人员︰ runbook 作业终止状态为挂起

## <a name="summary"></a>摘要

尝试执行它三次后不久，即被挂起您的 runbook。 这可能会中断成功完成 runbook 的情况和相关的错误消息不包含任何表明原因的其他信息。 本文对混合 Runbook 工作人员 runbook 执行故障有关的问题提供故障排除步骤。

如果本文没有解决您 Azure 问题，请访问 Azure 论坛[MSDN 和堆栈溢出](https://azure.microsoft.com/support/forums/)。 可以发布您的问题，在这些论坛上或[@AzureSupport在 Twitter 上](https://twitter.com/AzureSupport)。 另外，可以通过选择[Azure 支持](https://azure.microsoft.com/support/options/)网站上**获取支持**归档 Azure 支持请求。

## <a name="symptom"></a>故障现象

Runbook 执行失败并返回的错误是，"不能运行激活，因为该进程意外终止的作业操作。 作业操作尝试了 3 次。"


## <a name="cause"></a>原因

有几个可能的错误原因︰ 

  1. 混合工作是代理服务器或防火墙后面
  2. 混合工作人员正在其运行的计算机具有少于最低硬件[要求](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. 运行手册无法进行身份验证的本地资源


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>原因 1︰ 混合 Runbook 工作人员是在代理服务器或防火墙后面

混合的 Runbook 工作人员正在其运行的计算机位于防火墙或代理服务器后面和出站网络访问权限可能不允许使用或配置正确。

### <a name="solution"></a>解决方案

确保计算机具有出站访问权限 *。 在端口 443、 9354，和 30000 30199 cloudapp.net。 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>原因 2︰ 计算机有少于最低硬件要求

计算机运行混合 Runbook 工作人员应指定其承载此功能之前满足最低硬件要求。 否则为根据其他后台进程和执行期间由运行手册的争用的资源利用率，计算机将会变得多利用并导致 runbook 作业延迟或超时。 

### <a name="solution"></a>解决方案 

首先确认指定运行混合 Runbook 辅助功能的计算机满足最低硬件要求。  如果是这样，监视 CPU 和内存利用率，以确定性能的混合 Runbook 工作进程和窗口之间的任何关联。  如果没有内存或 CPU 压力，这可能表明需要升级或添加更多的处理器或增加内存来解决资源瓶颈和解决错误。 或者，选取不同的计算资源，可以在工作负荷需求指明有必要增加支持的最低要求和规模。         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>原因 3︰ 运行手册无法通过身份验证的本地资源

### <a name="solution"></a>解决方案

检查相应的事件与*Win32 进程已退出，代码为 [4294967295]*的说明**Microsoft SMA**事件日志。  此错误的原因是没有在运行手册中配置身份验证或指定混合工作人员组的运行方式凭据。  请查看[Runbook 权限](automation-hybrid-runbook-worker.md#runbook-permissions)确认您已正确配置身份验证的运行手册。  


 

