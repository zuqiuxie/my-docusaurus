## Web应用服务器是如何处理HTTP请求的

### 静态

![image-20220314162058668](https://tva1.sinaimg.cn/large/e6c9d24ely1h09hiv7alyj20rl0br0tw.jpg)

![image-20220314162113921](https://tva1.sinaimg.cn/large/e6c9d24ely1h09hj4laenj20vj098750.jpg)

## URL编码

![image-20220314162257603](https://tva1.sinaimg.cn/large/e6c9d24egy1h09hkxpmhwj20qq0cqgm8.jpg)

%3d   =
%23   #
%25   %
%26   &
%2f   /
%20   空格
%0a   新行
%00   空字节

## HTTP

### 请求

#### 构成

请求报文由请求行、请求头部、空行和请求数据4个部分组成

![image-20220314160137081](https://tva1.sinaimg.cn/large/e6c9d24ely1h09gysfizhj20zk0dutaq.jpg)

#### 请求方法

GET、POST、HEAD、PUT、DELETE、OPTIONS、TRACE、CONNECT

#### 请求头部

1. Host：请求的主机名，允许多个域名同处一个IP地址，即虚拟主机。
2. User-Agent：产生请求的浏览器类型。
3. Accept：客户端可识别的内容类型列表。
4. Accept-Encoding：请求报头域类似于Accept，但是它是用于指定可接受的内容编码。
5. Accept-Language：请求报头域类似于Accept，但是它是用于指定一种自然语言。
6. Referer：上一个资源的URL。
7. Connection：当值为Close时，告诉服务器发送响应的文件后关闭连接，为Keep-Alive时，告诉服务器在完成本次请求的响应后，保持连接

#### 请求报文分析

```
GET /js/2345 HTTP/1.1\r\n              {请求使用了相对的URL}
Accept: */*\r\n                        {用户希望得到的文档的类型}
Referer: http://www.2345.com/?lb\r\n   {上一个网页的URL}
Accept-Language: ZH-CN\r\n             {表示用户优先得到中文版的文档}
Accept-Encoding: gzip\r\n              {允许接受压缩过的数据}
User-Agent: Mozilla/4.0 \r\n           {用户使用的代理服务器}
Host: union2.50bang.org\r\n            {给出主机的域名}
Connection: Keep-Alive\r\n             {告诉服务器保持持久连接}
\r\n                                   {CRLF}

```

### 响应

#### 构成

HTTP响应报文由状态行、消息报头、空行、响应正文4个部分组成

### 常见状态码：

200   OK                           //客户端请求成功。
304   Not Modified                 //直接使用本地缓存
400   Bad Request                  //客户端请求有语法错误，不能被服务器所理解。
401   Unauthorized                 //请求未经授权
403   Forbidden                    //服务器收到请求，但是拒绝提供服务
404   Not Found                    //请求资源不存在
500   Internal Server Error        //服务器发生不可预期的错误
503   Server Unavailable           //服务器当前不能处理客户端的请求。

### Cookie、Session存在的意义？

关系？区别？

作用范围不同，Cookie 保存在客户端（浏览器），Session 保存在服务器端。. 
存取方式的不同，Cookie 只能保存 ASCII，Session 可以存任意数据类型，一般情况下我们可以在 Session 中保持一些常用变量信息，比如说 UserId 等。
有效期不同，Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能，Session 一般失效时间较短，客户端关闭或者 Session 超时都会失效。

## 数据库类型

![image-20220314164522780](https://tva1.sinaimg.cn/large/e6c9d24ely1h09i896rryj211e0hmdk6.jpg)

## 信息收集

### 主动

Subdomain3

### 被动

https://rapiddns.io/
https://ti.qianxin.com/

### 证书收集

https://crt.sh/

### 服务器信息收集

Whois查询
站长之家http://whois.chinaz.com
阿里云域名信息查询https://whois.aliyun.com
Kali whois命令
Python-whois模块查询域名信息
企业信息查询网站：https://www.tianyancha.com天眼查
https://www.qichacha.com/企查查
https://beian.miit.gov.cn/#/Integrated/recordQuery备案查询
http://www.beian.gov.cn/portal/recordQuery公安部备案查询