---
description: 基于OSI模型协议探测目标信息
---

# 主机探测

## Nmap Options

|                          |                         |
| ------------------------ | ----------------------- |
| 目标主机探测响应在200ms内          | --max-rtt-timeout 200ms |
| 每秒发送至少1000个数据包           | --min-rate 1000         |
| **指定 -P 选项，会替换默认主机发现探针** | -P                      |
| 只针对主机发现，不进行端口扫描          | -sn                     |
| 跳过主机发现的过程，视主机开启          | -Pn                     |
| 不进行DNS解析                 | -n                      |
| 路由追踪                     | --traceroute            |

### 扫描策略

{% tabs %}
{% tab title="探测" %}
|                  |                                                                |
| ---------------- | -------------------------------------------------------------- |
| 全面扫描             | nmap –T4 –A –v                                                 |
| 扫描C段存活\<ICMP>    | nmap -sn -PE -T4 192.168.0.0/24                                |
| 扫段主机\<Ping>      | nmap -sP 192.168.0.1/24                                        |
| 无ping探测          | nmap -p0 192.168.123.1/24                                      |
| 快速探测             | nmap -n -F \<target>                                           |
| 忽略主机状态，全端口探测     | nmap -Pn -n  \<target>  -p-                                    |
| ARP探测            | nmap -sn -PR 192.168.1.1/24                                    |
| snmp协议探测         | nmap -sU --script snmp-brute 192.168.1.0/24 -T4                |
| smb协议探测          | nmap ‐sU ‐sS ‐‐script smb‐enum‐shares.nse ‐p 445 192.168.1.119 |
| 利用FIN扫描方式探测防火墙状态 | namp -sF -T4                                                   |
{% endtab %}

{% tab title="主机发现" %}
|                         |                                       |
| ----------------------- | ------------------------------------- |
| -sL                     | 列表扫描，仅将指定的目标的IP列举出来                   |
| -PE/PP/PM               | ICMPecho, timestamp, and netmask包发现主机 |
| -PO\[protocollist]      | 使用IP协议包探测主机是否开启。                      |
| -PS/PA/PU/PY\[portlist] | 使用TCPSYN/ACK或SCTP INIT/ECHO方式进行发现。    |
{% endtab %}

{% tab title="端口发现" %}
|             |                            |
| ----------- | -------------------------- |
| -sS         | -sS SYN扫描，半连接扫描            |
| -sA         | -sA ACK扫描，适合用来映射防火墙规则      |
| -sW         | 窗口扫描原理与ACK扫描相同             |
| -sM         | Maimon扫描，使用 FIN/ACK 数据包为探针 |
| -sU         | 多数Linux系统会限制 ICMP响应速率      |
| -sN/sF/sX   | 设置TCP标志位，通常只能确定关闭的端口       |
| --scanflags | 自定义扫描，可以定制化TCP标志位          |
| -sI         | 空闲扫描，僵尸扫描                  |
| -sO         | 用于判断目标主机所支持的协议             |
{% endtab %}

{% tab title="DNS反向解析" %}
|                                     |               |
| ----------------------------------- | ------------- |
| -R                                  | 总是进行DNS解析     |
| --dns-servers \<serv1\[,serv2],...> | 指定DNS服务器      |
| --system-dns                        | 指定使用系统的DNS服务器 |
{% endtab %}
{% endtabs %}

### 端口参数详情

{% tabs %}
{% tab title="-sS" %}
\-sS SYN扫描，半连接扫描，优点是扫描速度快，不容易被记录，适用于任何兼容TCP堆栈，可以清晰可靠地区分端口开启、关闭和过滤状态。但需要root权限去构造原始数据包，目前IDS可以检测到这种扫描方式
{% endtab %}

{% tab title="-sA " %}
\-sA ACK扫描，适合用来映射防火墙规则。

ACK扫描不能确定端口为开发状态，可以用于映射防火墙规则集，确定是否有状态监测防火墙，以及对哪些端口进行了过滤。当扫描被过滤系统时，无论是开启或关闭的端口都会返回RST数据包，则Nmap会标记为 unfiltered ；若端口没有响应包或收到ICMP不可达报错，则标记为 filtered。
{% endtab %}

{% tab title="-sW " %}
窗口扫描原理与ACK扫描相同，但它会利用系统的实现细节区分端口开放和关闭，而不是都标记为 unfiltered。它会对RST数据包的TCP窗口值进行判断，若窗口字段为0，则该端口标记为closed，若端口字段非0，则标记为open。

这种扫描技术依赖于部分操作系统的特性，普遍性不高。某些系统开放的端口上窗口大小为正数，而关闭端口则为0。
{% endtab %}

{% tab title="-sM " %}
sM Maimon扫描，以发现者Uriel Maimon命名，使用 FIN/ACK 数据包为探针。

根据RFC 793（TCP），无论端口是打开还是关闭都应生成RST数据包来响应此探测。但是，Uriel注意到，如果端口是开放的，则许多BSD派生的系统只会丢弃该数据包，Nmap借此确定打开的端口。若无响应包，则标记为 open|filtered ；若收到RST包，则端口为 closed；若收到ICMP不可达报错，则端口为 filtered。

不过在现代系统上很少出现这种错误，他们都将回复RST数据包，因此用处不大。
{% endtab %}

{% tab title="-sU " %}
\-sU UDP扫描，速度很慢，扫描上千个端口需要至少17分钟，多数Linux系统会限制 ICMP响应速率。

UDP扫描会向指定的目标端口发送UDP数据包，对于大多数端口此数据包为空（无有效载荷），但对于一些常见端口，将发送特定协议的有效载荷数据包。根据响应包的存在与否，将端口分为四个状态：

> 目标端口的任何 UDP 响应 (异常) open
>
> 未收到返回信息 (即便重传) open|filtered
>
> ICMP 端口不可达错误 (type 3, code 3) closed
>
> 其他 ICMP 不可达错误 (type 3, code 1, 2, 9, 10, or 13) filtered

\

{% endtab %}

{% tab title=" -sN/sF/sX" %}
\-sN/sF/sX 设置TCP标志位。这三种扫描类型（以及--scanflags选项）利用TCP RFC 标准中的细节来区分开放端口和关闭端口，通常只能确定关闭的端口。

当目标主机符合RFC标准时，发送一个不包含SYN，RST，ACK标志的数据包，当目的端口关闭时将返回RST包，当端口开放时目标主机将丢弃数据包，因此无响应包，但也可能是防护设备导致无响应。因此只要不包含SYN、RST、ACK这三个位，则其他三个位（FIN，PSH和URG）的任何组合都可以。

> \-sN 不设置任何标志位；
>
> \-sF 只设置TCP FIN位，FIN位表示关闭连接（finish）；
>
> \-sX 设置FIN、PSH和URG标志，PSH表示数据传输（push)，URG表示紧急（urgent）。

根据扫描结果，将端口分为三种状态，若无响应包，则标记端口状态为 open|filtered；若收到TCP RST包，则端口状态为 closed；若收到 ICMP不可达错误（type 3, code 1, 2, 9, 10, or 13)，则标记端口状态为 filtered。

这三种扫描优点在于可以规避部分非状态防火墙和包过滤路由设备，而且比SYN更加隐蔽；但缺点是并非所有的系统都遵循RFC 793标准，而且无法区分 open 和 filtered 端口。协议探测扩展
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="--scanflags" %}
\--scanflags 自定义扫描，可以定制化TCP标志位，原理同上。可以通过该选项设置 URG、ACK、PSH、RST、SYN 和 FIN 的任意组合，这种定制化可以用来研究绕过防火墙设备，这块没有研究，欢迎评论。
{% endtab %}

{% tab title="-sl" %}


\-sI 空闲扫描，也有称之为僵尸扫描，利用僵尸主机进行扫描，优点是隐蔽性强，缺点是需要县找一台合适的僵尸主机。网上有文章。

首先我们需要知道：

> 确定TCP端口状态的一种方法是发送SYN数据包到该端口。若端口开放，则目标主机将响应 SYN / ACK（会话请求确认）数据包；若端口关闭，则目标主机将响应RST（重置）数据包。
>
> 收到未经请求的SYN / ACK数据包的计算机将以RST响应，未经请求的RST将被忽略。
>
> 每个IP数据包都有一个片段标识号（IP ID）。由于许多操作系统发送的每个数据包时只是增加此数字，因此探测IP ID可以告诉攻击者自上次探测以来已发送了多少个数据包。

通过组合这些特点，可以伪造您的身份，同时扫描目标网络，从而看起来像是无辜的僵尸机器进行了扫描。简单来说，空闲扫描技术主要是进行下面三步：

> 1.探测僵尸的IP ID并将其记录下来。
>
> 2.伪造来自僵尸的SYN数据包，并将其发送到目标主机的端口上。根据端口状态，目标的反应可能会导致僵尸的IP ID增加。
>
> 3.再次探测僵尸主机的IP ID。然后，通过将此新IP ID与步骤1中记录的IP ID进行比较来确定目标端口状态。若ID加2，则端口开放。
{% endtab %}

{% tab title="-sY/sZ" %}
\-sY/sZ SCTP INIT/COOKIE-ECHO扫描，这两种扫描方式是基于SCTP协议。

SCTP（Stream Control Transmission Protocol，流控制传输协议）是IETF（Internet Engineering Task Force）在2000年定义的一个传输层协议。SCTP可以看作是TCP协议的改进，它改进了TCP的一些不足。它一种提供了可靠、高效、有序的数据传输协议。相比之下TCP是面向字节的，而SCTP是针对成帧的消息。RFC-4960详细地定义了SCTP，介绍性的文档是RFC 3286。

TCP建立连接的过程是：SYN、SYN/ACK、ACK；

而SCTP建立连接的过程是：INIT、INIT-ACK、COOKIE-ECHO、COOKIE-ACK

![](../../.gitbook/assets/1623766058\_60c8b42aa48461d9a2411.png)\
主机发现阶段的 -PY 选项与 -sY 原理一样，通过向目标主机的指定端口发送 INIT数据包，根据主机响应判断是否存活。

\-sY/sZ 与TCP协议下的 -sS/sA，思想是一致的。\
\-sY SCTP INIT扫描是TCP SYN扫描的SCTP等效物。优点是速度快，不受限制性防火墙的限制。与SYN扫描一样，INIT扫描相对隐蔽，因为它永远不会完成SCTP连接。能有效区分开启，关闭和过滤状态。

\-sZ SCTP COOKIE-ECHO扫描，利用了SCTP协议在实现上，若向开放的端口发送COOKIE-ECHO请求，则该端口会直接丢弃该数据包不做响应；而对关闭的端口，则目标主机会返回ABORT响应。同样也可以用于检测基于状态检测的防火墙。 缺点是不能区分打开和过滤的端口。
{% endtab %}

{% tab title="-sO " %}
\-sO IP层协议扫描，这不算是端口扫描技术，该扫描技术用于判断目标主机所支持的协议。

该扫描技术允许用户通过-p选择扫描的协议号，也可以使用-F扫描nmap-protocols文件中的所有协议（共145种协议），默认情况下会扫描256个可能的值，以常规端口表格形式报告结果。
{% endtab %}

{% tab title="-b " %}
\-b FTP bounce scan，逐渐弃用

这种扫描技术是利用FTP服务器的功能。FTP协议（RFC 959）的一个有趣功能是支持所谓的代理FTP连接。这使用户可以连接到一个FTP服务器，然后要求将文件发送到第三方服务器。在许多级别上，这种功能已经很容易被滥用，其中之一是导致FTP服务器对其他主机进行端口扫描。因此大多数FTP服务器都不再支持该功能，所以这项扫描技术也基本弃用了。
{% endtab %}
{% endtabs %}

### UDP优化

<mark style="background-color:yellow;">增加主机并行数（--min-hostgroup）同时扫描多个主机；优先扫描常见端口-F；版本探测时，指定 --version-intensity 0 仅尝试最有可能针对给定端口号有效的探针；从防火墙后扫描；--host-timeout 指定主机超时时间，跳过响应缓慢的主机；最后一条，保持冷静，做点别的，不要盯着进度条，眼不见心不烦（/doge/）</mark>

## 协议拓展

### NetBios协议探测

该协议作用于局域网，为程序提供了请求低级服务统一的命令集。系统可以利用WINS服务、广播及Lmhost文件等多种模式将基于NETBIOS协议获得计算机名称解析为相应IP地址，在局域网内部使用NetBIOS协议可以方便地实现消息通信及资源的共享。

{% tabs %}
{% tab title="nmap" %}
```
nmap -sU --script nbstat.nse -p137 192.168.1.0/24 -T4
```
{% endtab %}

{% tab title="msf" %}
```
msf > use auxiliary/scanner/netbios/nbname
```
{% endtab %}

{% tab title="nbtscan" %}
win

```
nbtscan-1.0.35.exe -m 192.168.1.0/24
nbtscan -n
```

linux

```
nbtscan -r 192.168.1.0/24
nbtscan -v -s: 192.168.1.0/24
```
{% endtab %}
{% endtabs %}

#### ICMP协议探测 <a href="#scroller-21" id="scroller-21"></a>

{% tabs %}
{% tab title="CMD" %}
输出到文件

{% code overflow="wrap" %}
```
@for /l %i in (1,1,255) do @ping -n 1 -w 40 192.168.0.%i & if errorlevel 1 (echo 192.168.0.%i>>f:\a.txt) else (echo 192.168.0.%i >>f:\b.txt)
```
{% endcode %}
{% endtab %}
{% endtabs %}

### ARP协议探测

{% tabs %}
{% tab title="MSF" %}
```
msf > use auxiliary/scanner/discovery/arp_sweep
```
{% endtab %}

{% tab title="netdiscover" %}
```
netdiscover -r 192.168.1.0/24 -i wlan0
```
{% endtab %}

{% tab title="arp scan" %}
```
arp-scan --interface=wlan0 --localhost
```

win

```
arp-scan.exe -t 192.168.1.1/24
```

**Powershell**

{% code overflow="wrap" %}
```
c:\tmp>powershell.exe -exec bypass -Command "Import-Module .\arpscan.ps1;InvokeARPScan -CIDR 192.168.1.0/24"
```
{% endcode %}
{% endtab %}
{% endtabs %}

### SNMP

SNMP主要用于网络设备的管理。SNMP协议主要由两大部分构成：SNMP管理站和SNMP代理。SNMP管理站是一个中心节点，负责收集维护各个SNMP元素的信息，并对这些信息进行处理，最后反馈给网络管理员；而SNMP代理是运行在各个被管理的网络节点之上，负责统计该节点的各项信息，并且负责与SNMP管理站交互，接收并执行管理站的命令，上传各种本地的网络信息。

{% tabs %}
{% tab title="MSF" %}
```
msf > use auxiliary/scanner/snmp/snmp_enum
```
{% endtab %}
{% endtabs %}

### SMB

{% tabs %}
{% tab title="MSF" %}
```
scanner/smb/smb_version
```
{% endtab %}

{% tab title="CMD" %}
```
for /l %a in (1,1,254) do start /min /low telnet 192.168.1.%a 445
```
{% endtab %}
{% endtabs %}
