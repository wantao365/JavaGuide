### Centos修改init启动级别

> 为什么要修改启动级别，因为图形桌面环境会占用很多内存如果修改为最小化安装 minimal 则会节省很大空间
> free -h 查看当前内存使用情况

配置文件为【 /etc/inittab】

```shell
 multi-user.target: analogous to runlevel 3 （字符界面）
 graphical.target: analogous to runlevel 5 （图形界面）
```

查看当前启动运行级别

```shell
[root@localhost ~]# runlevel 
N 5
```

设置当前默认启动为 【multi-user.target】

```shell
[root@localhost ~]# systemctl set-default multi-user.target
```

> 修改配置文件 【vim /etc/inittab】
>
> 把 【multi-user.target: analogous to runlevel 3】 前的注释去掉
>
> 最后加上一段 【id:3:initdefault:】
>
> ```shell
> # inittab is no longer used when using systemd.
> #
> # ADDING CONFIGURATION HERE WILL HAVE NO EFFECT ON YOUR SYSTEM.
> #
> # Ctrl-Alt-Delete is handled by /usr/lib/systemd/system/ctrl-alt-del.target
> #
> # systemd uses 'targets' instead of runlevels. By default, there are two main targets:
> #
>  multi-user.target: analogous to runlevel 3
> # graphical.target: analogous to runlevel 5
> #
> # To view current default target, run:
> # systemctl get-default
> #
> # To set a default target, run:
> # systemctl set-default TARGET.target
> id:3:initdefault:
> ~
> ~
> ~
> :wq                                                                                                                         
> ```
>
> ```
> [root@localhost ~]#reboot
> ```

> Centos6 修改 init 级别
>
> 只需要把 【id:5:initdefault:】5改为3 重启虚拟机即可

