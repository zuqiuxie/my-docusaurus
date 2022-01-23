# 靶场搭建笔记

### 一、小皮面板安装

```
#安装
yum install -y wget && wget -O install.sh https://notdocker.xp.cn/install.sh && sh install.sh
#运行
sudo xp
```

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gxsfvafdmbj306q04rglo.jpg" alt="image-20211227154829039"  />

![image-20211227155849879](https://tva1.sinaimg.cn/large/008i3skNly1gxsg620hv0j32220nk43a.jpg)

### 二、spli-labs的配置

> 解压sqli-labs-master压缩文件，并复制到phpStudy根目录下（phpStudy\WWW），为了方便之后联系，可以修改文件夹的名字（sqli）。

![image-20211227155242808](https://tva1.sinaimg.cn/large/008i3skNly1gxsfzom7q1j30bd0emglr.jpg)



> 在sqli文件夹中打开sql-connections/db-creds.inc，修改数据库的密码后保存。

![image-20211227155538550](https://tva1.sinaimg.cn/large/008i3skNly1gxsg2q2we8j308l011t8i.jpg)

![image-20211227155528480](https://tva1.sinaimg.cn/large/008i3skNly1gxsg2k863vj30cq06njrl.jpg)

> 此时sqli-labs的配置就完成了，输入网址（http://172.20.10.3/sqli/），点击[Setup/reset Database for labs](http://127.0.0.1/sqli/sql-connections/setup-db.php)跳转到如下页面就表示已经完成了初始化安装

![image-20211227155946453](https://tva1.sinaimg.cn/large/008i3skNly1gxsg7113v1j30bv09tq3m.jpg)

### 三、upload-labs的配置

> 解压upload-labs-0.1压缩文件，并复制到phpStudy根目录下（phpStudy\WWW），为了方便之后联系，可以修改文件夹的名字（upload）。

![image-20211227160331273](https://tva1.sinaimg.cn/large/008i3skNly1gxsgaxpct2j30i508aq43.jpg)

> 此时upload-labs的配置就完成了，输入网址（http://127.0.0.1:7577/），跳转到如下页面就表示已经完成了初始化安装。

![image-20211227160507332](https://tva1.sinaimg.cn/large/008i3skNly1gxsgcm19tfj31240m377e.jpg)

### 四、 pikachu的配置

> 解压pikachu-master压缩文件，并复制到phpStudy根目录下（/web/pikachu-master）

![image-20211227160701202](https://tva1.sinaimg.cn/large/008i3skNly1gxsgekk8jfj30ja09c3z9.jpg)

> 在pikachu文件夹中找到打开inc/config.inc.php，修改数据库的密码后保存。
>
> 此时pikachu的配置就完成了，输入网址（http://127.0.0.1:7575/），页面最上方会出面一个红色的热情提示"欢迎使用,pikachu还没有初始化，点击进行初始化安装!",点击即可完成安装啦！

![image-20211227160758630](https://tva1.sinaimg.cn/large/008i3skNly1gxsgfkm2khj30mn0goabv.jpg)



# TOP10漏洞整理

![img](https://tva1.sinaimg.cn/large/008i3skNly1gxsghisn9fj30ss0f0t9r.jpg)

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