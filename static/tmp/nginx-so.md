nginx

nginx 安装后运行总是找不到libpcre模块

	start nginx：error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory

意思是找不到libpcre.so.1这个模块，而导致启动失败。


[user@vps ~]# /usr/local/webserver/nginx/sbin/nginx
nginx: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory
经过搜索资料，发现部分linux系统存有的通病。要解决这个方法非常容易

如果是32位系统
[user@vps ~]#  ln -s /usr/local/lib/libpcre.so.1 /lib

如果是64位系统
[user@vps ~]#  ln -s /usr/local/lib/libpcre.so.1 /lib64

然后在启动nginx就OK了
[user@vps ~]# /usr/local/webserver/nginx/sbin/nginx