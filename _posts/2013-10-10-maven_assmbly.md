---
layout: mypost
title: "如何用maven将java工程打包为jar文件"
tagline: "如何用maven将java工程打包为jar文件"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。如何用maven将java工程打包为jar文件"
category: Maven
tags: [Maven]
---



####1.可使用jar插件完成.

	<plugin>
		<!-- Build an executable JAR -->
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-jar-plugin</artifactId>
		<configuration>
		    <archive>
		        <manifest>
		            <addClasspath>true</addClasspath>
		            <classpathPrefix>lib/</classpathPrefix>
		            <mainClass>com.badnotes.task.Task</mainClass>
		        </manifest>
		    </archive>
			<finalName>taskService</finalName>
		</configuration>
	</plugin>

user>mvn package # 即可完成打包.

user>java -jar taskService.jar #但是如果工程有依赖其他包的话，会出现找不到class异常.



####2.这个时候用assembly这个插件则可以将相应的依赖一起打入包中.

	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-assembly-plugin</artifactId>
		<configuration>
		    <archive>
		        <manifest>
		            <mainClass>com.badnotes.task.Task</mainClass>
		        </manifest>
		    </archive>
		    <descriptorRefs>
		        <descriptorRef>jar-with-dependencies</descriptorRef>
		    </descriptorRefs>
		</configuration>
	</plugin>

user>mvn assembly:assembly 

user>java -jar taskService-jar-with-dependencies.jar

ok,正常运行。

