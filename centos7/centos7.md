# CentOS 7 

> 下载 http://mirrors.163.com/centos/7/isos/x86_64/

1. 网络适配器

   选“桥接模式”

2. 查看 IP

   ```shell
   ip addr
   ```

3. 修改网卡配置

   ```shell
   # vi /etc/sysconfig/network-scripts/ifcfg-ens33
   ONBOOT=yes 
   # wq 保存
   ```

4. 重启

   ```shell
   service network restart
   ```

5. 分配IP

   ```shell
   dhclient
   ```

6. 查看IP (ens33)

   ```shell
   [root@localhost ~]# ip addr
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
       link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
       inet 127.0.0.1/8 scope host lo
          valid_lft forever preferred_lft forever
       inet6 ::1/128 scope host 
          valid_lft forever preferred_lft forever
   2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
       link/ether 00:0c:29:2f:8c:43 brd ff:ff:ff:ff:ff:ff
       inet 192.168.1.5/24 brd 192.168.1.255 scope global noprefixroute ens33
          valid_lft forever preferred_lft forever
       inet6 2409:8a60:2a47:7170:c3ce:909f:3830:d848/64 scope global noprefixroute dynamic 
          valid_lft 192303sec preferred_lft 105903sec
       inet6 fe80::4b79:cf9f:3f01:d5fc/64 scope link noprefixroute 
          valid_lft forever preferred_lft forever
   
   ```

7. 修改为静态IP

   ```shell
   BOOTPROTO=static
   
   ONBOOT=yes
   IPADDR=192.168.1.5
   NETMASK=255.255.255.0
   GATEWAY=192.168.1.1
   DNS1=119.29.29.29
   
   ```

8. 重启

   ```shell
   service network restart
   ```

   





