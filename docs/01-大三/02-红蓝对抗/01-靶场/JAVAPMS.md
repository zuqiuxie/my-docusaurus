---
title:  javaPMS靶场复现
---



# 一、反序列化漏洞及利用

## 1、通过fscan 扫描发现反序列化漏洞 Shiro

![image-20220103144922280](https://tva1.sinaimg.cn/large/008i3skNly1gy0hhxs4daj30pf0680tb.jpg)

## 2、Shiro工具 key获取

![image-20220103144424321](https://tva1.sinaimg.cn/large/008i3skNly1gy0hcrl7u0j30wm0u0myo.jpg)

![image-20220103144446507](https://tva1.sinaimg.cn/large/008i3skNly1gy0hd5l2s9j312l0u0acl.jpg)

## 3、key 爆破后利用



![image-20220103144725322](https://tva1.sinaimg.cn/large/008i3skNly1gy0hfwv8v4j31080u0aft.jpg)

## 4、shell执行成功

![image-20220103144814008](https://tva1.sinaimg.cn/large/008i3skNly1gy0hgr9oz8j31080u0gp7.jpg)