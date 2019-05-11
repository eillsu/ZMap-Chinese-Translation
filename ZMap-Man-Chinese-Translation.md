# ZMap-Man-Chinese-Translation

翻译：eillsu

GitHub：https://github.com/eillsu/ZMap-Chinese-Translation

## 名称

zmap - 快速互联网扫描器

## 概要

```
zmap  [ -p <port> ] [ -o <outfile> ] [ OPTIONS... ] [ ip/hostname/range]
```

## 描述

ZMap 是一种用于扫描整个互联网（或大型样本）的网络工具。ZMap 能够在大约45分钟内通过千兆网络连接扫描整个互联网，达到约98%的理论线路速度。

## 选项

### 基础选项

```
   ip/hostname/range
          要扫描的 IP 地址或者 DNS 主机名。接受使用 CIDR 块表示法中的IP范围。默认为0.0.0/8
   -p, --target-port=port
   				要扫描的 TCP 或者 UDP端口号(进行 SYN 和 基本的 UDP 扫描)。
   -o, --output-file=name
          当使用输出模块，将结果输出到文件。用 - 表示屏幕输出 stdout。
   -b, --blacklist-file=path
          说明要排除的子网的文件，使用 CIDR 表示法，每行一个。建议使用此选项排除 RFC 1918 地址、多播、IANA 保留空间和其他IANA专用地址。blacklist.conf 是示例文件。参考/etc/zmap/blacklist.conf。
```

### 扫描选项

```
   -n, --max-targets=n
          限制要探测的目标数量。这可以是一个数字（例如-n 1000）或可扫描地址空间的百分比（例如-n 0.1%）（在排除黑名单后）。
   -N, --max-results=n
          扫描到这么多结果后退出。
   -t, --max-runtime=secs
          限制发送数据包的时间长度。
   -r, --rate=pps
          以包/秒为单位设置发送速率。
   -B, --bandwidth=bps
          以位/秒为单位设置发送速率（支持后缀g、m和k（例如-b 10M表示10Mbps）。这将覆盖--rate标志。
   -c, --cooldown-time=secs
          发送完成后继续接收的时间（默认值=8）
   -e, --seed=n
          用于选择地址排序，如果你想运行多个zmap对地址进行扫描时使用。
   --shards=N
          将扫描分割为zmap的不同实例中的N个分片/分区（默认值= 1）。当分片时，需要设置 --seed。
   --shard=n
          设置要扫描的分片(默认值 = 0）。分片在[0, N)范围内被从0开始索引(0-indexed)，其中 N 是分片的总数。当分片的时候，需要设置 --seed。
   -T, --sender-threads=n
          用于发送数据包的线程数。ZMap将尝试根据处理器核心数检测最佳发送线程数。
   -P, --probes=n
          要发送到每个IP的探测数量（默认值= 1）。
   -d, --dryrun
          将每个数据包打印到stdout而不是发送它（对于调试很有用）
```

### 网络选项

```
   -s, --source-port=port|range
          发送的数据包的源端口(一个或多个)。
   -S, --source-ip=ip|range
          发送数据包的源地址(一个或多个)。单个IP或范围（例如10.0.0.1-10.0.0.9）。
   -G, --gateway-mac=addr
          发送数据包的网关 MAC 地址(以防自动检测不起作用)。
   -i, --interface=name
          要使用的网络接口。
```

### 探测选项

ZMap允许用户指定和编写自己的探测模块。探测模块负责生成要发送的探测包，并处理来自主机的响应。

       --list-probe-modules
              列出可用的探测模块（例如tcp_synscan）。
       -M, --probe-module=name
              选择探测模块（默认 = tcp_synscan）。
       --probe-args=args
              传递给探测模块的参数。
       --list-output-fields
              列出所选探测模块可以发送到输出模块的字段。

### 输出选项

ZMap允许用户指定和编写自己的输出模块，并用于 ZMap。输出模块负责处理探测模块返回的字段集，并将它们输出给用户。用户可以指定输出字段，并在输出字段上写入过滤器。

       --list-output-modules
              列出可用的输出模块（例如 tcp_synscan）。
       -O, --output-module=name
              选择输出模块（默认 = csv）。
       --output-args=args
              传递给输出模块的参数。
       -f, --output-fields=fields
              以逗号分隔的要输出的字段列表。
       --output-filter
              在探测模块定义的字段上指定输出过滤器。有关详细信息，请参阅输出过滤器部分。

### 其他选项

```
   -C, --config=filename
         读取配置文件，该文件可以指定任何其他选项。
   -q, --quiet
          不要每秒打印一次状态更新。
   -g, --summary
          在扫描结束时打印配置和结果摘要。
   -v, --verbosity=n
          日志详细程度（0-5，默认 = 3）。
   -h, --help
          打印帮助并退出。
   -V, --version
          打印版本并退出。
```

### UDP 探测模块选项

这些参数都使用 `--probe-args = args` 选项传递。 一次只能传递一个参数。

       file:/path/to/file
              要通过UDP发送到每个主机的有效载荷文件的路径。
       template:/path/to/template
              模板文件的路径。对于每个目标主机，将填充模板文件，设置为 UDP 有效载荷并发送。
       text:<text>
              要发送到每个目标主机的 ASCII 文本。
       hex:<hex>
              发送到每个目标主机的十六进制编码的二进制文件。
       template-fields
              打印被允许的模板字段的信息并退出。

### 输出过滤器

探测模块生成的结果可以在传输到输出模块之前进行过滤。在探测模块的输出字段中定义过滤器。过滤器使用简单的过滤语言编写，类似于 SQL，并使用 `--output-filter` 选项传递给 ZMap。输出过滤器通常用于过滤掉重复的结果，或仅传递成功响应到输出模块。

过滤器表达式的格式为`<fieldname> <operation> <value>`。`<value>`的类型必须是字符串或无符号整数，并且匹配`<fieldname>`的类型。整数比较的有效操作是`=` `!=`, `<`, `>`, `<=`, `>=`。字符串比较的操作是 `=`, `!=`。`--list-output-fields`标志将打印所选探测模块可用的字段和类型，然后退出。

可以通过组合过滤表达式构造复合过滤器表达式，使用括号来指定操作顺序，`&&`（逻辑AND）和`||`（逻辑OR）运算符。

例如，仅针对成功的非重复响应的过滤器将写为：`--output-filter="success = 1 && repeat = 0"`

zmap v2.1.1

September 2015

(END)