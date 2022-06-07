# 系统信息收集

# 1、wind

## wind系统排查

### 系统基本信息

```
systeminfo
```

### 用户信息

```
net user
wmic useraccount get name ,SID
```

### 任务计划

```
PowerShell -Command "& {get-scheduledtask}"
```

### 进程

```
tasklist
```

### 查看防火墙

```
netsh advfirewall firewall show rule  name=all
```

### 查看网络连接

```
netstat -ano
```



## 查看系统启动项

```
get-scheduledtask

PowerShell -Command "& {get-scheduledtask}"
```

查看防火墙

```
netsh advfirewall firewall show rule  name=all
```

查看网络连接

```
netstat -ano
```

### Recent

### 修改时间工具

attribute changer

## websehll 查杀

河马

u盾

# 二、linux

```
lscpu
```

查看内核

```
uname -a
cat /proc/version
lsmod
```

查看特权用户

```
awk -F: '{if($3==0)print $1}' /etc/passwd
```

查看登录信息

```
 sudo lastb   
```

查看用户最后登录信息

```
sudo lastlog
```

启动项

```
cat /etc/init.d/rc.local 
cat /etc/rc.local 
ls -alt /etc/init.d 
```

计划任务

```
crontab -l   
cat /etc/crontab 
```

查看进程

```
ps -aux
```

网络连接

```
netstat -antlp
```

进程查看

```
ls -alt /proc/[pid]
lsof -p [pid]
```

文件属性

```
lsattr 查看文件属性
chattr 修改文件属性
```

查看隐藏进程

```
ps -ef | awk '{print $2}' | sort -n | uniq >1
ls /proc | sort -n | uniq >2
diff 1 2
```

服务排查

```
chkocnfig --lsit
```

查找文件

```
时间查找
find / -ctime 0 -name "*.sh"

-mtime  更改时间
-atime  访问时间
-ctime  创建时间
-n  n天以内
+n  n天之前

权限查找
find / -perm 777

webshell查找
find / -name "*.php" -o -name "*.jsp" | xargs eval
find / -name "*.php" -o -name "*.jsp" |xargs egrep 'assert|phpspy|c99sh|milw0rm|eval|\(gunerpress|\(base64_decoolcode|spider_bc|shell_exec|passthru|\(\$\_\POST\[|eval \(str_rot13|\.chr\(|\$\{\"\_P|eval\(\$\_R|file_put_contents\(\.\*\$\_|base64_decode'


查找SUID程序
find / -type f -perm -05000 -ls -uid 0 2>/dev/null
```

日志排查

```
查询最后10行: tail -n 10 file
查询10行之后: tail -n +n file
查询前10行: head -n 10 file
查询开始至最后10行: head -n -10 file
查找文件中独立ip地址个数: awk '{print $1}' file |sort|uniq|wc -l
查找指定时间段日志: sed
```

#
