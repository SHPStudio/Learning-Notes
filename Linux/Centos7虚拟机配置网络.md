# centos7 vbox与宿主机互通
## 介绍
  我们安装centos7后发现系统不能访问外网也不能与宿主机ping通
  这里我们需要使用nat网络nat网络宿主机到虚拟机会有问题，其他不会有问题
  所以我们还需要配置一个Host-only来处理宿主机到虚拟机的通信问题

## nat
  首先配置nat网卡 让系统能够访问外网和宿主机

  1. 设置网卡1为nat模式 把mac地址复制下来并勾选上接入网线
  2. 到系统中的`/etc/sysconfig/network-scripts`下创建ifcfg-eth0文件并进行配置

            DEVICE=eth0
            HWADDR=08:00:27:23:02:75
            ONBOOT=yes
            NM_CONTROLLED=yes
            BOOTPROTO=dhcp

  文件名和device一样，HWADDR是设置的网卡的时候的mac地址
  3. 重启机器

  4. 设置网卡2为host-only模式 同样需要把mac地址复制下来
  5. 进入系统中的`/etc/sysconfig/network-scripts`下创建ifcfg-enp0s3
  进行配置

            TYPE=Ethernet
            BOOTPROTO=dhcp
            DEFROUTE=yes
            PEERDNS=yes
            PEERROUTES=yes
            IPV4_FAILURE_FATAL=no
            IPV6INIT=yes
            IPV6_AUTOCONF=yes
            IPV6_DEFROUTE=yes
            IPV6_PEERDNS=yes
            IPV6_PEERROUTES=yes
            IPV6_FAILURE_FATAL=no
            NAME=enp0s3
            UUID=cace23fb-1d7e-4fa3-a23a-bf14c2ce6fe8
            DEVICE=enp0s3
            ONBOOT=no

   6. 重启机器 使用 `ip ad`查看网卡信息 host-only配置的ip就可以通过宿主机去访问虚拟机，虚拟机之前通信需要使用nat网卡的配置的ip
