使用tomcat时的一些异常

#### 1.NoClassDefFoundError

eg:

	Caused by: java.lang.ClassNotFoundException: com.google.common.collect.Table

问题原因

a：无相应jar包 

b: jar 未添加到classpath

c: 未载入





更换tomcat，编译正常，运行找不到Class，重新引用打包文件


http://vipcowrie.iteye.com/blog/1561291
http://vipcowrie.iteye.com/blog/1562251



#### 2.启动tomcat，访问无响应一直处于等待状态
有时该tomcat已经运行了几个进程了，ps -ef|grep java 查看，结束相关的全部进程重启



		new Thread(new Runnable() {
            @Override
            public void run() {
                // do something
            }
        }).start();



        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {

            }
        });
        thread.setDaemon(true);  // 默认为false 设置为用户线程
        thread.start();

        // 当为false时，其他线程都结束了，此线程任务没有完成也不会停止

#### 3.更新war无效

a: <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
b: 情况2
c: 文件权限不够
部署tomcat war包是不能自动解包，删除之前解包后的文件重启

刷新 maven