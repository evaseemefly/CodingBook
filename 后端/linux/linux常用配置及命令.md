_本记录迁移自 onenote 中的部分总结的知识点_

# 1 常用配置

## 1.1 配置共享目录：

## 1.2 开启 ssh 登录

参考文章：
https://segmentfault.com/a/1190000004686476

1. 安装 ssh
   这里使用的是 openssh 系列工具

更新下系统工具和依赖,执行 sudo apt-get update;

执行安装命令:sudo apt-get install openssh-server openssh-client,等待安装完成。

2. 启动 ssh
   先查看 ssh 是否在运行,执行命令 sudo ps -e |grep ssh,如果没有任何显示，是没有运行 ssh 服务的.反之,出现 sshd 字样，表示 ssh 服务已运行.

如果 ssh 没有运行,使用，命令 sudo service ssh start 启动.

```启动
/etc/init.d/ssh start #停止
/etc/init.d/ssh stop #重新启动
/etc/init.d/ssh restart

#利用 service 命令 #启动
sudo service ssh start #停止
sudo service ssh stop #重新启动
sudo service ssh restart
```

至此,ssh 已经安装并运行起来,可以使用 ssh 客户端连接了.但是,现在是连接不上的.
在 ssh 登录的时候会提示 SSh 服务拒绝连接,怎么破,请往下看.

3. 修改配置文件
   此处修改的是 ssh 配置文件:`/etc/ssh/sshd_config`

使用命名打开配置文件:gedit `/etc/ssh/sshd_config`;

将配置文件中的 PermitRootLogin without-password 一行注释掉;

添加新的一行内容:PermitRootLogin yes;

保存配置文件,重启 ssh 服务.

```

```
## 1.3 设置ftp
[参考文章](http://light3moon.com/2015/01/19/ubuntu%20%E5%BC%80%E5%90%AF%20ftp%20%E6%9C%8D%E5%8A%A1/)
直接从源里面安装：
```
sudo apt-get install vsftpd
```
安装完毕后或许会自动生成一个帐户”ftp”，/home下也会增加一个文件夹。
如果没有生成这个用户的话可以手动来，生成了就不用了：
```
sudo useradd -m ftp
sudo passwd ftp
```
有”ftp”帐户后还要更改权限：
```
sudo chmod 777 /home/ftp
```
在这个目录下我建立一个文件夹专门保存需要共享的内容

配置文件
通过`sudo vim /etc/vsftpd.conf`修改。配置文件比较简单，如下：

```
#独立模式启动
listen=YES

#同时允许4客户端连入，每个IP最多5个进程
max_clients=200
max_per_ip=4

#不允许匿名用户访问，允许本地（系统）用户登录
anonymous_enable=NO
local_enable=YES
#注意此处必须改为YES，否则有问题
write_enable=YES

#是否采用端口20进行数据传输
connect_from_port_20=YES

#生成日志
xferlog_enable=YES

#指定登录转向目录
local_root=/home/ftp/ftp
```
这样，在同局域网的电脑上，用我的IP地址，用帐号”ftp”和对应密码就可以登录了，密码是第一步里面passwd那句指定的。就这样就结束了，请大家拍砖！！对了，更改配置后不要忘了重启ftp服务：
```
sudo /etc/init.d/vsftpd restart
```
此外还有开启关闭服务的命令：
```
sudo /etc/init.d/vsftpd start
sudo /etc/init.d/vsftpd stop
```

若出现无法上传的情况，需要将上面的`write_enable`改为`YES`    

还需要查看被上传的目录下的各文件夹的权限：
```
sudo chmod -R 777 *
```