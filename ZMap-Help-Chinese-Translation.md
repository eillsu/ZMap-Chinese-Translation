# ZMap-Help-Chinese-Translation

翻译：eillsu

GitHub：https://github.com/eillsu/ZMap-Chinese-Translation

zmap 2.1.1

快速的互联网扫描器。

```
用法: zmap [OPTIONS]... [SUBNETS]...
```

## 基本参数：

```
  -p, --target-port=port        扫描的端口 (for TCP and UDP scans)。
  -o, --output-file=name        输出文件。
  -b, --blacklist-file=path     要以CIDR表示法排除的子网文件，例如192.168.0.0/16
  -w, --whitelist-file=path     用CIDR表示法限制扫描的子网文件，例如， 192.168.0.0/16
```

## 扫描选项：

```
  -r, --rate=pps                以数据包/秒为单位设置发送速率。
  -B, --bandwidth=bps           设置发送速率，以位/秒为单位（支持后缀G，M和K）。
  -n, --max-targets=n           要探测的目标数量（作为地址空间的数量或百分比）。
  -t, --max-runtime=ses         发送数据包的上限时间。
  -N, --max-results=n           要返回的结果的上限数量。
  -P, --probes=n                要发送到每个IP的探测数量（默认 = 1）。
  -c, --cooldown-time=secs      发送最后一次探测后继续接收多长时间（默认 = 8）。
  -e, --seed=n                  用于选择地址排列的种子。
  		--retries=n                   发送失败时尝试发送数据包的最大次数（默认= 10）。
  -d, --dryrun                  实际上不发送数据包。
  		--shards=N                    设置分片总数（默认 = 1）。
  		--shard=n                     设置扫描哪个分片（从0开始的索引）（默认 = 0）。
```

## 网络选项：

```
  -s, --source-port=port|range  扫描数据包的源端口。
  -S, --source-ip=ip|range      扫描数据包的源地址。
  -G, --gateway-mac=addr        指定网关MAC地址。
      --source-mac=addr         源MAC地址。
  -i, --interface=name          指定要使用的网络接口。
  -X, --vpn                     发送IP数据包而不是用以太网（用于VPN）。
```

## 探测模块：

```
  -M, --probe-module=name       选择探测模块（默认 = tcp_synscan）。
  		--probe-args=args             传递给探测模块的参数。
 		  --list-probe-modules          列出可用的探测模块。
```

## 数据输出：

```
  -f, --output-fields=fields    在结果集中应该被输出的字段。
  -O, --output-module=name      选择输出模块(默认 = default)。
  		--output-args=args            传递给输出模块的参数。
  		--output-filter=filter        在响应字段上指定过滤器以限制将哪些响应发送到输出模块。
  		--list-output-modules         列出可用的输出模块。
 		  --list-output-fields          列出所选探测模块可以输出的所有字段。
```

## 记录和元数据： 

```
 -v, --verbosity=n              日志详细程度(0-5)(default= 3)
 -l, --log-file=name            将日志条目写入文件
 -L, --log-directory=directory  将日志条目写入此目录中的带时间戳的文件
 -m, --metadata-file=name       扫描元数据的输出文件（JSON）
 -u, --status-updates-file=name 将扫描进度更新到CSV文件
 -q, --quiet                    不要打印状态更新
 		 --disable-syslog               禁用日志消息到syslog
 	   --notes=notes                  将用户指定的注释注入扫描元数据
 		 --user-metadata=json           将用户指定的JSON元数据注入扫描元数据
```

## 其他选项：

```
  -C, --config=filename         读取配置文件，该文件可以指定任何选项。
                                  (default= /usr/local/etc/zmap/zmap.conf)
      --max-sendto-failures=n   扫描中止前最大NIC发送失败（默认 = -1）。
      --min-hitrate=n           在扫描中止之前扫描可以达到的最低命中率（默认值 = 0.0）。
  -T, --sender-threads=n        用于发送数据包的线程（默认 = 1）。
      --cores=STRING            Comma-separated list of cores to pin to
      --ignore-invalid-hosts    忽略白名单/黑名单文件中的无效主机
  -h, --help                    打印帮助并退出
  -V, --version                 打印版本并退出
```

## 例子：

```
zmap -p 80 (在Internet上搜索tcp/80上的主机并输出到stdout)
zmap -N 5 -B 10M -p 80 (找到5个HTTP服务器，以10 Mb/s的速度扫描)
zmap -p 80 10.0.0.0/8 192.168.0.0/16 -o (在tcp/80上扫描两个子网)
zmap -p 80 1.2.3.4 10.0.0.3 (在tcp/80上扫描1.2.3.4,10.0.0.3)
```

## 探测模块 (tcp_synscan) 帮助：

向特定端口发送TCP SYN数据包的探测模块。可能的分类是：synack和rst。SYN-ACK数据包被认为是成功的，复位rst数据包被认为是失败的响应。

## 输出模块(csv) 帮助：

默认情况下，ZMap将唯一的成功的 IP 地址(例如 SYN-ACK from a TCP SYN scan) 以 ASCII 的形式(例如 192.168.1.5)输出到 stdout 或者指定的输出文件。在内部，这由“csv”输出模块处理，相当于运行zmap --output-module = csv --output-fields = saddr --output-filter =“success = 1 && repeat = 0”。