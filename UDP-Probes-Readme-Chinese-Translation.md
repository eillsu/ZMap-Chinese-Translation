UDP 数据探测
======

该目录包含一组可与UDP探测器模块一起使用的数据文件。


使用：
-----

```
$ zmap -M udp -p 137 --probe-args=file:examples/udp-probes/netbios_137.pkt
```


探测：
-----

```
citrix_1604.pkt         在UDP端口1604上触发Citrix应用程序发现服务的响应
db2disco_523.pkt        在UDP端口523上触发IBM DB2发现服务的响应
digi1_2362.pkt          在UDP端口2362上触发 Digi ADDP发现服务的响应（default magic）
digi2_2362.pkt          在UDP端口2362上触发 Digi ADDP发现服务的响应 (devkit magic)
digi3_2362.pkt          在UDP端口2362上触发 Digi ADDP发现服务的响应 (oem magic)
dns_53.pkt              使用UDP端口53上的BIND版本TXT记录查询DNS供应商和版本
dns_53_queryAwww.google.it.pkt     通过UDP端口53查询域www.google.it的A记录
dns_53_queryAwww.google.com.pkt    通过UDP端口53查询域www.google.com的A记录
ipmi_623.pkt            从UDP端口623上的IPMI端点触发Get Channel Authentication回复
mdns_5353.pkt           触发UDP端口5353上的mDNS/Avahi/Bonjour发现服务的响应
memcache_11211.pkt      触发UDP端口11211上的memcached响应（统计信息项）。
mssql_1434.pkt          触发UDP端口1434上的Microsoft SQL Server发现服务的响应
natpmp_5351.pkt         触发UDP端口5351上启用NATPMP的设备的响应
netbios_137.pkt         触发UDP端口137上的NetBIOS服务状态回复
ntp_123.pkt             触发UDP端口123上的NTP服务的响应
ntp_123_monlist.pkt     触发来自UDP端口123上的NTP服务的命令“monlist”的响应
pca_nq_5632.pkt         触发UDP端口5632上的PC Anywhere服务的响应（网络查询）
pca_st_5632.pkt         触发UDP端口5632上的PC Anywhere服务响应（状态）
portmap_111.pkt         触发UDP端口111上SunRPC端口映射器服务的响应
ripv1_520.pkt       	  触发UDP端口520上启用RIPv1的路由器/设备的响应
sentinel_5093.pkt       触发UDP端口5093上Sentinel许可证管理器服务的响应
snmp1_161.pkt           使用通过UDP端口161的社区字符串查询SNMP v1服务的系统描述字段
snmp2_161.pkt           使用community string public通过UDP端口161查询SNMP v2服务的系统描述字段
snmp3_161.pkt           触发UDP端口161上SNMP v3服务的响应
upnp_1900.pkt           触发UDP端口1900上UPnP SSDP服务的响应
wdbrpc_17185.pkt        触发UDP端口17185上的VxWorks WDBRPC服务的响应
wsd_3702.pkt            触发UDP端口3702上WSD/DPWS服务的响应
```

注意：
-----

这些探测文件大多数都会在响应中返回有用的数据。解析这些数据需要捕获原始输出并使用特定于协议的解剖器对其进行解码。在大多数情况下，Wireshark能够解码这些响应。