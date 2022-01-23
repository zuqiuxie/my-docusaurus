# nmap使用

## 基本使用

### 1、ping扫描

> 这种扫描方式以及-sP 扫描，检测网络上哪些主机正在运行，通过向指定的IP地址发送ICMP echo请求数据包，收到一个RST包，就表示主机正在运行。

```
namp -sP <ip>
```

### 2、TCP SYN Ping扫描

> 对于root用户，-PS 让nmap使用SYN包而不是ACK包来对目标主机进行扫描。如果主机正在运行就返回一个RST包(或者一个SYN/ACK包)。-PS默认在80端口发送TCP SYN数据包；我们还可以指定端口，例如-PS135(指定135)端口。当管理员对TCP SYN数据包中的SYN数据包没有过滤时可绕过。

```
nmap -sP -PS <ip>
```

### 3、TCP ACK Ping扫描

> 类比TCP SYN Ping扫描

```
nmap -sP -PA < 要扫描的目标ip地址>
```

### 4、半开扫描

> 扫描动作极少会被记录，更具有隐蔽性

```
nmap -sS < 要扫描的目标ip地址>
```

### 5、自定义端口扫描

```
nmap -p80-1000 < 要扫描的目标ip地址>
```

## 其他参数

### -Pn

>  非ping扫描，不执行主机发现，可以跳过防火墙

```
nmap -Pn < 要扫描的目标ip地址>
```

### -sV

>  探测打开端口对应服务的版本信息

```
nmap -sV < 要扫描的目标ip地址>
```

###  -vv

>  对扫描结果详细输出

```
nmap -vv < 要扫描的目标ip地址>
```

### -traceroute

> 路由追踪扫描

```
nmap -traceroute < 要扫描的目标ip地址>
```

### -O

> 操作系统检测

```
nmap -O < 要扫描的目标ip地址>
```

### -A

>  万能开关扫描
> 包含了1-10000端口ping扫描，操作系统扫描，脚本扫描，路由跟踪，服务探测 （花费时间长）

```
nmap -A < 要扫描的目标ip地址>
```

### C段扫描

```
nmap ip地址/24
```

### 扫描一个目标列表 

```
nmap -iL [ip地址列表文件]
```

## 脚本扫描

```
auth: 负责处理鉴权证书（绕开鉴权）的脚本 
broadcast: 在局域网内探查更多服务开启状况，如dhcp/dns/sqlserver等服务 
brute: 提供暴力破解方式，针对常见的应用如http/snmp等 
default: 使用-sC或-A选项扫描时候默认的脚本，提供基本脚本扫描能力 
discovery: 对网络进行更多的信息，如SMB枚举、SNMP查询等 
dos: 用于进行拒绝服务攻击 
exploit: 利用已知的漏洞入侵系统 
external: 利用第三方的数据库或资源，例如进行whois解析 
fuzzer: 模糊测试的脚本，发送异常的包到目标机，探测出潜在漏洞 

intrusive: 入侵性的脚本，此类脚本可能引发对方的IDS/IPS的记录或屏蔽 
malware: 探测目标机是否感染了病毒、开启了后门等信息 
safe: 此类与intrusive相反，属于安全性脚本 
version: 负责增强服务与版本扫描（Version Detection）功能的脚本 
vuln: 负责检查目标机是否有常见的漏洞（Vulnerability），如是否有MS08_067
```



```
负责处理鉴权证书（绕开鉴权）的脚本,也可以作为检测部分应用弱口令
nmap --script=auth ip

提供暴力破解的方式  可对数据库，smb，snmp等进行简单密码的暴力猜解
nmap --script=brute ip

默认的脚本扫描，主要是搜集各种应用服务的信息，收集到后，可再针对具体服务进行攻击
nmap --script=default ip

检查是否存在常见漏洞
nmap --script=vuln ip

在局域网内探查更多服务开启状况
nmap -n -p445 --script=broadcast ip

利用第三方的数据库或资源，例如进行whois解析
nmap --script external ip
```

![image-20211229173102970](https://tva1.sinaimg.cn/large/008i3skNly1gxuu2pfbooj30ey0brmy4.jpg)