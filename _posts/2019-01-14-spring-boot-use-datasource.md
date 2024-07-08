---
layout: mypost
title: "Spring boot Use Datasource"
tagline: "Spring boot Use Datasource"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Spring boot Use Datasource"
category: Spring
tags: [Spring]
---



Spring boot Use Datasource
---------------------------------

#### 1. 数据源配置

```properties
# application.properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost/ares
spring.datasource.username=
spring.datasource.password=
```



#### 2. 默认连接池

​	默认使用 hikari连接池, 如果运行环境没有hikari包,会加载失败,将尝试使用tomcat-jdbc

```java
// DataSourceBuilder.java

	private static final String[] DATA_SOURCE_TYPE_NAMES = new String[] {
			"com.zaxxer.hikari.HikariDataSource",
			"org.apache.tomcat.jdbc.pool.DataSource",
			"org.apache.commons.dbcp2.BasicDataSource" };

	@SuppressWarnings("unchecked")
	public static Class<? extends DataSource> findType(ClassLoader classLoader) {
		for (String name : DATA_SOURCE_TYPE_NAMES) {
			try {
				return (Class<? extends DataSource>) ClassUtils.forName(name,
						classLoader);
			}
			catch (Exception ex) {
				// Swallow and continue
			}
		}
		return null;
	}

	private Class<? extends DataSource> getType() {
		Class<? extends DataSource> type = (this.type != null) ? this.type
				: findType(this.classLoader);
		if (type != null) {
			return type;
		}
		throw new IllegalStateException("No supported DataSource type found");
	}
```



#### 3. 手动指定连接类型

```properties
# application.properties
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
```



#### 4. 连接池其他配置参数

​	a). 通过Spring配置设置连接池参数

```properties
# application.properties
spring.datasource.hikari.connectionTimeout=30000
spring.datasource.hikari.idleTimeout=600000
spring.datasource.hikari.maxLifetime=1800000
```

​	b). HikariDataSource 直接继承自HikariConfig, 如果未设置Spring的Datasource参数,Spring 框架也并未提供任何默认参数,而是直接使用Hikari的默认配置. Hikari的默认配置就是已经优化过的,在大多数情况下你不需要设定其他值.

```java
// HikariDataSource.java
public class HikariDataSource extends HikariConfig implements DataSource, Closeable
```

> ​	HikariCP comes with *sane* defaults that perform well in most deployments without additional tweaking. **Every property is optional, except for the "essentials" marked below.**
>
> [HikariCP#Configruration]: https://github.com/brettwooldridge/HikariCP#configuration-knobs-baby	"HikariCP Configuration"
