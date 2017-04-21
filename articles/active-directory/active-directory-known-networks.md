<properties 
    pageTitle="已知的网络 |Microsoft Azure" 
    description="通过将已知的网络配置，可避免必须拥有由您的组织在多个地域的号单元和从利用可疑活动报告 IP 地址号单元中包含的 IP 地址。" 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>已知的网络


您可以使用 Azure Active Directory 访问和使用情况报告深入了解的完整性和组织的目录的安全性。 使用此信息，目录管理员可以更好地确定可能的安全风险，以便他们可以充分地规划以减轻这些风险可能所在。

它是可能的"*在多个地域的号单元*"和"*号单元从 IP 地址与可疑的活动*"的报告错误地标记实际上归您组织的 IP 地址。 

可以例如，发生此错误时︰ 

- 在您办公室有登录远程数据中心在旧金山的波士顿在用户触发"签名在多个地域单元"报表 

- 您的组织的用户试图登录几次与密码不正确的触发器标记单元从 IP 地址与可疑活动报告 

若要防止这种情况下生成误导性安全报告，应到您所在组织的公用 IP 地址的列表添加已知的 IP 地址范围。    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>若要添加您的组织的公用 IP 地址范围，请执行以下步骤︰ 

1.  登录到[Azure 的管理门户](https://manage.windowsazure.com)。

2.  在左窗格中，单击**撤销**。 <br><br>![云应用程序发现的工作原理](./media/active-directory-known-networks/known-netwoks-01.png)

3.  在**目录**选项卡，选择目录。

4.  在菜单上，单击**配置**。 <br><br>![云应用程序发现的工作原理](./media/active-directory-known-networks/known-netwoks-02.png)

5.  在配置选项卡上，转到**您的组织的公用 IP 地址范围** <br><br>![云应用程序发现的工作原理](./media/active-directory-known-networks/known-netwoks-03.png)

6.  单击**添加已知的 IP 地址范围**。

7.  在出现的对话框中添加地址范围，然后在完成后单击检查按钮。 <br><br>![云应用程序发现的工作原理](./media/active-directory-known-networks/known-netwoks-04.png)


**其他资源**


* [查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)
* [从 IP 地址与可疑活动签名宏](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [在多个地域的签名宏](active-directory-reporting-sign-ins-from-multiple-geographies.md)


