# shiro反序列化漏洞源码分析

## 实验步骤

#### （1） 下载shiro 源码并切换版本为1.2.4：

git clone https://github.com/apache/shiro.git

cd shiro

git checkout shiro-root-1.2.4

![image-20220402095736879](https://tva1.sinaimg.cn/large/e6c9d24egy1h0v57smopej20j80c875z.jpg) 

#### （2） 修改shiro/samples/web/pom.xml中jstl版本为1.2

![image-20220402095847494](https://tva1.sinaimg.cn/large/e6c9d24egy1h0v5912y1sj20ln0ik0ug.jpg) 

#### （3） 配置tomcat路径及端口。

![image-20220402100036176](https://tva1.sinaimg.cn/large/e6c9d24egy1h0v5awib8xj20sy0m5dhs.jpg) 

#### （4） 部署对应war包。

![image-20220402100135824](https://tva1.sinaimg.cn/large/e6c9d24egy1h0v5bya1foj20tb0k7gn0.jpg) 

#### （5） 通过maven安装对应依赖

![image-20220402100333956](https://tva1.sinaimg.cn/large/e6c9d24egy1h0v5dzm6rtj20hs0h3q47.jpg) 

#### （6） 设置字节码版本 不然会报错

![image-20220402100421786](https://tva1.sinaimg.cn/large/e6c9d24egy1h0v5etonybj20r20jvwgh.jpg)

#### (7)debug

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h0v570orz4j21f60u0abm.jpg) 

#### （8） 在对应代码打断点即可进行调试

![image-20220402105819077](https://tva1.sinaimg.cn/large/e6c9d24ely1h0v6yytsw1j20z60o0gnv.jpg)

####  （9）断点1![image-20220402163141806](https://tva1.sinaimg.cn/large/e6c9d24egy1h0vglufzwdj20wy0nxq69.jpg)

#### （10）login 断点

![image-20220402163333153](https://tva1.sinaimg.cn/large/e6c9d24egy1h0vgnrwytlj20ux09ugmr.jpg)

#### （11）login实现代码

![image-20220402164906116](https://tva1.sinaimg.cn/large/e6c9d24egy1h0vh3yganbj20vv09ogmv.jpg)

![image-20220402170109744](https://tva1.sinaimg.cn/large/e6c9d24egy1h0vhgibddlj20s7046q3n.jpg)

#### （11）base64编码点

![image-20220402163231391](https://tva1.sinaimg.cn/large/e6c9d24egy1h0vgmoy84wj20ue062wf2.jpg)