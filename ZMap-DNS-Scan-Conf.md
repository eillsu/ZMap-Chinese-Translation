# ZMap-DNS-Scan-Conf

ZMap 扫描 DNS 服务器的配置文件。

系统：Linux & MacOS

## 自定义配置文件

在`/etc/zmap/zmap.conf`中指定或使用自定义配置文件。

```
$ cat zmap.conf
### Probe Module to use
probe-module udp

### Destination port to scan
target-port 53

### Scan rate in packets/sec
# rate 10000

### Scan rate in bandwidth (bits/sec); overrides `rate`
# bandwidth 1M	# 1mbps

### Blacklist file to use. We encourage you to exclude
### RFC1918, IANA reserved, and multicast networks,
### in addition to those who have opted out of your
### network scans.
blacklist-file "/usr/local/etc/zmap/blacklist.conf"  # MacOS

whitelist-file "/usr/local/etc/zmap/whitelist.conf"  # MacOS

### Optionally print a summary at the end
summary
```

ZMap支持使用配置文件来代替在命令行上指定所有要求的选项。配置中可以通过每行指定一个长名称的选项和对应的值来创建：

```
interface "eth1"
source-ip 1.1.1.4-1.1.1.8
gateway-mac b4:23:f9:28:fa:2d # upstream gateway
cooldown-time 300 # seconds
blacklist-file /etc/zmap/blacklist.conf
output-file ~/zmap-output
quiet
summary
```

然后ZMap就可以按照配置文件并指定一些必要的附加参数运行了：

```
$ zmap --config=~/.zmap.conf --target-port=4
```

```
$ zmap -M udp -p 53 --probe-args=file:examples/udp-probes/dns_53.pkt

dns_53.pkt              使用UDP端口53上的BIND版本TXT记录查询DNS供应商和版本

dns_53_queryAwww.google.it.pkt     通过UDP端口53查询域www.google.it的A记录

dns_53_queryAwww.google.com.pkt    通过UDP端口53查询域www.google.com的A记录


zmap -M UDP -p 53 -o dnslist.csv
```

-p, --target-port=port

要扫描的目标TCP端口号（例如，443）

-o, --output-file=name

将结果写入该文件，使用`-`代表输出到标准输出。

-b, --blacklist-file=path

文件中被排除的子网使用CIDR表示法（如192.168.0.0/16），一个一行。建议您使用此方法排除RFC 1918地址、组播地址、IANA预留空间等IANA专用地址。在conf/blacklist.example中提供了一个以此为目的示例黑名单文件。

-r, --rate=pps

设置发包速率，以包/秒为单位

-B, --bandwidth=bps

以比特/秒设置传输速率（支持使用后缀G，M或K（如`-B 10M`就是速度10 mbps）的。设置会覆盖`--rate`。

-T, --sender-threads=n

用于发送数据包的线程数（默认值= 1）

-M, --probe-module=name

选择探测模块（默认值= tcp_synscan）

--probe-args=args

向模块传递参数

ZMap可以通过使用--probe-args命令行选项来设置四种不同的UDP载荷。这些是：可在命令行设置可打印的ASCII 码的‘text’载荷和十六进制载荷的‘hex’，外部文件中包含载荷的‘file’，和通过动态字段生成的载荷的‘template’。

-O, --output-module=name

选择输出模块（默认值为csv）

-f, --output-fields=fields

输出的字段列表，以逗号分割

为了得到UDP响应，请使用-f参数确保您指定的“data”字段处于输出范围。

--output-filter

指定输出过滤器对探测模块定义字段进行过滤

--output-filter="success = 1 && repeat = 0"

-C, --config=filename

加载配置文件，可以指定其他路径。

-q, --quiet

不必每秒刷新输出

-g, --summary

在扫描结束后打印配置和结果汇总信息

-v, --verbosity=n

日志详细程度（0-5，默认值= 3）

## 白名单文件

需要配置白名单文件。如果指定了白名单文件，只有那些网络前缀在白名单内的才会扫描。

-w, --whitelist-file=path

文件用于记录限制扫描的子网，以CIDR的表示法，例如192.168.0.0/16



## 黑名单文件

```
$ cat blacklist.conf

# From IANA IPv4 Special-Purpose Address Registry

# http://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml

# Updated 2013-05-22

0.0.0.0/8           # RFC1122: "This host on this network"
10.0.0.0/8          # RFC1918: Private-Use
100.64.0.0/10       # RFC6598: Shared Address Space
127.0.0.0/8         # RFC1122: Loopback
169.254.0.0/16      # RFC3927: Link Local
172.16.0.0/12       # RFC1918: Private-Use
192.0.0.0/24        # RFC6890: IETF Protocol Assignments
192.0.2.0/24        # RFC5737: Documentation (TEST-NET-1)
192.88.99.0/24      # RFC3068: 6to4 Relay Anycast
192.168.0.0/16      # RFC1918: Private-Use
198.18.0.0/15       # RFC2544: Benchmarking
198.51.100.0/24     # RFC5737: Documentation (TEST-NET-2)
203.0.113.0/24      # RFC5737: Documentation (TEST-NET-3)
240.0.0.0/4         # RFC1112: Reserved
255.255.255.255/32  # RFC0919: Limited Broadcast

# From IANA Multicast Address Space Registry

# http://www.iana.org/assignments/multicast-addresses/multicast-addresses.xhtml

# Updated 2013-06-25

224.0.0.0/4         # RFC5771: Multicast/Reserved
```

## MacOS系统说明

安装 ZMap

```
$ brew install zmap
```

在mac系统中的位置是不同的，/usr/local/Cellar/zmap/2.1.1_1/etc/zmap/zmap.conf是配置文件所在位置。

```
$ brew info zmap
zmap: stable 2.1.1 (bottled), HEAD
Network scanner for Internet-wide network studies
https://zmap.io
/usr/local/Cellar/zmap/2.1.1_1 (14 files, 262.4KB) *
  Poured from bottle on 2018-09-20 at 15:45:00
From: https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git/Formula/zmap.rb
==> Dependencies
Build: byacc ✘, cmake ✘, gengetopt ✘, pkg-config ✘
Required: gmp ✔, json-c ✔, libdnet ✔
==> Options
--HEAD
	Install HEAD version
==> Analytics
install: 154 (30 days), 502 (90 days), 2,513 (365 days)
install_on_request: 150 (30 days), 480 (90 days), 2,215 (365 days)
build_error: 0 (30 days)
```

