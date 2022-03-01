# 经典漏洞的waf bypass

## 一、sql注入

### bypass

#### 1、大小写绕过

​	根据应用程序的过滤规则，通常会对恶意关键字设置黑名单，如果存在恶意关键字，应用程序就会退出运行。但在过滤规则中可能存在过滤不完整或只过滤小写或大小的情况，没有针对大小写组合进行过滤，导致可以通过大小写混写来绕过关键字过滤！

```
aNd 1=1 ......orDer ByseLeCt......
```

#### 2、双写关键字绕过

```
id =1 and 1=1加入and被过滤了，最后变成了id =1 1=1：可以这样构造：anandd ......id =1 anandd 1=1其他类似：id =1 order by 3，变成了 id =1 der by 3：oorrder......
```

#### 3、编码绕过

双重URL编码绕过

#### 4、URL编码

```
and%61%6e%64%25%36%31%25%36%65%25%36%34
```

#### 5、16进制编码

MySQL数据库可以识别16进制数据，会对其进行自动转换！

PHP配置开启了GPC，会自动对单引号进行转义，所以将注入的数据转换为16进制，就不需要使用单引号了

```
0x746f6e79 GPC后 --> 'tony'
```

#### 6、Unicode编码绕过

Unicode编码：

形式：“u”或者是“%u”加上4位16进制Unicode码值！

IIS中间件可以识别Unicode字符，当URL中存在Unicode字符时，IIS会自动进行转换！

```
假如对select关键字进行了过滤，可以对其中几个字母进行unicode编码：se%u006cect
```

#### 7、ASCII编码

ASCII编码：

char()函数可以将字符转换为ASCII码

#### 8、内联注释绕过

MySQL会执行/! .../中语句！

/! 50010/：50010表示5.00.10 MySQL版本号

当MySQL数据库的实际版本号大于内联注释中的版本号时，就会执行内联注释内的语句！

```
id=1 /*!and*/ 1=1
```

#### 9、空格过滤绕过

会将空格加入黑名单！可以使用制表符，换行符，括号、反引号，/**/等来代替空格！

```
select /**/1,2,database();
select1,2,database(); // 制表符
select1,2,database();
```

换行符，不可见字符，需要url编码！

```
select (1) from ...
```

#### 10、其他：

and可替换为&&

or可替换为||

=转换为like,greatest,between

#### 11、等价函数与命令：

hex()、bin() ==> ascii()
sleep() ==>benchmark()
concat_ws()==>group_concat()
mid()、substr() ==> substring()
@@user ==> user()
@@datadir ==> datadir()

### WAF

#### 1、采用sql语句预编译和绑定变量

pdo

mysqli

(语法结构已经生成了，参数传进来的都当做字符串字面值！)

但某些场景只能采用字符串拼接的方式，这时要严格检查参数的数据类型！

#### 2、使用安全函数

ESAPI.encoder().encodeForSQL(codec, name)
该函数会将 name 中包含的一些特殊字符进行编码，这样 sql 引擎就不会将name中的字符串当成sql命令来进行语法分析了！

intval() // 避免出现数字型注入漏洞
htmlspecialchars() // 字符转换为HTML实体mysql_real_escape_string() // 对字符串中的特殊字符进行转义addslashes() // 预定义字符之前添加反斜杠

#### 3、输入验证

白名单或黑名单过滤

#### 4、编码输出

#### 5、waf进行运行时保护

#### 6、服务器配置修复

#### 7、PHP配置：

```
magic_quotes_gpcmagic_quotes_sybase
```

## 二、xss

### bypass

#### 1、大小写和双写绕过

```
<sCRIpt>aLert(1)</sCRIPT>

<scscriptript>alert(1)</scscriptript>
```

#### 2、a标签

```
<a href=”javascript:onclick=alert(1)”>test</a>

<a href=javascript:alert(1)>test</a>
```

#### 3、src属性

```
<img src=x onerror=alert(1)>

<img/src=x onerror=alert(1)>

<video src=x onerror=alert(1)>

<audio src=x onerror=alert(1)>

<iframe src=”javascript:alert(1)”>
```

#### 4、利用事件绕过

```
<svg onload=alert(1)><body onload=alert(1)>

<select autofogus onfocus=alert(1)>

<textarea autofocus onfocus=alert(1)>

<video><source onerror="javascript:alert(1)">

<iframe onload=alert(1)>
```

### WAF

chrome浏览器自带防御,可拦截反射性XSS（HTML内容和属性），js和富文本的无法拦截，所以我们必须得自己做一些防御手段。

#### 1、HTML节点内容的防御

将用户输入的内容进行转义：

```arcade
var escapeHtml = function(str) {
    str = str.replace(/</g,'&lt;');
    str = str.replace(/</g,'&gt;');
    return str;
}
ctx.render('index', {comments, from: escapeHtml(ctx.query.from || '')});
```

![clipboard.png](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202271404250.png)

#### 2、HTML属性的防御

对空格，单引号，双引号进行转义

```arcade
var escapeHtmlProperty = function (str) {
    if(!str) return '';
    str = str.replace(/"/g,'&quto;');
    str = str.replace(/'/g,'&#39;');
    str = str.replace(/ /g,'&#32;');
    return str;
}
ctx.render('index', {posts, comments,
    from:ctx.query.from || '',
    avatarId:escapeHtmlProperty(ctx.query.avatarId || '')});
```

![clipboard.png](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202271404296.png)

#### 3、JavaScript的防御

对引号进行转义

```arcade
var escapeForJS = function(str){
        if(!str) return '';
        str = str.replace(/\\/g,'\\\\');
        str = str.replace(/"/g,'\\"');
        return str;
}
```

![clipboard.png](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202271404269.png)

#### 4、富文本的防御

富文本的情况非常的复杂，js可以藏在标签里，超链接url里，何种属性里。

```xml
<script>alert(1)</script>
<a href="javascript:alert(1)"></a>
<img src="abc" onerror="alert(1)"/>
```

所以我们不能过用上面的方法做简单的转义。因为情况实在太多了。

现在我们换个思路，
提供两种过滤的办法：

1）黑名单
我们可以把`<script/>` onerror 这种危险标签或者属性纳入黑名单，过滤掉它。但是我们想，这种方式你要考虑很多情况，你也有可能漏掉一些情况等。

2）白名单
这种方式只允许部分标签和属性。不在这个白名单中的，一律过滤掉它。但是这种方式编码有点麻烦，我们需要去解析html树状结构，然后进行过滤，把过滤后安全的html在输出。
大家可以看到：

![clipboard.png](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202271404285.png)

```
<script>不在白名单中，所以被过滤掉了。
```

#### 5、CSP(Content Security Policy)

内容安全策略（Content Security Policy，简称CSP）是一种以可信白名单作机制，来限制网站中是否可以包含某来源内容。默认配置下不允许执行内联代码（`<script>`块内容，内联事件，内联样式），以及禁止执行eval() , newFunction() , setTimeout([string], ...) 和setInterval([string], ...) 。

#### 示例：

1.只允许本站资源

```pgsql
Content-Security-Policy： default-src ‘self’
```

2.允许本站的资源以及任意位置的图片以及 [https://segmentfault.com](https://segmentfault.com/) 下的脚本。

```css
Content-Security-Policy： default-src ‘self’; img-src *;
script-src https://segmentfault.com
```

## 三、文件上传

### bypass

#### 1、JS绕过

抓包改名

![image-20211227202101180](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202271408261.jpeg)



#### 2、绕type类型

绕过 contnet-type 检测上传

有些上传模块，会对 http 的类型头进行检测，如果是图片类型，允许上传文件到服务器，否则返回上传失败。因为服务端是通过 content-type 判断类型，content-type 在客户端可被修改。则此文件上传也有可能被绕过的风险。

上传文件,脚本文件，抓包把 content-type 修改成 image/jpeg 即可绕过上传。

#### 3、绕黑名单

绕过黑名单上传

上传模块，有时候会写成黑名单限制，在上传文件的时获取后缀名，再把后缀名与程序中黑名单进行检测，如果后缀名在黑名单的列表内，文件将禁止文件上传。

上传图片时，如果提示不允许 php、asp 这种信息提示，可判断为黑名单限制，上传黑名单以外的后缀名即可。
 在 iis 里 asp 禁止上传了，可以上传 asa cer cdx 这些后缀，如在网站里允许.net 执行 可以上传 ashx 代替 aspx。如果网站可以执行这些脚本，通过上传后门即可获取 webshell。
 在不同的中间件中有特殊的情况，如果在 apache 可以开启 application/x-httpd-php 在 AddType application/x-httpd-php .php .phtml .php3 后缀名为 phtml 、php3 均被解析成 php 有的 apache 版本默认就会开启。上传目标中间件可支持的环境的语言脚本即可，如.phtml、php3。

####  4、htaccess 重写解析绕过

上传模块，黑名单过滤了所有的能执行的后缀名,如果允许上传.htaccess。htaccess 文件的作用是 可以帮我们实现包括：文件夹密码保护、用户自动重定向、自定义错误页面、改变你的文件扩展名、封禁特定IP 地址的用户、只允许特定 IP 地址的用户、禁止目录列表，以及使用其他文件作为 index 文件等一些功能。
 在 htaccess 里写入 SetHandler application/x-httpd-php 则可以文件重写成 php 文件。要 htaccess 的规则生效 则需要在 apache 开启 rewrite 重写模块，因为 apache 是多数都开启这个模块，所以规则一般都生效

上传.htaccess 到网站里.htaccess 内容是

```
<FilesMatch "jpg">
 SetHandler application/x-httpd-php
 </FilesMatch>
```

 再上传恶意的 jpg 到.htaccess 相同目录里，访问图片即可获取执行脚本。



#### 5、大小写绕过

有的上传模块 后缀名采用黑名单判断，但是没有对后缀名的大小写进行严格判断，导致可以更改后缀大小写可以被绕过。如 PHP、 Php、 phP、pHp

#### 6、空格绕过

在上传模块里，采用黑名单上传，如果没有对空格进行去掉可能被绕过。
 抓包上传，在后缀名后添加空格



#### 7、windows 系统特征

在 windows 中文件后缀名. 系统会自动忽略.所以 shell.php. 像 shell.php 的效果一样。所以可以在文件 名后面机上 . 绕过
 抓包修改在后缀名后加上 . 即可绕过。

#### 8、NTFS 交换数据流::$DATA

如果后缀名没有对::$DATA 进行判断，利用 windows 系统 NTFS 特征可以绕过上传。
 burpsuite 抓包，修改后缀名为 php::$DATA

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202271408812.jpeg)

 

#### 9、叠加特征绕过

在 windwos 中如果上传文件名 test.php:.jpg 的时候，会在目录下生产空白的文件名 test.php
 再利用 php 和 windows 环境的叠加属性，


 以下符号在正则匹配时相等
 双引号" 等于 点号. 大于符号> 等于 问号?
 小于符号< 等于 星号*
 文件名.<或文件名.<<<或文件名.>>>或文件名.>><空文件名

首先抓包上传 a.php:.php 上传会在目录里生成 a.php 空白文件，接着再a.php 改成 a.>>>

#### 10、双写后缀名绕过上传

在上传模块，有的代码会把黑名单的后缀名替换成空，例如 a.php 会把 php 替换成空，但是可以使用双写
 绕过例如 asaspp，pphphp，即可绕过上传
 抓包上传，把后缀名改成 pphphp 即可绕过

![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202271408014.jpeg)



#### 11、目录可控%00 截断绕过

使用白名单验证会相对比较安全，因为只允许指定的文件后缀名。但是如果有可控的参数目录，也存在被绕过的风险.
 代码中使用白名单限制上传的文件后缀名，只允许指定的图片格式。但是$_GET['save_path']服务器接受客户端的值，这个值可被客户端修改。所以会留下安全问题

上传参数可控
 当 gpc 关闭的情况下，可以用%00 对目录或者文件名进行截断。
 php 版本小于 5.3.4
 首先截断攻击，抓包上传将%00 自动截断后门内容。
 例如 1.php%00.1.jpg 变成 1.php

 

#### 12、二次渲染

有些图片上传，会对上传的图片进行二次渲染后在保存，体积可能会更小，图片会模糊一些，但是符合网站的需求。例如新闻图片封面等可能需要二次渲染，因为原图片占用的体积更大。访问的人数太多时候会占用，很大带宽。二次渲染后的图片内容会减少，如果里面包含后门代码，可能会被省略。导致上传的图片马，恶意代码被清除

首先判断图片是否允许上传 gif，gif 图片在二次渲染后，与原图片差别不会太大。所以二次渲染攻击最好用 git 图片马

 制作图片马
 将原图片上传，下载渲染后的图片进行对比，找相同处，覆盖字符串，填写一句话后门，或者恶意指令。
 原图片与渲染后的图片这个位置的字符串没有改变所在原图片这里替换成`<? php phpinfo();?>`直接上传即可

### WAF

### 系统运行时的防御：

1、文件上传的目录设置为不可执行。只要web容器无法解析该目录下面的文件，即使攻击者上传了脚本文件，服务器本身也不会受到影响，因此这一点至关重要。
2、判断文件类型。在判断文件类型时，可以结合使用MIME Type、后缀检查等方式。在文
件类型检查中，强烈推荐白名单方式，黑名单的方式已经无数次被证明是不可靠的。此外，对于图片的处理，可以使用压缩函数或者resize函数，在处理图片的同时破坏图片中可能包含的HTML代码。
3、使用随机数改写文件名和文件路径。文件上传如果要执行代码，则需要用户能够访问到这个文件。在某些环境中，用户能上传，但不能访问。如果应用了随机数改写了文件名和路径，将极大地增加攻击的成本。再来就是像shell.php.rar.rar和crossdomain.xml这种文件，都将因为重命名而无法攻击。
4、单独设置文件服务器的域名。由于浏览器同源策略的关系，一系列客户端攻击将失效，比如上传crossdomain.xml、上传包含Javascript的XSS利用等问题将得到解决。
5、使用安全设备防御。文件上传攻击的本质就是将恶意文件或者脚本上传到服务器，专业的安全设备防御此类漏洞主要是通过对漏洞的上传利用行为和恶意文件的上传过程进行检测。恶意文件千变万化，隐藏手法也不断推陈出新，对普通的系统管理员来说可以通过部署安全设备来帮助防御。

#### 系统开发阶段的防御

1、系统开发人员应有较强的安全意识，尤其是采用PHP语言开发系统。在系统开发阶段应充分考虑系统的安全性。
2、对文件上传漏洞来说，最好能在客户端和服务器端对用户上传的文件名和文件路径等项目分别进行严格的检查。客户端的检查虽然对技术较好的攻击者来说可以借助工具绕过，但是这也可以阻挡一些基本的试探。服务器端的检查最好使用白名单过滤的方法，这样能防止大小写等方式的绕过，同时还需对%00截断符进行检测，对HTTP包头的content-type也和上传文件的大小也需要进行检查。

#### 系统维护阶段的防御

1、系统上线后运维人员应有较强的安全意思，积极使用多个安全检测工具对系统进行安全扫描，及时发现潜在漏洞并修复。
2、定时查看系统日志，web服务器日志以发现入侵痕迹。定时关注系统所使用到的第三方插件的更新情况，如有新版本发布建议及时更新，如果第三方插件被爆有安全漏洞更应立即进行修补。
3、对于整个网站都是使用的开源代码或者使用网上的框架搭建的网站来说，尤其要注意漏洞的自查和软件版本及补丁的更新，上传功能非必选可以直接删除。除对系统自生的维护外，服务器应进行合理配置，非必选一般的目录都应去掉执行权限，上传目录可配置为只读。