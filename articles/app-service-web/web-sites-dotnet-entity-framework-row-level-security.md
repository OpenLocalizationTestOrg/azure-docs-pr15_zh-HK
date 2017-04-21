<properties
    pageTitle="教程︰ 使用实体框架和行级别安全性的多租户数据库的 Web 应用程序"
    description="了解如何开发 ASP.NET MVC 5 web 应用程序与多租户 SQL 数据库 backent，使用实体框架和行级别安全性。"
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>教程︰ 使用实体框架和行级别安全性的多租户数据库的 Web 应用程序

本教程展示如何构建 web 的多租户应用程序与"[共享的数据库、 共享的模式](https://msdn.microsoft.com/library/aa479086.aspx)"组织模型，使用实体框架和[行级别安全性](https://msdn.microsoft.com/library/dn765131.aspx)。 在此模型中，一个数据库的多租户，包含数据和每个表中的每一行都是与"租户 id"。 行级安全性 (RLS)，一个新功能，Azure SQL 数据库中，用于防止租户访问彼此的数据。 这就要求只是单一的小更改应用程序。 通过集中数据库内的租户访问逻辑，RLS 简化应用程序代码，并减少租户之间的意外的数据泄漏的风险。

我们先简单的联系人管理器应用程序中[创建一个 ASP.NET MVP 应用程序使用身份验证和 SQL 数据库并将其部署到 Azure 应用程序服务](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)。 右侧的应用程序现在，允许所有用户 （租户） 若要查看所有联系人︰

![在启用 RLS 之前的联系人管理器应用程序](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

有几个很小的更改，我们将添加支持多组织，这样，用户就能够看到只属于自己的联系人。

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>步骤 1︰ 设置 SESSION_CONTEXT 的应用程序中添加一个侦听器类

没有我们需要进行一次应用程序更改。 因为应用程序的所有用户都连接到数据库使用相同的连接字符串 （即同一 SQL 登录），目前尚没有办法知道它应该筛选哪些用户 RLS 策略。 这种方法是 web 应用程序中非常常见，因为它使有效的连接池，但它意味着我们需要另一种方法来标识数据库中的当前应用程序用户。 解决方案是将该应用程序，它执行任何查询之前打开一个连接之后, 在[SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806)中的当前用户 Id 设置键 / 值对。 SESSION_CONTEXT 是会话范围键 / 值存储，和 RLS 的政策将使用存储在它的用户 Id 来标识当前用户。

我们将添加[侦听器](https://msdn.microsoft.com/data/dn469464.aspx)（尤其是在[DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)），一种新的功能在实体框架 (EF) 6，自动设置当前用户 Id SESSION_CONTEXT 中通过 EF 打开连接时执行 T SQL 语句。

1.  在 Visual Studio 中打开 ContactManager 项目。
2.  模型文件夹中的解决方案资源管理器中，右键单击并选择添加 > 类。
3.  命名为"SessionContextInterceptor.cs"的新类，然后单击添加。
4.  SessionContextInterceptor.cs 中的内容替换为以下代码。

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

这就是所需的唯一应用程序更改。 接下来，生成并发布应用程序。

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>步骤 2︰ 添加对数据库架构的用户 Id 列

接下来，我们需要将用户 Id 列添加到联系人表的每一行与用户 （租户）。 我们将改变直接在数据库中的架构，使我们不必在 EF 的数据模型中包括此字段。

连接到数据库直接使用 SQL Server 管理 Studio 或 Visual Studio，，然后执行以下的 T SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

将用户 Id 列添加到联系人表。 我们使用 nvarchar(128) 数据类型与 AspNetUsers 表中存储用户 Id 相匹配，我们创建一个默认约束，将自动设置为新插入的行的用户 Id，以将当前存储在 SESSION_CONTEXT 中的用户 Id。

现在表如下所示︰

![SSMS 联系人表](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

当创建新的联系人时，它们会自动分配正确的用户 Id。 出于演示目的，但是，让我们给这些现有联系人的几个现有用户。

如果您在该应用程序已创建几个用户 (如使用本地 Google、 Facebook 帐户)，您将看到它们的 AspNetUsers 表中。 在下面的屏幕快照，则只能有一个用户为止。

![SSMS AspNetUsers 表](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

复制 Id 为user1@contoso.com,，并将其粘贴到下面的 T SQL 语句。 执行此语句将三个联系人与此用户 Id 相关联。

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>步骤 3︰ 创建数据库中的行级别的安全策略

最后一步是创建一个安全策略，在 SESSION_CONTEXT 中使用用户 Id 自动筛选查询所返回的结果。

虽然仍连接到数据库，执行下面的 T SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

这段代码执行三个操作。 首先，创建新的架构作为一种最佳做法，为集中和限制 RLS 对象的访问。 下一步，它将创建谓词函数行的用户 Id 与在 SESSION_CONTEXT 的用户 Id 相匹配时，将返回"1"。 最后，它将创建为联系人表上的筛选器和块谓词添加此功能的安全策略。 筛选器谓词使查询只返回行属于当前用户，并阻止谓词作为一种安全措施来防止应用程序曾经意外插入行的错误的用户。

现在，运行应用程序，并作为登录user1@contoso.com。 现在该用户能够看到我们以前分配给此用户 Id 的联系人︰

![在启用 RLS 之前的联系人管理器应用程序](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

为了进一步验证这，请注册一个新用户。 他们将看到任何联系人，因为没有已分配给它们。 如果在创建新联系人时，会将它分配给他们，和他们只能够看到它。

## <a name="next-steps"></a>下一步行动

就是这样！ 简单的联系人管理器 web 应用程序转换为多租户之一，每个用户都有其自己的联系人列表。 通过使用行级安全，我们已经避免强制执行应用程序代码中的租户访问逻辑的复杂性。 这种透明性使应用程序能够将精力集中在真正的业务问题，同时也减少了为应用程序的基本代码意外泄露数据的风险增大。

本教程已简要介绍了可能与 RLS。 例如，就可能有更复杂的或细粒度访问逻辑，并可能将不只是当前用户 Id 存储在 SESSION_CONTEXT。 它也可能是对[集成与弹性数据库工具客户端库的 RLS](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md)支持中向外扩展数据层的多租户 shards。

除了这些可能性，我们正在使 RLS 甚至更好。 如果您有任何疑问，想法或您希望看到的事情，请让我们知道在注释。 我们非常感谢您的反馈 ！
