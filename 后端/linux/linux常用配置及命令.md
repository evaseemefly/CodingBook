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
