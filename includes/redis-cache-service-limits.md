| 资源                                    | 限制                                  |
|---------------------------------------------|----------------------------------------|
| 高速缓存大小                                  | （有关详细信息[请联系我们](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)） 530 GB                                  |
| 数据库                                   | 64                                     |
| 最大连接的客户端                       | 40000                                 |
| Redis （高可用性） 的缓存副本 | 1 |
| Shards 高级缓存群集中    | 10 |

Azure Redis 高速缓存限制，并且大小都不同，每个定价层。 要查看定价层和它们相关联的大小，请参阅[Azure Redis 缓存定价](https://azure.microsoft.com/pricing/details/cache/)。

Azure Redis 缓存配置限制的详细信息，请参阅[默认 Redis 服务器配置](redis-cache/cache-configure.md#default-redis-server-configuration)。

Azure Redis 缓存实例的配置和管理通过 Microsoft，因为不是所有的 Redis 命令支持 Redis Azure 的高速缓存中。 有关详细信息，请参阅 [Redis 命令中不支持 Azure 的 Redis Cache]((redis-cache/cache-configure.md#redis-commands-not-supported-in-azure-redis-cache)。