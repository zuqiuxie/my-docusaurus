# snort安装

## 1、准备

![image-20220430145912113](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rra9qxfzj20v40hkgn1.jpg)

## 2、安装nmap

在安装nmap的时候会安装winpcap

![image-20220430145949245](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrazla0gj20dv0ax407.jpg)

### 3、打开snort/bin

```
cmd snor.exe -W 查看网卡
```

![image-20220430150038752](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrbpte6xj20vd0hvq3z.jpg)

![image-20220430150136169](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrcpr2g2j20q407q75i.jpg)

## 4、安装小皮面板 安装WNMP

![image-20220430150221770](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrdicj0kj20m60hddhq.jpg)

## 5、打开sqlmap目录 配置数据库

在小皮->设置->文件位置->mysql->mysql目录->bin->cmd

![image-20220430150406675](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrgwdjejj20m90hjgmi.jpg)

![image-20220430150423365](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrgugmp4j20vb0hkq5v.jpg)

之后是创建数据库，如下图所示，使用命令`create database snort`和`create database snort_archive`分别创建两个数据库

![在这里插入图片描述](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrhts1ucj20d502ajre.jpg)

运行sql脚本创建表

```
C:\phpstudy_pro\Extensions\MySQL5.7.26\bin>mysql -u root -p -D snort < C:\Users\hzl\Desktop\新建文件夹\snort-windows\Snort2.8.6\schemas\create_mysql
```

查看数据库

```
mysql.exe -u root -p
use snort
show tables;
```

![image-20220430150801459](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrjem3duj20er0b8mxi.jpg)

## 6、安装图形化界面

#### 1）修改acid配置文件 12行 将adodb位置放进去

![image-20220430151201265](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrnkisy7j20z00o8q8t.jpg)

#### 2）修改两个数据库文件配置

![image-20220430151349592](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rrpfxg2xj20h6093mya.jpg)

#### 3）添加jpgraph位置

![image-20220430151821824](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rru5wj08j20i50my422.jpg)

#### 4）小皮启动服务

拷贝文件到web目录

![image-20220430154201565](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rsisbca1j20nd07bjrt.jpg)

![image-20220430154130830](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rsi9598sj20gd0fawg7.jpg)

![image-20220430154108238](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rshv4mc3j21210nrdjw.jpg)

#### 5）配置snort

接下来要做的事就是配置snort，让snort运行起来，将数据写入到数据库中，然后我们的图形化界面才能读取数据库中的数据。如下图所示，我们需要找到snort的etc文件夹下的配置文件，可以看到snort.conf。
这里需要配置的地方有几个，如下图所示，是将rule和preproc_rules的路径添加到配置文件中。
1、rules 规则文件位置

![image-20220430155427460](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rsvq8jt1j20i805zab4.jpg)

2、output 到database 配置mysql 信息

![image-20220430155550765](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rsx61vdaj20r203g3zb.jpg)

3、一些配置信息文件位置

![image-20220430155621738](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rsxp9tb0j20ek02ft8x.jpg)

![image-20220430155634671](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rsxx6saxj20hi025t8u.jpg)

6）运行snort 并将结果保存在数据库中

在snort/bin 文件夹中运行，打开snort后的抓包如下图所示。这里-c是配置文件，-i是接口，-l是存放日志的位置。对了，这个-i 6是要通过查看自己的网卡，不一定是6，好像会变。这条语句：`snort -W`

```
snort -dev -c c:\snort\etc\snort.conf -i 2 -l c:/snort/log/
```

![image-20220430155917020](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rt0qv75jj20xt0fwwio.jpg)

运行成功有结果了

![image-20220430155939987](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rt14xnpuj20qo0e7tbr.jpg)

ping 8.8.8.8 进行测试

![image-20220430160140315](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rt38p2quj20qo0dxwfo.jpg)

可以看到日志文件如下图所示，生成的alert都会进入alert.ids文件中，而文件sfportscan.log则是对端口扫描的记录。

![image-20220430160028683](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rt1zgq7mj20uz0hjq4n.jpg)

可以看到snort.log中的内容如下图所示，可以看到时间，ip，端口等信息。

![image-20220430160053408](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rt2estjsj20ua0nun1w.jpg)

然后从acid中看下，可以看到已经看到了图形化界面。

![image-20220430160204151](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rt3mxwymj211z0l6q6o.jpg)

![image-20220430160312840](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rt4u01u0j211p0cmjtk.jpg)

## 7、base 安装

然后配置下base的图形化界面，进入base的目录下，看到base_conf.php文件。进入配置base_conf.php文件，将上述的位置都添加进去，并且把mysql的数据都写入进去。如下图，输入网址“127.0.0.1/base”就可以进入图形化界面。大致和acid一样。

1）访问127.0.0.1/base

![image-20220430160821246](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rta6gwrpj21050afjt0.jpg)

2）adodb 文件位置

![image-20220430160909831](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rtb0pp44j210e0e2gmw.jpg)

3）配置两个数据库信息

![image-20220430161025737](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rtcbq3zej20qn0j7jtl.jpg)

4）设置base账号密码

![image-20220430161102362](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rtcz2svej20qn0j7jtl.jpg)

5）初始化数据库

![image-20220430161140310](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rtdmhjhuj212h04ogm3.jpg)

6）成功

![image-20220430161156228](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1rtdw819hj211y0mvq7h.jpg)

## 8、添加规则

检测tcp协议 请求存在GET /QAX/idstest 报警

```
alert tcp any any -> any any (content: "GET /QAX/idstest"; sid:10000002; msg: "GET QAX"; )
alert tcp any any -> any any (content: "GET /docs/intro"; sid:10000002; msg: "GET QAX"; )
/docs/intro
```

![image-20220502104422028](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1tv5tnpr4j20l806974o.jpg)

![image-20220502104451829](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1tv690mc4j20sj0jxjx5.jpg)

![image-20220502104511282](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h1tv6l6r1sj20pf0otjw0.jpg)