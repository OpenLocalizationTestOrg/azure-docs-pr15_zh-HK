<properties
   pageTitle="连接器版本发布历史记录 |Microsoft Azure"
   description="本主题列出了最前沿的标识管理器 (FIM) 和 Microsoft 标识管理器 (MIM) 的连接器的所有版本"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>连接器版本版本历史记录
Forefront 标识管理器 (FIM) 和 Microsoft 标识管理器 (MIM) 的连接器会经常更新。

>[AZURE.NOTE]
本主题只是 FIM 和 MIM 上。 Azure AD 连接不支持这些连接器。

本主题列出了所有的连接器的已发布版本。

相关的链接︰

- [下载最新的连接器](http://go.microsoft.com/fwlink/?LinkId=717495)
- [一般的 LDAP 接口](active-directory-aadconnectsync-connector-genericldap.md)的参考文档
- [一般的 SQL 接口](active-directory-aadconnectsync-connector-genericsql.md)的参考文档
- [Web 服务连接器](http://go.microsoft.com/fwlink/?LinkID=226245)参考文档
- [PowerShell 连接器](active-directory-aadconnectsync-connector-powershell.md)参考文档
- [Lotus Domino 连接器](active-directory-aadconnectsync-connector-domino.md)参考文档

## <a name="111170"></a>1.1.117.0
发布︰ 2016 3 月

**新的连接器**  
[一般的 SQL 接口](active-directory-aadconnectsync-connector-genericsql.md)的初始版本。

**新功能︰**

- 一般的 LDAP 连接器︰
    - 增加了与 Isode 的增量导入支持。
- Web 服务连接器︰
    - CsEntryChangeResult 活动和 setImportErrorCode 活动以允许对象级别错误返回到同步引擎更新。
    - 更新为使用新的对象级别的错误功能的 SAP6 和 SAP6User 的模板。
- Lotus Domino 连接器︰
    - 导出时，您需要一个验证者每个通讯簿。 现在可以使用相同的密码的所有 certifiers 可以使管理更容易。

**已解决的问题︰**

- 一般的 LDAP 连接器︰
    - 为 IBM Tivoli DS，某些引用特性是没有正确检测到。
    - 为打开 LDAP 增量导入过程中，被截断的开头和结尾的字符串的空格。
    - 对于 Novell 和 NetIQ，Ou 中的容器之间以及在同一时间移动对象导出重命名对象失败。
- Web 服务连接器︰
    - 如果 web 服务具有相同的绑定多个终结点，然后连接器未正确地发现这些终结点。
- Lotus Domino 连接器︰
    - FullName 特性到邮件数据库导出未解决问题。
    - 导出的同时添加和从组中删除成员仅导出添加的成员。
    - 如果 Notes 文档中是无效的 (设置为 false 属性 isValid)，则连接器无法正常工作。

## <a name="older-releases"></a>较早版本
之前 3 月 2016年连接器作为支持主题发布。

**一般的 LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597，2015年 9 月
- [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549，2015 3 月
- [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534，2015 年 1 月
- [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419，2014年 9 月
- [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082，2014 3 月

**Web 服务**

- [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419，2014年 9 月

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419，2014年 9 月

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597，2015年 9 月
- [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549，2015 3 月
- [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712，2014年 8 月
- [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003，2014年 2 月  
- [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721，2013年 10 月
- [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534，2013年 8 月

## <a name="next-steps"></a>下一步行动
了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
