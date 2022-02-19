# day10

---

## 计划任务

---

### 简介

作用：计划任务主要是做一些周期性的任务，目前最主要的用途是定期备份数据

### 一次性调度执行 at

```
- 语法格式 at<TIMESPEC>
```

**例如**

```
初识一次性任务计划

at
设置一个定时创建用户的任务
[root@localhost ~]# at now +1min
at> useradd uuuu 
at> <EOT>
at> <EOT>
job 1 at Thu Feb 10 19:43:00 2022
[root@localhost ~]# id uuuu
uid=1007(uuuu) gid=1008(uuuu) 组=1008(uuuu)

calhost ~]# at now +1min
at> useradd tian
at> <EOT>
job 2 at Thu Feb 10 19:45:00 2022
[root@localhost ~]# atq
2	Thu Feb 10 19:45:00 2022 a root
```

### 循环调度执行`cron`

简介：`cron`的概念和`crontab`是不可分割的

`crontab`是一个命令，常见于Unix和Linux的操作系统之中

用于设置周期性呗执行的指令

该命令从标准输入设备读取指令，并将其存放于"`crontab`"文件中，以供之后读取和执行。



查看进程状态：

```
[root@localhost ~]# systemctl status crond.service
● crond.service - Command Scheduler
   Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; vendor preset: enabled)
   Active: active (running) since 四 2022-02-10 16:10:53 CST; 3h 44min ago
 Main PID: 1167 (crond)
   CGroup: /system.slice/crond.service
           └─1167 /usr/sbin/crond -n

2月 10 16:10:53 localhost.localdomain systemd[1]: Started Command Scheduler.
2月 10 16:10:53 localhost.localdomain systemd[1]: Starting Command Scheduler...
2月 10 16:10:53 localhost.localdomain crond[1167]: (CRON) INFO (RANDOM_DELAY will ...)
2月 10 16:10:54 localhost.localdomain crond[1167]: (CRON) INFO (running with inoti...)
Hint: Some lines were ellipsized, use -l to show in full.

[root@localhost ~]# ps aux | grep crond
root       1167  0.0  0.0 126236  1664 ?        Ss   16:10   0:01 /usr/sbin/crond -n
root       4951  0.0  0.0 112676   980 pts/0    R+   19:57   0:00 grep --color=auto crond

```

`cron`

```
计划任务存储位置：[root@localhost ~]# ls /var/spool/crom/
管理方式
创建计划：[root@localhost ~]# crontab -e Edit jobs for the current user
        crontab: usage error: no arguments permitted after this option
        Usage:
         crontab [options] file
         crontab [options]
         crontab -n [hostname]

        Options:
         -u <user>  define user
         -e         edit user's crontab
         -l         list user's crontab
         -r         delete user's crontab
         -i         prompt before deleting
         -n <host>  set host in cluster to run users' crontabs
         -c         get host in cluster to run users' crontabs
         -s         selinux context
         -x <mask>  enable debugging

        Default operation is replace, per 1003.2

查询计划：[root@localhost ~]# crontab -l List the jobs for the current user
		管理员可以使用-u usermame，去管理其他用户的计划任务

删除计划：[root@localhost ~]# crontab -r Remove all jobs for the current users
```