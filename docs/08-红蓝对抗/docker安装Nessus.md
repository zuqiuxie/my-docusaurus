# docker安装Nessus

> 因为我电脑是mac的所以安装不了Nessus，安装是总是有问题
>
> 然后就在晚上找了docker镜像进行安装

v1:镜像较大，8GB左右，适用于网速较好的情况，不需要等待容器初始化。

- `docker run --rm -itd -p 8834:8834 registry.cn-hangzhou.aliyuncs.com/steinven/nessus:v0.1`，注意该命令中的`--rm`参数，每次容器停止后会自动删除，如不想删除，去掉该参数即可
- 访问`https://ip:8834`（注意是`https`），账号：`admin`密码：`admin`
- 如果长时间无法访问，请在宿主机执行`docker exec 容器名 /etc/init.d/nessusd start`,手动起一下服务，什么原因导致的暂不清楚

v2:大小1GB多，需要手动激活（license已在容器内，不再需要web端申请），需要等待初始化，注册完成后需要保存快照。

- `docker run --rm -itd -p 8834:8834 registry.cn-hangzhou.aliyuncs.com/steinven/nessus:v0.2`
- 登录Web进行管理员账号注册并端等待初始化完成
- 容器内进行license注册：`/opt/nessus/sbin/nessuscli fetch --register-offline /tmp/license`
- 容器内重启服务:`/etc/init.d/nessusd restart`
- 宿主机对容器打快照：`docker commit xx:xx 容器ID`

