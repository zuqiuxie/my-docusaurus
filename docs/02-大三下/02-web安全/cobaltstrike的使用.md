# cobaltstrike的使用

## 功能介绍：

![image-20220320200308694](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320200308.png)

```
Perferences：设置Cobal Strike界面、控制台、以及输出报告样式、TeamServer连接记录
Visualization：选择视图的显示方式，有三种，不好解释，亲自用了就会知道了
VPN Interfaces:设置vpn接口
LIsteners:设置监听。最重要的功能，里面有很多监听脚本
```

![image-20220320200336699](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320200336.png)

```
Applications -> 获取浏览器版本信息
Credentials -> 凭证当通过hashdump或者Mimikatz抓取过的密码都会储存在这里。
Downloads -> 下载文件Event Log -> 主机上线记录以及团队协作聊天记录
Keystrokes -> 键盘记录
Proxy Pivots -> 代理模块
Screenshots -> 进程截图
Script Console -> 控制台 更多脚本https://github.com/rsmudge/cortana-scripts
Targets -> 显示目标
Web Log -> Web访问记录
```

![image-20220320200426299](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320200426.png)

```
HTML Application      生成恶意的HTA木马文件；
MS Office Macro       生成office宏病毒文件；
Payload Generator     生成各种语言版本的payload;
USB/CD AutoPlay       生成利用自动播放运行的木马文件；
Windows Dropper       捆绑器，能够对文档类进行捆绑；
Windows Executable    生成可执行exe木马；
Windows Executable(S) 生成无状态的可执行exe木马。
```

![image-20220320201751582](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320201751.png)

```
Web Drive-by（钓鱼攻击）
Manage                     对开启的web服务进行管理；
Clone Site                 克隆网站，可以记录受害者提交的数据；
Host File                  提供一个文件下载，可以修改Mime信息；
PowerShell Web Delivery    类似于msf 的web_delivery ;
Signed Applet Attack       使用java自签名的程序进行钓鱼攻击;
Smart Applet Attack        自动检测java版本并进行攻击，针对Java 1.6.0_45以下以及Java 1.7.0_21以下版本；
System Profiler            用来获取一些系统信息，比如系统版本，Flash版本，浏览器版本等。
Spear Phish                是用来邮件钓鱼的模块。
```









## 1、连接

服务器打开cobaltstrike：

```
chmod +x teamserver
./teamserver 124.221.143.90 baobao.     //./teamserver ip 密码
```

![image-20220320172435618](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320172435.png)

打开cobaltstrike.exe:

![image-20220320174708480](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320174708.png)

连接成功后就可以利用了

## 2、对内交流：

![image-20220320192939824](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320192939.png)

## 3、监听

![image-20220320193440127](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320193440.png)

![image-20220320193525963](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320193526.png)

## 4、payload生成

attacks–>Web Drive-by–>Scripted Web Delivery

![image-20220320193810589](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320193836.png)

![image-20220320193927934](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320193927.png)

让目标机执行生成的payload就可以进一步利用了

![image-20220320194637882](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320194637.png)

![image-20220320194719382](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320194719.png)

## 5、命令执行

![image-20220320195351094](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320195447.png)

## 6、木马程序生成

attacks->packages->Windows Executable

![image-20220320222328284](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320222328.png)

生成的木马文件放到被攻击机上，运行后就可监听到

## 7、屏幕截图

```
screenshot    //运行屏幕截屏命令
```

然后打开View->Screenshots，则可以看到屏幕截图

![image-20220320222820415](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320222831.png)

## 8、键盘记录获取

```
ps          //查看系统进程，随便选择一个程序的进程PID  
keylogger 2640  //键盘记录注入进程
```

## 9、Powershell生成

attacks->packages->HTML Application

![image-20220320223450042](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320223526.png)

![image-20220320224001715](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320224001.png)

![image-20220320224146021](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320224149.png)

![image-20220320224214528](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320224214.png)

![image-20220320224254331](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320224254.png)

![image-20220320224330366](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320224330.png)

![image-20220320224442888](https://bao-1309906221.cos.ap-nanjing.myqcloud.com/20220320224442.png)