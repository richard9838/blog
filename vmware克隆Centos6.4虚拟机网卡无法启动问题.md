**快速处理办法：**

```bash
cat /etc/sysconfig/network-scripts/ifcfg-eth0
sed -i '/UUID/d' /etc/sysconfig/network-scripts/ifcfg-eth0
sed -i '/HWADDR/d' /etc/sysconfig/network-scripts/ifcfg-eth0
>/etc/udev/rules.d/70-persistent-net.rules
reboot
```

===========================================================

**vmware克隆Centos6.4虚拟机网卡无法启动问题**

在老男孩的教学过程中，发现有学生遇到如下问题而无法解决。

通过vmware8的完全克隆功能快速创建一台版本为CentOS 6.4的linux虚拟机。
创建后症状：启动之后使用ifconfig，发现无ip地址，只有回环地址为127.0.0.1，
MAC地址以及主机名都和源主机相同（源主机采用手动方式配置的IP）。
无论如何执行下面命令都无济于事：
/etc/init.d/network restart
ifup eth0

解决办法：
1.编辑eth0的配置文件：vi /etc/sysconfig/network-scripts/ifcfg-eth0，删除HWADDR地址那一行及UUID的行如下：
HWADDR=00:0c:29:08:28:9f
UUID=cee39dbb-6a10-4425-9daf-768b6e79a9c9
提示：当然你也可以根据实际的HWADDR和UUID修改，而不删除。见
/etc/udev/rules.d/70-persistent-net.rules

eth0网卡文件修改后：

```bash
[root@oldboy ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=none
IPADDR=10.0.0.27
NETMASK=255.255.255.0
DNS2=8.8.8.8
GATEWAY=10.0.0.254
DNS1=10.0.0.254
IPV6INIT=no
USERCTL=no
```

2.如果有必要再清空如下文件：
\> /etc/udev/rules.d/70-persistent-net.rules。
提示：机器名可以不改

3.重启系统：reboot或在VM外面重启。

4.原因猜测：VM克隆为了保护源机器和克隆机器启动网路配置地址冲突而做的保护策略。