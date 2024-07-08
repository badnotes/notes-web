---
layout: mypost
title: "Google Guava Cache 使用入门"
tagline: "Google Guava Cache 使用入门"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Google Guava Cache 使用入门。"
category: Cache
tags: Java Cache 
---




#### 0. 前言
为了更加有效的使用资源，任何项目中都必须考虑缓存问题。需要结合实际去考虑如何构架。
这里主要总结下如何借用 Guava Cache 使用 jVM 缓存。

#### 1. 初始化,两种方式

**初始化时定义数据加载源**

    LoadingCache<Object, Graph> cache = CacheBuilder.newBuilder()
        .maximumSize(10000)
        .expireAfterWrite(10, TimeUnit.SECONDS)
        .build(new CacheLoader<Object, Object>() {
            @Override
            public Graph load(Graph o) throws Exception {
                return createExpensiveGraph(key);
            }
        });
    Object value = cache.get("key");

**在使用get()时定义数据加载源**

    Cache<String, Object> cache = CacheBuilder.newBuilder().maximumSize(1000).build();
    Object value = cache.get("key", new Callable<Object>() {
        public Object call() {
            createExpensiveGraph(key);
        }
    });

使用第二种定义方式可以在为每个key定义数据加载方式。好像更加灵活。

不过第一种也可以在get(key, Callable)时重新实现数据加载方式。

#### 2. expireAfterWrite refreshAfterWrite 区别

* CacheBuilder.newBuilder().expireAfterWrite(1, TimeUnit.SECONDS);

    当Cache里面有多个值过期，未设置自动清除过期数据，

    cache.size() # 过期数据仍然在Cache里面

    cache.get("key") # 将清楚所有过期数据，若key过期会重新加载

* CacheBuilder.newBuilder().refreshAfterWrite(1, TimeUnit.SECONDS);

    当Cache里面有多个值过期，未设置自动清除过期数据，

    cache.size() # 过期数据仍然在Cache里面

    cache.get("key") # 将更新所有过期数据(包括其他过期的key)，


#### 3. expireAfterWrite expireAfterAccess 区别

* expireAfterAccess(long, TimeUnit)  
    基于访问（read or write），以最后一次访问时间算

* expireAfterWrite(long, TimeUnit)  
    基于创建（after create），以创建时间计算

#### 4. 回收的参数设置

* 大小的设置：CacheBuilder.maximumSize(long) CacheBuilder.weigher(Weigher) CacheBuilder.maxumumWeigher(long)
* 时间：expireAfterAccess(long, TimeUnit) expireAfterWrite(long, TimeUnit)
* 引用：CacheBuilder.weakKeys() CacheBuilder.weakValues() CacheBuilder.softValues()
* 明确的删除：invalidate(key) invalidateAll(keys) invalidateAll()
* 删除监听器：CacheBuilder.removalListener(RemovalListener)


#### 5. refresh机制

* LoadingCache.refresh(K) 在生成新的value的时候，旧的value依然会被使用。
* CacheLoader.reload(K, V) 生成新的value过程中允许使用旧的value
* CacheBuilder.refreshAfterWrite(long, TimeUnit) 自动刷新cache

设置自动清除cache，可将未过期的cache清除，到期的缓存会自动被清除掉

* Cache.invalidate(key)
* Cache.invalidateAll(keys)
* Cache.invalidateAll()

手动清除过期缓存，方法被调用是执行，可以定义定时器定期清除。

* cache.cleanUp();

直接往cache里面放数据，而不是其他方式加载。

* cache.put(key,value);
* cache.putAll(Map<?,?>);


