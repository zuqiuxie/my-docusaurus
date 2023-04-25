## 1、数据库安装和ossec数据库配置

我数据库放在本机

1）创建ossec用户，创建ossec数据库

```
mysql -uroot -proot 
 
create database ossec;
 
grant all on ossec.* to ossec@localhost;   #授权
 
set password for ossec@%=password('ossec');

flush privileges;
 
exit
```

2）创建ossec表结构

ossec源码

https://codeload.github.com/ossec/ossec-hids/zip/refs/tags/v2.8.3

```
tar zxf ossec-hids-2.8.3.tar.gz
cd ossec-hids-2.8.3
mysql -uossec -p ossec < ./src/os_dbd/mysql.schema
```

![image-20220503140424598](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6k59k2yj21cw0rqtaw.jpg)

## 2、ossec-server ossec-agent服务端安装

#### 1）ossec-server

1）docker一键安装

```
docker run --name ossec-server -d -p 1514:1514/udp -p 1515:1515\
  -e SYSLOG_FORWADING_ENABLED=true \
  -v /var/ossec/data:/var/ossec/data xetusoss/ossec-server
 
 #-v /var/ossec/data:/var/ossec/data 可能会出错也可运行
 
  docker run --name ossec-server -d -p 1514:1514/udp -p 1515:1515\
  -e SYSLOG_FORWADING_ENABLED=true xetusoss/ossec-server
```

2）查看状态 docker ps

![image-20220503140644170](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6ml55h7j21gy0lkgox.jpg)

#### 2）ossec-agent

1）docker一键安装

```
docker run -idt intellafintech/ossec-agent-docker:4.4
```

2）查看状态 docker ps

![image-20220503141435554](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6uqdcogj21iw09kdi7.jpg)

## 3、ossec-server 修改配置

```
 ##查看容器id
 docker ps
 ##进入容器
 docker exec -it caa21a9b5004 bash 
 
 ##编辑 配置文件
 vim  /var/ossec/etc/ossec.conf
 
 添加数据库信息
 ##数据库信息
  <database_output>
        <hostname>124.233.192.250</hostname>
        <username>ossec</username>
        <password>ossec</password>
        <database>ossec</database>
        <type>mysql</type>
  </database_output>

#添加agent 信息
/var/ossec/bin/manage_agents
A #添加
名字
客户端ip
Y
E #查看ksy
002
key
MDI2IDE3Mi4xNy4wLjMgMTcyLjE3LjAuMyA2MmZjNjBiMGY1ZjA4Y2VlZmRkZjA3ZWJhOTMzM2RmNjBhZmU3MWZlM2RlN2M5ZDExZGY3OGZkZTZjOWZiMTQ1

#启动服务
/var/ossec/bin/ossec-control start

#查看日志检查是否报错
cat /var/ossec/logs/ossec.log
```



```
 #查看容器id
 docker ps
 #进入容器
 docker exec -it caa21a9b5004 bash 
```

![image-20220503140756180](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6nt7q0tj21i60fcgnu.jpg)

```
 #编辑 配置文件
 vim  /var/ossec/etc/ossec.conf
 
 添加数据库信息
 ##数据库信息
  <database_output>
        <hostname>124.233.192.250</hostname>
        <username>ossec</username>
        <password>ossec</password>
        <database>ossec</database>
        <type>mysql</type>
  </database_output>

```

![image-20220503140933970](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6pi8hxmj20rw0is40o.jpg)

```
#添加agent 信息
/var/ossec/bin/manage_agents
A #添加
名字
客户端ip
Y
E #查看ksy
002


key
MDI2IDE3Mi4xNy4wLjMgMTcyLjE3LjAuMyA2MmZjNjBiMGY1ZjA4Y2VlZmRkZjA3ZWJhOTMzM2RmNjBhZmU3MWZlM2RlN2M5ZDExZGY3OGZkZTZjOWZiMTQ1
```

![image-20220503141640058](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6wxii74j20u30u0juk.jpg)

![image-20220503141703178](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6xaxpelj21d40eydic.jpg)

```
#启动服务
/var/ossec/bin/ossec-control start
```

![image-20220503141802648](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6yc71frj212m0ci40h.jpg)

```
#查看日志检查是否报错
cat /var/ossec/logs/ossec.log
```

![image-20220503141902598](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v6zdzogcj21gw0ikah1.jpg)

## 4、ossec-agent 修改配置

```
#查看容器id
 docker ps
#进入容器
 docker exec -it caa21a9b5004 bash 
 
#编辑配置文件
vim  /var/ossec/etc/ossec.conf
#添加修改 服务器ip
<client>
    <server-ip>172.17.0.2</server-ip>
  </client>
  
#添加agent 信息
/var/ossec/bin/manage_agents
I
key
y

#启动服务
/var/ossec/bin/ossec-control start

#查看日志检查是否报错
cat /var/ossec/logs/ossec.log
```



```
 #查看容器id
 docker ps
 #进入容器
 docker exec -it caa21a9b5004 bash 
```

![image-20220503142255719](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v73f75v1j21i80c2q52.jpg)

```
#编辑配置文件
vim  /var/ossec/etc/ossec.conf
#添加修改 服务器ip
<client>
    <server-ip>172.17.0.2</server-ip>
  </client>
```

![image-20220503142403005](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v74krzqsj21590u076d.jpg)

```
#添加agent 信息
/var/ossec/bin/manage_agents
I
key
y
```

![image-20220503142531481](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v764rbnnj21hq0ms0vo.jpg)

```
#启动服务
/var/ossec/bin/ossec-control start
```

![image-20220503142558611](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v76l8xl8j20ss08gab5.jpg)

```
#查看日志检查是否报错
cat /var/ossec/logs/ossec.log
```

![image-20220503142623464](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v770u7vej21hq0ek794.jpg)

## 5、ossec-server 查看连接状态

```
#sever 端 查看连接
/var/ossec/bin/agent_control -l

#查看报警
cat /var/ossec/logs/alerts/alerts.log
```

![image-20220503142716372](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v77xkikyj20w20bwwgc.jpg)

![image-20220503143024443](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7b74ddkj20rs0fydhw.jpg)

## 6、web ui 安装

服务与sever端是分离的，web端只要能连接数据库就好

我在本机安装

```
wget https://github.com/ECSC/analogi/archive/master.zip
unzip master.zip
```

![image-20220503143222591](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7d8ukdpj20uo0nmjtv.jpg)

```
编辑db_ossec.php文件，修改MySQL的配置信息
```

![image-20220503143318101](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7e89yy8j21qw0rgn2b.jpg)

打开小皮面板进行配置

![image-20220503143404029](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7f2z87ij20mg0j2q4h.jpg)

选择php5.4

![image-20220503143435149](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7fn4vpej20nk0iyjsv.jpg)

访问网站

![image-20220503143503912](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7g202twj21gl0u0djx.jpg)

## 7、规则配置

以下规则都在agent端ossec.config 配置

![image-20220503144248358](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7o3jrjlj20bm0a8dgf.jpg)

#### 1）ossec 完整性检查说明

/var/ossec/etc/ossec.conf 配置

```
 <syscheck>
    <!-- Frequency that syscheck is executed - default to every 22 hours -->
    <frequency>10</frequency>
 
    <!-- Directories to check  (perform all possible verifications) -->
    <directories check_all="yes">C:\Users\hzl\Desktop\222</directories>
    <!-- Windows files to ignore -->
    <ignore>C:\WINDOWS/System32/LogFiles</ignore>
    
  </syscheck>

```

![image-20220503144307730](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7og2nazj20m20s8wk2.jpg)

##### 效果

修改目录中任意文件都会报警

![image-20220503144435989](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1v7pyn22tj21cy0u0td7.jpg)

![image-20220503170243276](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1vbpvyk8ej20on0ia0wg.jpg)

#### 
