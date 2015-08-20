---
layout: post
title: "how to add linux machine to windows domain"
tagline: "how to add linux machine to windows domain"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。how to add linux machine to windows domain"
category: Linux
tags: [Linux]
---
{% include JB/setup %}

For join to windows domain using linux, we will install likewise-open or pbis-open. now I use pbis-open.


##### 1. download pbis-open

*download at*:
http://download1.beyondtrust.com/Technical-Support/Downloads/PowerBroker-Identity-Services-Open-Edition/?Pass=True


##### 2. install it


```> sudo chmod a+x pbis-open-8.3.0.3287.linux.x86_64.deb.sh```


##### 3. nano edit and paste above contents


```> sudo nano /lib/systemd/system/lwsmd.service```


contents:

	# =========================================
	[Unit]
	Description=BeyondTrust PBIS Service Manager
	After=network.target

	[Service]
	Type=forking
	EnvironmentFile=/opt/pbis/libexec/init-base.sh
	ExecStart=/opt/pbis/sbin/lwsmd --start-as-daemon
	ExecReload=/opt/pbis/bin/lwsm refresh
	ExecStop=/opt/pbis/bin/lwsm shutdown
	# We want systemd to give lwsmd some time to finish gracefully, but still want
	# it to kill lwsmd after TimeoutStopSec if something went wrong during the
	# graceful stop. Normally, Systemd sends SIGTERM signal right after the
	# ExecStop, which would kill lwsmd. We are sending useless SIGCONT here to give
	# lwsmd time to finish.
	KillSignal=SIGCONT
	PrivateTmp=true

	[Install]
	WantedBy=multi-user.target nss-lookup.target
	# ===========================================

##### 4. start lwsmd

show the lwsmd status 
```> service lwsmd status```

start lwsmd service 
```> service lwsmd start```

make the service start at boot system 
```> systemctl enable lwsmd.service```


##### 5. join to domain


```> sudo domainjoin-cli query```

```> sudo domainjoin-cli join --disable ssh domainname.com administrator```


##### 6. copy before user home files to domain user home 

##### 7. logout and login using domain\username



@resources

* https://www.linux.com/learn/tutorials/336477:how-to-join-a-ubuntu-machine-to-a-windows-domain
* http://askubuntu.com/questions/452904/likewise-open-14-04-other-easy-way-to-connect-ad
* http://askubuntu.com/questions/613451/ubuntu-15-04-join-domain-problem-pbis
