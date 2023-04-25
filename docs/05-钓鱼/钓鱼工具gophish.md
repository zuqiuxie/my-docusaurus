# 钓鱼工具gophish

# 概念

Gophish项目地址：https://github.com/gophish/gophish
Gophish官网地址：https://getgophish.com/

Gophish：开源网络钓鱼工具包

Gophish是为企业和渗透测试人员设计的开源网络钓鱼工具包。 它提供了快速，轻松地设置和执行网络钓鱼攻击以及安全意识培训的能力。

> 以上摘自gophish的github项目简介



gophish自带web面板，对于邮件编辑、网站克隆、数据可视化、批量发送等功能的使用带来的巨大的便捷
在功能上实现分块，令钓鱼初学者能够更好理解钓鱼工作各部分的原理及运用

# 快速安装

安装包下载：https://github.com/gophish/gophish/releases

当前（2020.07.23）最新版本是 v0.10.1
gophish可以运行在常见的操作系统上，下面分别介绍两大主流系统Linux与Windows上的安装及运行

## Linux

根据自身操作系统的版本下载相应的安装包
gophish-v0.10.1-linux-32bit.zip
gophish-v0.10.1-linux-64bit.zip

使用wget命令下载到本地

```
//32bit
root@ubuntu:~$ wget https://github.com/gophish/gophish/releases/download/v0.10.1/gophish-v0.10.1-linux-32bit.zip
//64bit
root@ubuntu:~$ wget https://github.com/gophish/gophish/releases/download/v0.10.1/gophish-v0.10.1-linux-64bit.zip
```

### 这里以64位Ubuntu系统为例：

#### 下载完成后，利用unzip命令进行解压

```
root@ubuntu:~$ mkdir gophish
```

#### 解压至gophish文件夹中

```
root@ubuntu:~$ unzip gophish-v0.10.1-linux-64bit.zip -d ./gophish
```

#### 修改配置文件：

```
root@ubuntu:~$ cd gophish
```


若需要远程访问后台管理界面，将listen_url修改为0.0.0.0:3333，端口可自定义。（这项主要针对于部署在服务器上，因为一般的Linux服务器都不会安装可视化桌面，因此需要本地远程访问部署在服务器上的gophish后台）
如果仅通过本地访问，保持127.0.0.1:3333即可

```
root@ubuntu:~$ vi config.json

{
		//后台管理配置
        "admin_server": {
                "listen_url": "0.0.0.0:3333",  // 远程访问后台管理
                "use_tls": true,
                "cert_path": "gophish_admin.crt",
                "key_path": "gophish_admin.key"
        },
        "phish_server": {
                "listen_url": "0.0.0.0:80",
                "use_tls": false,
                "cert_path": "example.crt",
                "key_path": "example.key"
        },
        "db_name": "sqlite3",
        "db_path": "gophish.db",
        "migrations_prefix": "db/db_",
        "contact_address": "",
        "logging": {
                "filename": "",
                "level": ""
        }
}
```



### 运行gophish：

运行gophish脚本

```
root@ubuntu:~$ ./gophish
```

访问后台管理系统：
本地打开浏览器，访问https://ip:3333/ （注意使用https协议）
可能会提示证书不正确，依次点击 高级 — 继续转到页面 ，输入默认账密进行登录：账号admin，密码查看程序运行处

> 注：最新版本的gophsih(v0.11.0)删除了默认密码“ gophish”。取而代之的是，在首次启动Gophish时会随机生成一个初始密码并将其打印在终端中
> 详情见：https://github.com/gophish/gophish/releases/tag/v0.11.0

此处建议，如果是部署在公网服务器上，尽量登录后在功能面板Account Settings处修改高强度密码

> 如果依旧访问不到后台管理系统，有可能是服务器未对外开放3333端口，可查看防火墙策略、配置iptables 等方式自检

![image-20220810143425815](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51nrzkov6j21ij0u0wg8.jpg)

访问钓鱼界面：
本地打开浏览器，访问http://ip:80/
由于我们还未配置钓鱼页面，提示小段的404 page not found说明运行正常

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51ntn19otj20km05gmx8.jpg)

至此，已完成了Linux下gophish的安装以及相关配置，并且运行起来了。
另外我在实践中遇到过一个小问题，当仅仅使用命令./gophish来启动gophish时，经过一段时间会自动掉线（脚本终止运行）
所以需要长期保持运行时，可以结合nohup与&来启动gophish，保持后台稳定运行
具体命令：

```
root@ubuntu:~$ nohup ./gophish &
```




> 注：最新版本的gophsih(v0.11.0)删除了默认密码“ gophish”。取而代之的是，在首次启动Gophish时会随机生成一个初始密码并将其打印在终端中
> 详情见：https://github.com/gophish/gophish/releases/tag/v0.11.0

# 功能介绍

由于后台管理系统是不区分操作系统的，因此不区分Linux和Windows来介绍各功能

进入后台后，左边的栏目即代表各个功能，分别是**Dashboard仪表板** 、**Campaigns钓鱼事件** 、**Users & Groups用户和组** 、**Email Templates邮件模板** 、**Landing Pages钓鱼页面** 、**Sending Profiles发件策略**六大功能
由于实际使用中并不是按照该顺序来配置各个功能，因此下面通过实际使用顺序来详细介绍各功能的使用方法

## Sending Profiles 发件策略

Sending Profiles的主要作用是**将用来发送钓鱼邮件的邮箱配置到gophish**

![image-20220810145542440](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oe0yrt8j20u014on18.jpg)

### 点击New Profile新建一个策略，依次来填写各个字段

#### Name：

Name字段是为新建的发件策略进行命名，不会影响到钓鱼的实施，建议以发件邮箱为名字，例如如果使用qq邮箱来发送钓鱼邮件，则Name字段可以写xxxxxx@qq.com

#### Interface Type:

Interface Type 是接口类型，默认为SMTP类型且不可修改，因此需要发件邮箱开启SMTP服务

#### From:

From 是发件人，即钓鱼邮件所显示的发件人。（在实际使用中，一般需要进行近似域名伪造）这里为了容易理解，就暂时以qq邮箱为例，所以From字段可以写：test<xxxxxx@qq.com>

#### Host:

Host 是smtp服务器的地址，格式是smtp.example.com:25，例如qq邮箱的smtp服务器地址为smtp.qq.com

#### Username:

Username 是smtp服务认证的用户名，如果是qq邮箱，Username则是自己的qq邮箱号xxxx@qq.com

#### Password:

Password 是smtp服务认证的密码，例如qq邮箱，需要在登录qq邮箱后，点击 设置 - 账户 - 开启SMPT服务 生成授权码，Password的值则可以填收到的授权码

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oez1tsjj20uh08hgnw.jpg)

#### （可选）Email Headers:

Email Headers 是自定义邮件头字段，例如邮件头的X-Mailer字段，若不修改此字段的值，通过gophish发出的邮件，其邮件头的X-Mailer的值默认为gophish

设置好以上字段，可以点击Send Test Email来发送测试邮件，以检测smtp服务器是否认证通过

#### 成功收到测试邮件：

至此，发件邮箱的配置已经完成。当然，在实际钓鱼中，不可能使用自己的qq邮箱去发送钓鱼邮件。一是暴露自身身份，且邮件真实性低，二是qq邮箱这类第三方邮箱对每个用户每日发件数存在限制。
因此，如果需要大批量去发送钓鱼邮件，最好的方式是使用自己的服务器，申请近似域名，搭建邮件服务器来发件

## Landing Pages 钓鱼页面

完成钓鱼邮件的编写后，下一步则需要设计由邮件中超链接指向的钓鱼网页，点击New Page新建页面

### ![image-20220810145923804](/Users/oo/Library/Application Support/typora-user-images/image-20220810145923804.png)Name:

Name 是用于为当前新建的钓鱼页面命名，可以简单命名为钓鱼页面1

### Import Site:

与钓鱼邮件模板的编辑一样，gophish为钓鱼页面的设计也提供了两种方式，第一种则是Import Site
点击Import Site后，填写被伪造网站的URL，再点击Import，即可通过互联网自动抓取被伪造网站的前端代码
这里以伪造XX大学电子邮箱登录界面为例，在Import Site中填写https://mail.XX.edu.cn/cgi-bin/loginpage，并点击import

### 内容编辑框：

内容编辑框是编辑钓鱼页面的第二种方法，但是绝大多数情况下，它更偏向于用来辅助第一种方法，即对导入的页面进行源码修改以及预览。


由于编码的不同，通常直接通过Import Site导入的网站，其中文部分多少存在乱码现象，这时候就需要查看源码并手动修改过来
例如预览刚才导入的XX大学邮箱登录界面，就发现在多处存在乱码现象

### （重点）Capture Submitted Data:

通常，进行钓鱼的目的往往是捕获受害用户的用户名及密码，因此，在点击Save Page之前，记得一定要勾选**Capture Submitted Data**

当勾选了Capture Submitted Data后，页面会多出一个**Capture Passwords**的选项，显然是捕获密码。通常，可以选择勾选上以验证账号的可用性。如果仅仅是测试并统计受害用户是否提交数据而不泄露账号隐私，则可以不用勾选
另外，当勾选了**Capture Submitted Data**后，页面还会多出一个Redirect to，其作用是当受害用户点击提交表单后，将页面**重定向**到指定的URL。可以填写被伪造网站的URL，营造出一种受害用户第一次填写账号密码填错的感觉
（一般来说，当一个登录页面提交的表单数据与数据库中不一致时，登录页面的URL会被添加上一个出错参数，以提示用户账号或密码出错，所以在Redirect to中，最好填写带出错参数的URL）
因此，令此处的Redirect to的值为https://mail.XX.edu.cn/cgi-bin/loginpage?errtype=1

填写好以上参数，点击Save Page，即可保存编辑好的钓鱼页面

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oj2hccjj20l3092gm4.jpg)

### 踩坑汇总

> #### （必看）经验之谈 · 注意事项
>
> **在导入真实网站来作为钓鱼页面时，绝大多数情况下并非仅通过Import就能够达到理想下的克隆，通过多次实践，总结出以下几点注意事项**
>
> ```
> 【捕获不到提交的数据】导入后要在HTML编辑框的非Source模式下观察源码解析情况，如果明显发现存在许多地方未加载，则有可能导入的源码并非页面完全加载后的前端代码，而是一个半成品，需要通过浏览器二次解析，渲染未加载的DOM。这种情况下，除非能够直接爬取页面完全加载后的前端代码，否则无法利用gophish进行钓鱼，造成的原因是不满足第2点。
> 【捕获不到提交的数据】导入的前端源码，必须存在严格存在`<form method="post" ···><input name="aaa" ··· /> ··· <input type="submit" ··· /></form>`结构，即表单（POST方式）— Input标签（具有name属性）Input标签（submit类型）— 表单闭合的结构，如果不满足则无法捕获到提交的数据
> 【捕获不到提交的数据】在满足第2点的结构的情况下，还需要求`<form method="post" ···>`在浏览器解析渲染后（即预览情况下）不能包含`action`属性，或者`action`属性的值为空。否则将会把表单数据提交给action指定的页面，而导致无法被捕获到
> 【捕获数据不齐全】对于需要被捕获的表单数据，除了input标签需要被包含在`<form>`中，还需满足该<input>存在name属性。例如`<input name="username">`,否则会因为没有字段名而导致value被忽略
> 【密码被加密】针对https页面的import，通常密码会进行加密处理，这时需要通过审计导入的前端代码，找到加密的JavaScript函数（多数情况存在于单独的js文件中，通过src引入），将其在gophish的HTML编辑框中删除，阻止表单数据被加密
> 以上5点是在实践中总结出来的宝贵经验，或许还有其他许多坑未填，但所有的坑通常都围绕在`<form><input /></form>`结构中，所以如果遇到新坑，先将该结构排查一遍，还是不行，再另辟蹊径
> ```
>
> 



## Email Templates 钓鱼邮件模板

完成了邮箱配置之后，就可以使用gophish发送邮件了。所以，接下来需要去编写钓鱼邮件的内容
点击New Template新建钓鱼邮件模板，依次介绍填写各个字段

### Name:

同样的，这个字段是对当前新建的钓鱼邮件模板进行命名。可以简单的命名为：邮件模板1

### Import Email:

![image-20220810150713065](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oq0b1l3j20vg0m6758.jpg)

gophish为编辑邮件内容提供了两种方式，第一种就是Import Email

用户可以先在自己的邮箱系统中设计好钓鱼邮件，然后发送给自己或其他伙伴，收到设计好的邮件后，打开并选择导出为eml文件或者显示邮件原文，然后将内容复制到gophish的Import Email中，即可将设计好的钓鱼邮件导入

需要注意，在点击Import之前需要勾选上`Change Links to Point to Landing Page`，该功能实现了当创建钓鱼事件后，会将邮件中的超链接自动转变为钓鱼网站的URL

### Subject:

Subject 是邮件的主题，通常为了提高邮件的真实性，需要自己去编造一个吸引人的主题。这里简单填写成“第一次钓鱼测试”

### 内容编辑框：

内容编辑框是编写邮件内容的第二种模式，内容编辑框提供了Text和HTML两种模式来编写邮件内容，使用方式与正常的编辑器无异。
其中HTML模式下的预览功能比较常用到，在编辑好内容后，点击预览，就可以清晰看到邮件呈现的具体内容以及格式

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oqiobj1j20kq0b2wet.jpg)

### Add Tracking Image:

Add Tracking Image 是在钓鱼邮件末添加一个跟踪图像，用来跟踪受害用户是否打开了收到的钓鱼邮件。默认情况下是勾选的，如果不勾选就无法跟踪到受害用户是否打开了钓鱼邮件
（注：跟踪受害用户是否点击钓鱼链接以及捕捉提交数据不受其影响）

### Add Files:

Add Files 是在发送的邮件中添加附件，一是可以添加相关文件提高邮件真实性，二是可以配合免杀木马诱导受害用户下载并打开

当填写完以上字段后，点击Save Template，就能保存当前编辑好的钓鱼邮件模板

## Users & Groups 用户和组

当完成上面三个功能的内容编辑，钓鱼准备工作就已经完成了80%，Users & Groups 的作用是将钓鱼的目标邮箱导入gophish中准备发送
点击New Group新建一个钓鱼的目标用户组

### Name:

Name 是为当前新建的用户组命名，这里简单命名为目标1组

### Bulk Import Users:

Bulk Import Users是批量导入用户邮箱，它通过上传符合特定模板的CSV文件来批量导入目标用户邮箱
点击旁边灰色字体的Download CSV Template可以下载特定的CSV模板文件。其中，模板文件的Email是必填项，其余的Frist Name 、Last Name、Position可选填

### Add:

除了批量导入目标用户的邮箱，gophish也提供了单个邮箱的导入方法，这对于开始钓鱼前，钓鱼组内部测试十分方便，不需要繁琐的文件上传，直接填写Email即可，同样其余的Frist Name 、Last Name、Position可选填

编辑好目标用户的邮箱后，点击Save Changes即可保存编辑好的目标邮箱保存在gophish中

## Campaigns 钓鱼事件

Campaigns 的作用是将上述四个功能Sending Profiles 、Email Templates 、Landing Pages 、Users & Groups联系起来，并创建钓鱼事件
在Campaigns中，可以新建钓鱼事件，并选择编辑好的钓鱼邮件模板，钓鱼页面，通过配置好的发件邮箱，将钓鱼邮件发送给目标用户组内的所有用户

![image-20220810151204555](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51ov2nl8lj20u00wumzu.jpg)

### 点击New Campaign新建一个钓鱼事件

### Name:

Name 是为新建的钓鱼事件进行命名，这里简单命名为第一次钓鱼

### Email Template:

Email Template 即钓鱼邮件模板，这里选择刚刚上面编辑好的钓鱼邮件模板邮件模板1

### Landing Page:

Landing Page 即钓鱼页面，这里选择刚刚上面编辑好的名为钓鱼页面1的XX大学邮箱登录页面的钓鱼页面

### （重点）URL:

URL 是用来替换选定钓鱼邮件模板中超链接的值，该值指向部署了选定钓鱼页面的url网址（这里比较绕，下面具体解释一下，看完解释再来理解这句话）

简单来说，这里的URL需要填写当前运行gophish脚本主机的ip。
因为启动gophish后，gophish默认监听了3333和80端口，其中3333端口是后台管理系统，而80端口就是用来部署钓鱼页面的。
当URL填写了http://主机IP/，并成功创建了当前的钓鱼事件后。gophish会在主机的80端口部署当前钓鱼事件所选定的钓鱼页面，并在发送的钓鱼邮件里，将其中所有的超链接都替换成部署在80端口的钓鱼页面的url

所以，这里的URL填写我本地当前运行gophish主机的IP对应的url，即http://192.168.141.1/

另外，需要保证的是该URL对于目标用户组的网络环境是可达的。例如填写内网IP，则该钓鱼事件仅能够被内网目标用户所参与，而外网目标用户网络不可达。如果部署了gophish的是公网服务器，URL填写公网IP或域名，则需要保证目标用户的内网环境能够访问该公网服务器的IP（有些企业的内网环境会设定防火墙策略，限制内网用户能够访问的公网IP）

### Launch Date:

Launch Date 即钓鱼事件的实施日期，通常如果仅发送少量的邮箱，该项不需要修改。如果需要发送大量的邮箱，则配合旁边的Send Emails By效果更佳

### （可选）Send Emails By:

Send Emails By 配合Launch Date使用，可以理解为当前钓鱼事件下所有钓鱼邮件发送完成的时间。Launch Date作为起始发件时间，Send Emails By 作为完成发件时间，而它们之间的时间将被所有邮件以分钟为单位平分。

例如，Launch Date的值为2020.07.22，09:00，Send Emails By的值为2020.07.22，09:04，该钓鱼事件需要发送50封钓鱼邮件。
那么经过以上设定，从9:00到9:04共有5个发件点，这5个发件点被50封邮件平分，即每个发件点将发送10封，也就是每分钟仅发送10封。

这样的好处在于，当需要发送大量的钓鱼邮件，而发件邮箱服务器并未限制每分钟的发件数，那么通过该设定可以限制钓鱼邮件不受约束的发出，从而防止因短时间大量邮件抵达目标邮箱而导致的垃圾邮件检测，甚至发件邮箱服务器IP被目标邮箱服务器封禁
Sending Profile:
Sending Profile 即发件策略，这里选择刚刚编辑好的名为XXXXX@qq.com的发件策略

### Groups:

Groups 即接收钓鱼邮件的目标用户组，这里选择刚刚编辑好的名为目标1组的目标用户组

填写完以上字段，点击Launch Campaign后将会创建本次钓鱼事件（注意：如果未修改Launch Date，则默认在创建钓鱼事件后就立即开始发送钓鱼邮件）

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51ow1j243j217m05odg9.jpg)

## Dashboard 仪表板

当创建了钓鱼事件后，Dashboard 会自动开始统计数据。统计的数据项包括邮件发送成功的数量及比率，邮件被打开的数量及比率，钓鱼链接被点击的数量及比率，账密数据被提交的数量和比率，以及收到电子邮件报告的数量和比率。另外，还有时间轴记录了每个行为发生的时间点

> 关于电子邮件报告，详情参考：
> https://docs.getgophish.com/user-guide/documentation/email-reporting

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51owkvmaej21600d7t9j.jpg)

需要注意的是，Dashboard统计的是所有钓鱼事件的数据，而非单个钓鱼事件的数据，如果仅需要查看单个钓鱼事件的统计数据，可以在Campaigns中找到该钓鱼事件，点击View Results按钮查看

# Results for 第一次钓鱼：

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oybv7xuj214o0hrabq.jpg)


至此，一次在gophish发起的钓鱼事件所需实施步骤就已经全部完成，接下来就等着鱼儿上钩

### 查看捕获的数据

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oykwnxij20f608sdgn.jpg)

模拟目标用户的行为，打开上述发送的钓鱼邮件

点击超链接，跳转到部署好的钓鱼页面，发现与真实的XX大学邮箱登录界面无差别。（乱码问题以通过人工替换解决）观察网站的URL，可以看到钓鱼邮件中的超链接指向的就是之前在新建Campaigns的表单中填写的URL，只不过后面多了一个rid参数。
这里的?rid=csF1qTj具有唯一性，即唯一指向打开的这封钓鱼邮件，换句话说csF1qTj是为这封邮件的收件人唯一分配的。
如果此次名为第一次钓鱼的Campaigns选择的目标存在多个目标邮箱，则gophish会为每一封目标邮件分配一个唯一的rid值，以此来区别不同的收件人

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oyzyx99j21hc0smwll.jpg)

输入用户名密码，test/test并点击提交

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51oz4aqdfj20jc0igac5.jpg)

点击提交后，部署的钓鱼页面重定向到了真实的XX大学邮箱系统页面，且添加上了出错参数errtype=1，使账号或密码错误的提示显示出来，达到迷惑受害用户的作用

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51ozeefw7j21hc0sm7bg.jpg)

在gophish中查看捕获的数据 依次点击 Campaigns - 选择第一次钓鱼的View Results按钮 - 展开Details - 展开Submitted Data
可以看到在钓鱼页面提交的账密信息

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51ozhwz9gj20ld09bdgd.jpg)

以上即gophish功能面板中所有功能的介绍，通过以上功能的学习和利用，已经可以实施较为基础的网络钓鱼了。而Sending Profiles发件策略 — Landing Pages钓鱼页面 — Email Templates邮件模板 — Users & Groups用户和组 — Campaigns钓鱼事件 — Dashboard仪表板的顺序也是在实际使用中需要按照的顺序，如果前一步不能配置好，将会影响到后续的功能达不到想要的效果。因此，严格遵循该步骤不说可以事半功倍，至少能避免在成为“捕鱼人”的道路上迷踪失路。

# 实际案例

熟悉了上面的各项功能，虽然可以进行简单的钓鱼，但对于仿真度、可信度要求较高的网络钓鱼还是不够的。但凡具有一定网络安全意识的人一般情况下都不会上钩，所以下面采用一个实际的案例来看看一个常规的钓鱼还需要额外做哪些工作。

事情是这样的，一天，客户要求对公司的人员进行一次为期7天的网络钓鱼，来提高员工的网络安全意识，避免在真正的HW行动中因为钓鱼而使系统被攻陷。所以，一场漫长的网络钓鱼就开始在偷偷被谋划准备中了···

> 因为涉及公司信息，故以文字和少量图片来叙述。主要是学习一些方式方法，并在实际需要中提供一些思路
> 本文提到的所有方式方法请在得到授权的前提下进行，并确保仅以提高员工安全意识为目的
> 再次提醒，一切未经授权的网络攻击均为违法行为，互联网非法外之地

## 前期准备

近似域名：
搜寻了一番，我们锁定了客户公司具有登录功能的门户网站，打算通过它来钓员工的OA门户账号密码。为了提高可信度，我们根据门户网站的域名abclmn.com申请了近似域名abcimn.com，因为i的大写I，与门户网站的l(L)在某些字体下看起来是没有区别的，所以能够达到一定的仿真效果

## 搭建邮箱服务器：

有了近似域名后，我们将域名映射到服务器上。然后在服务器上利用postfix+dovecot搭建了邮箱服务，因为只需要发信，所以仅开启了SMTP服务。这样，我们就能够以xxx@abcI(i)mn.com为发件人来发邮件了，当然客户公司的邮箱系统账号是xxx@abcl(L)mn.com的形式，因此可以达到以假乱真的效果。

## 选择邮主题：

经过一番信息收集，我们找到了一则通知是关于客户公司近期正在推广新的信息公示系统，并附上了信息公示系统的URL（无需身份认证），所以可以利用这一点编辑一封主题是关于推广新信息公示系统的钓鱼邮件，诱导公司员工先通过我们的钓鱼页面登录OA门户，再通过重定向，定位到新的信息公示系统。这样，既能捕获到员工的OA账号密码，又能够有效降低可疑度，形成一个闭环。

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51p067tqaj21bj0h5ach.jpg)

## 确定发件人：

准备的差不多后，客户发来了一份员工及部门的邮箱列表，通过筛选，结合公司情况，我们确定了一个技术部的邮箱，并伪造成"技术部<jishubu@abcI(i)mn.com>"来发送钓鱼邮件，保证了钓鱼邮件的权威性和可信度

## 配置gophish

将门户页面、钓鱼邮件模板、目标用户邮箱导入到gophisih中保存，并在Sending Profiles配置好我们搭建的邮箱服务使其可以正常发件并不被丢进垃圾箱、过滤箱等。
配置完成后，创建钓鱼事件，因为发送量较大，需要设定Send Emails By 来限制每分钟的发信量，我们设定了10封/分钟的速度来发送，避免被识别成垃圾邮件。
完成所有字段填写后启动钓鱼事件，就开始缓缓地发送钓鱼邮件了···

## 查看钓鱼结果

虽然上钩的只有46个用户，占发件数量的2%，但是在真实场景下，哪怕仅有1个真实用户上钩，对于系统来说就足够造成巨大的威胁，这也是为什么社会工程能够成为一项重要攻击手段的原因

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h51p0clx78j21690h2gn7.jpg)

