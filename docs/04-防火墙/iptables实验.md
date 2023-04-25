# iptables实验

## 1、不允许本机屏别的主机。但允许别的主机屏本机

#### 配置

```
 
```



#### ![image-20220316152926244](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0br9u0fl5j20it0as0tn.jpg)配置前

本机可以`ping 144.114.114.114`

![image-20220316151012926](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bqpy6dkrj20do04h0t6.jpg)

#### 配置后

本机不能`ping 144.114.114.114`

![image-20220316152710793](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0br7hcmi9j20dy064q3g.jpg)

其他可以`ping 本机`

![image-20220316152750900](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0br86gbu9j20dx078q3y.jpg)



## 2、只允许 xxx ip ssh 连接本机

```
sudo iptables -A INPUT -p tcp --dport 22 -s 112.17.241.45 -j ACCEPT 
sudo iptables -A INPUT -p tcp --dport 22  -j DROP
```

![image-20220316154244706](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0brnny837j20i00bhq3x.jpg)

### 配置前

#### 任意ip都可以访问22端口的ssh



### 配置后

#### 只有112.17.241.45 可以连接22端口的ssh

![image-20220316154435820](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0brplg532j20eh0dgwfs.jpg)

#### 其他ip不可以

![image-20220316154644317](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0brrtnqxjj20gp020glq.jpg)

## 3、只能访问xx ip的8080端口

```
sudo iptables -A OUTPUT -p tcp --dport 8080 -d 124.223.192.250 -j ACCEPT 
sudo iptables -A OUTPUT -p tcp   -j DROP
```

![image-20220316160114276](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bs6wqy8zj20ix0andgz.jpg)

#### 配置后

可以访问指定ip的指定端口

![image-20220316160257169](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bs8ov9p4j20ny0jbaci.jpg)

其他都不能访问其他任何ip

![image-20220316160447088](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bsalhfbrj20bj040dfr.jpg)

![image-20220316160533887](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bsben561j207b01iwed.jpg)

## 4、禁止主机的ping功能，并禁止除xx ip外的ping请求

```
sudo iptables -A OUTPUT -p icmp --icmp-type 8 -s 0/0 -j DROP
sudo iptables -A INPUT -p icmp  --icmp-type 8 -s 124.223.192.250 -j DROP
```

#### 本机不能ping其他

![image-20220316162157685](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bssh18hbj20gs076js7.jpg)

#### 124.223.192.250 不能 ping本机

![image-20220316162233250](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bst2x93mj20ey05jgm1.jpg)

#### 其他可以ping本机

![image-20220316162255809](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bsth548cj20ey04x0t8.jpg)

## 5、只允许访问响应中有"admin"的http请求

```
sudo iptables -A INPUT -p tcp -m string --string "admin" --algo bm --sport 8088 -j ACCEPT 
sudo iptables -A INPUT -p tcp  --sport 8088 -j DROP
```

![image-20220316163804153](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24ely1h0bt987kzvj20ot0bf75e.jpg)

#### 开启http服务

```
python2 -m SimpleHTTPServer 8088
```

