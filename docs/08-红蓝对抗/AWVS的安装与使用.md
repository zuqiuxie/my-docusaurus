# AWVS 破解版安装

## Docker

#### bash

```bash
#  pull 拉取下载镜像
docker pull secfa/docker-awvs

#  将Docker的3443端口映射到物理机的 13443端口
docker run -it -d -p 13443:3443 secfa/docker-awvs

# 容器的相关信息
awvs13 username: admin@admin.com
awvs13 password: Admin123
AWVS版本：13.0.200217097
```

浏览器访问：[https://127.0.0.1:13443/ ](https://127.0.0.1:13443/#/about)即可



![img](https://image.3001.net/images/20200413/15867487723520.png)



## macOS 导入 AWVS 证书

当我们兴高采烈地在虚拟机中安装好 AWVS 的时候，然后 macOS 的 Chrome 浏览器去访问的时候居然碰到了下面的情况，居然还没有任然继续访问的选项！！！黑人问号？？？：



![img](https://image.3001.net/images/20200412/15866954417622.png)



然而在 Windows 的 Chrome 浏览器下是有`继续前往`的选项的。

不要慌问题不大，下面是解决方法：

首先使用 Safari 浏览器打开 awvs，看到如下信息后 点击「`显示详细信息`」



![img](https://image.3001.net/images/20200412/15866989588334.png)



接着点击「`访问此网站`」，此时Safari浏览器已经可以访问了，与此同时「`钥匙串访问`」中新增了刚刚自签名的 AWVS13 证书：



![img](https://image.3001.net/images/20200412/15866992434620.png)



鼠标选中证书，「`右键`」-「`显示简介`」-「`信任`」选择「`始终信任`」:



![img](https://image.3001.net/images/20200412/15866999783636.png)