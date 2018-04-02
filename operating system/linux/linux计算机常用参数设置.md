linux服务器基本设置
- 
1.关闭防火墙（以centos6.5为例）
- 
	1.查看防火墙状态 service iptables status
	2.关闭（打开）防火墙 
		1.永久生效
		 chkconfig iptables off（on）
		2.重启时效
		 service iptables stop(start)
	
2.修改ip地址
- 
	修改eth0文件
    1.编辑网卡文件 vi /etc/sysconfig/network-script/ifcfg-eth0
	2.修改ip为静态 BOOTPROTO="static"
    3.添加ip IPADDR=192.168.100.251
    4.需要修改mak地址的可以 注释掉 HWADDR 添加 MACADDR="00:0C:29:B3:40:11"
    5.重启网卡服务 service network restart
3.主机名和域名修改
- 
	1.修改计算机名称
		1.打开配置文件 vi  /etc/sysconfig/network
		2.设置HOSTNAME=计算机名称
		3.reboot
	2.修改域名解析
		查看当前主机名 hostname
		1.修改hosts文件 vi /etc/hosts 在末尾加上 ip地址或者127.0.0.1 域名
	    2.退出保存即可，测试 ping 域名
	