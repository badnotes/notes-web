linux-info

查看内核版本系统位数等
	
	uname -a
	Linux 192-168-0-1 2.6.32-279.el6.x86_64 #1 SMP Fri Jun 22 12:19:21 UTC 2012 x86_64 x86_64 x86_64 GNU/Linux


查看系统版本

	# 适用于所有的linux，包括Redhat、SuSE、Debian等发行版。

	cat /etc/redhat-release

	CentOS release 6.3 (Final)


	# 此命令系统默认没安装
	lsb_release -a

	LSB Version:    :core-3.1-ia32:core-3.1-noarch:graphics-3.1-ia32:graphics-3.1-noarch
	Distributor ID: CentOS
	Description:    CentOS release 5.4 (Final)
	Release:        5.4
	Codename:       Final
