#### <a name="create-an-a-record-set-with-single-record"></a>创建与单个记录集的记录

若要创建的记录集，请使用`azure network dns record-set create`。 指定资源组，区域名称记录设置相对名称、 记录类型和时间来生存 (TTL)。

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

之后创建一个记录集，将 IPv4 地址添加到记录集的`azure network dns record-set add-record`。

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>创建与单个记录集 AAAA 记录

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>创建与单个记录集的 CNAME 记录

CNAME 记录只允许一个单个字符串值。


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>创建 MX 记录与单个记录集

在此示例中，我们使用记录集名称"@"在区域 apex 创建 MX 记录 (在本例中，"contoso.com")。 这是常见的 MX 记录。

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>创建与单个记录集的 NS 记录

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>创建与单个记录集的 PTR 记录  
在这种情况下我的 arpa-zone.com' 表示表示 IP 范围的 ARPA 区域。  在此区域中设置每个 PTR 记录与此 IP 地址范围内的 IP 地址相对应。    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>创建 SRV 记录与单个记录集

如果在区域的根中创建一条 SRV 记录，您可以指定记录名称中的"_service"和"_protocol"。 没有必要包括"@"在记录名称。


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>创建与单个记录集的 TXT 记录

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
