# day11

---

## 日志管理

---

### 日志进程rsyslog

哪类程序--->产生什么日志--->放到什么地方

```
第一类
rsyslogd：系统专职日志程序
处理绝大部分日志记录
系统操作有关的信息，如登录信息，程序启动关闭信息，错误信息
```

```
第二类
httpd/nginx/mysql: 各类应用程序，可以以自己的方式记录日志讲解对应程序时会逐步介绍
```

```
d：deamon：程序
[root@localhost ~]# ps aux | grep reyslogd
root       2453  0.0  0.0 112676   980 pts/0    S+   16:19   0:00 grep --color=auto reyslogd
```

常见的日志文件（系统，进程，应用程序）

```
tail -10 /var/log/messages    //系统主日志文件
tail -f /var/log/messages     //动态查看日志文件的尾部
tailf  /var/log/secure        //认证，安全
tail /var/log/yum.log         //yum
tail /var/log/mailog          //跟邮件的postfix相关
tail /var/log/cron            //crond,at进程产生的日志
tail /var/log/dmesg           //和系统启动相关
```

rsyslogd配置

```
相关程序
yum install rsyslog logrotate
默认已安装
启动程序
systemctl start rsyslog.service
相关文件
rpm -qc rsyslog 
/etc/rsyslog.conf	rsyslogd的主配置文件（关键）
/etc/sysconfig/rsyslog		rsyslogd相关文件，定义级别（了解一下）
/etc/logrotate.d/syslog		和日志办轮转（切割）相关（任务二）
```

主配置文件

```
告诉rsyslogd进程什么日志，应该存到哪里
vim /etv/rsyslog.conf
RULES：即规则，是生成一套日志，以及存储日志的策略
		规则由设备+级别+存储位置组成
		RULES由FACILITY+LEVEL+FILE组成
		
```

---

### 日志轮转logrotate

简介

```
将大量的日志，分割管理，删除旧日志
日志 记录了程序运行时的各种信息
通过日志可以分析用户行为，记录运行轨迹，查找程序问题
可以磁盘的空间是有限的
日志轮转就行飞机里的黑匣子，记录的信息再重要也只能记录最后一段时间发生的事
为了节省空间和整理方便，日志文件经常需要按！时间或！大小等维度分成多份的，删除时间久远的日志文件
```

工作原理

```
配置未见种类
主文件： /etc/logrotateconf（决定每个日志文件如何轮转）
子文件夹： /etc/logrotate.d/*(自定义配置，方便管理)

主配置文件介绍
vim /etc/logrotate.conf
全局设置
week      //轮转的周期，一周轮转
rotate4       //保留四份
create        //轮转后创建新文件
dateext       //使用日期作为后缀
#compress      //是否压缩
include  /etc/logrotate.d     //包含该目录下的子配置文件

/var/log/wtmp{				//对某日志文件设置轮转的方法
monthly						//一月轮转一次
minsize  1M					//最小达到1M才轮转，monthly and minsize
create 0664 root utmp		//轮转后创建文件，并设置权限
rotate 1					//保留一份
}

/var/log/btmp{
missingok			//丢下不提示
monthly					//每月轮转一次
create 0600 root utmp		//轮转后创建文件，并设置权限
rotate 1					//保留一份
}
```

yum日志轮转实例

```
轮转的目标文件/var/log/yum.log
配置轮转规则
# vim/etc/logrotate.d/yum
/var/log/yum.log{
missingok			//丢失不执行
#notifempty				//空文件不轮转
#maxsize 30k			//达到30k轮转，daily	or	size
#yearly				//或者一年一轮转
daily					//速效周期到一天
rotate 3					//轮转保留三次
cretate 0600 root root
}
测试
修改时间，手动触发轮转
#date 04011000
#/usr/sbin/logrotate -s /var/lib/logrotate/logrotate.staus /etc/logrotate.conf
#ls /var/log/yum*
```







