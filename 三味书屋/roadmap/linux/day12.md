# day12

---

## 网络管理

---

### NetworkManager服务

```
网络管理器是一个动态的控制器与配置系统，它用于当网络设备可用时的保持设备和连接开始并激活
默认情况下，CentOS/RHEL 7 已安装网络管理器，并处于启用状态。
systemctl status NetworkManager			查看网络管理程序的状态
systemctl status network				查看网络子管理程序的状态
```

### 配置网络的工具

```
配置网络的方法：命令，字符，图形
命令行配置：
配置文件vim：vim /etc/sysconfig/network-scripts/ifcfg-ens32
命令行	nmcli ：如果没有这个命令，可以执行安装  yum -y install NetworkManager

图形配置
简易图形：nmtui
界面图形：nm- connection- editor
```

```
显示机器中含有几块网卡：nmcli device
```

---

### 配置网络参数

配置ip

```
配置网卡参数
先备份网卡配置文件，再修改
cp /etc/sysconfig/network-scripts/ifcfg-ens33  .
vim /etc/sysconfig/network-scripts/ifcfg-ens33
修改文件要谨慎
ONBOOT=yes								//是否启用该设备
BOOTPROTO=none							//手动（none）自动（dhcp）static（静态）
概念：获取地址 的方法	   boot		启动		protocol		协议
IPADDR=192.168.0.16						//根据自动获取的地址进行配置，用来定位主机
	ip=	互联网协议			ADDRESS		地址
NETMASK=255.255.255.0					//子网掩码		用来定义网络，这台机器是192.168.0的网络
GATEWAY=192.168.0.2						//网关，也叫默认路由，带你上网的路由器的地址
DNS1=192.168.0.2						//域名解析。当你输入域名访问网站时，它会告诉你ip地址。
DNS2=114.114.114.114					
网卡信息
```

```
mac地址=硬件地址
```

```
ip地址是一台主机在网路中的标识。同一个网络可以直接通信。
```

```
查看ip地址		ip a
```

```
重启网卡		systemtcl restart network
```

更改主机名

```
查看主机名 #hostname
配置主机名 #hostnamectl set-hostname fengfeng.example.com
查看和配置主机名 #cat /etc/hostname
重启生效 #reboot
```

网络测试工具

```
ip a   //查看所有ip（ifconfig）
ip route   //查看路由，查看网关
ip neigh   //另一台主机ping通，查看邻居
ping 127.0.0.1		回环地址
```

显示端口号

```
ss -tnl
```

初始化服务器

```
最小化安装：兼容程序	开发包
为你的服务器配置root密码
配置ip地址（VMNAT8）
配置YUM源		自动挂载光驱/阿里YUM		配置YUM仓库
关闭防火墙
systemctl stop firewald
systemctl disable firewald
systemctl staus firewald		检查状态

selinux		
			临时关闭	setenforce 0
			永久关闭	vim /etc/sysconfig/selinux
						SELINUX=disabled

安装常用程序	yum install -y lrzsz sysstat elinks wget net-tools bash-completion
关机快照
```
