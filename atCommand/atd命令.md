### atd命令

[鸟哥的私房菜](http://cn.linux.vbird.org/linux_basic/0430cron_1.php)定义atd 服务如下：

>- at ：at 是个可以处理仅运行一次就结束排程的命令，不过要运行 at 时，
>  ​		必须要有 atd 这个[服务 (第十八章)](http://cn.linux.vbird.org/linux_basic/0560daemons.php) 的支持才行。在某些新版的 distributions 
>  ​		中，atd 可能默认并没有启动，那么 at 这个命令就会失效呢！不过我们的 CentOS 默认是启动的！

#### 安装

Ubuntu自身不带atd功能，可以使用以下命令安装：

```
sudo apt install atd
```

鸟哥介绍需要使用atd restart命令启动atd，但是在Ubuntu平台下，该命令并无输出。

#### 使用

命令格式为：

`at TIME` ，例如：`at now+1minutes` ; 

`at -c 工作号` 查看已列工作，例如：`at -c 1` 。

一次性命令设置过程如下：

```
[zhibin@zhibin-vertual-machine]# at now+5minutes  <==记得单位要加 s 喔！
at>echo 123
at> <EOT>   <==这里输入 [ctrl] + d 就会出现 <EOF> 的字样！代表结束！
job 4 at 2009-03-14 15:38
```

该命令执行完之后，生成了job 4命令。

> 所有的命令都可以执行，
>
> 例如关机之前两遍sync 命令：
>
> at>sync
>
> at>sync
>
> at>shutdown -h now

查看已列命令：

```
[zhibin@zhibin-vertual-machine]# at -c 4
#!/bin/sh               <==就是透过 bash shell 的啦！
# atrun uid=0 gid=0
# mail     root 0
umask 22
....(中间省略许多的环境变量项目)....
cd /root || {           <==可以看出，会到下达 at 时的工作目录去运行命令
         echo 'Execution directory inaccessible' >&2
         exit 1
}

echo 10
```

#### 查看结果

当5分钟过后，该命令已经被执行，我们再执行`at -c 4` ，结果如下：

```
Cannot find jobid 4
You have new mail in /var/mail/zhibin
```

根据提示，我们从`/var/mail/zhibin` 查看结果。

#### 后悔药：取消命令

查看已建哪些命令：`atq` ，会列出等待执行的命令。

取消命令：`atrm 5` 取消`job 51` 命令。