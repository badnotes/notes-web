---
layout: mypost
title: "使用redis做数据缓存"
tagline: "使用redis做数据缓存"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。使用redis做数据缓存。"
category: Cache
tags: Cache Redis
---



##### 1. 每次使用时都从redis缓存加载

利：自定义缓存时间，很方便的手动清理

弊：redis和应用可能不在同一台机器，使用时还有通过网络传输。   
   即时在同一台机器，也要通过网络层进行数据交换。  
   所以如果使用非常频繁的数据，直接使用redis是不合理的。

    for (User user: users){
        user.setAddress(getAddressMapFromRedis.get(key));
    }

##### 2. 定义为方法变量，每次使用方法时加载，

好吧，使用频繁的数据，我先把整个map都提取出来，降低redis的访问次数。

    Map<String,String> addressMap = getAddressMapFromRedis();
    for (User user: users){
        user.setAddress(addressMap.get(key));
    }
    
利：解决了在循环内每次访问数据都要通过redis

弊：多个客户端使用这个方法时这个map会多次加载，   
   如果map数据量大的话，并发量比较高的时候，内存的使用量会成直线上升。

##### 3. 定义为类变量，定时更新

将类定义为单例的，这样的话，数据就会只加载一次，并且定时更新。

    Cache<String, Object> cache = CacheBuilder.newBuilder().expireAfterWrite(1, TimeUnit.HOURS).build();
    private Map<String, String> getAddressMap(){
        try {
            Object addr = cache.get("address", new Callable<Map<String, String>>() {
                @Override
                public Map<String, String> call() throws Exception {
                    return getAddressMapFromRedis();
                }
            });
            return (Map<String, String>) addr;
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
        return getAddressMapFromRedis();
    }
    
如果每次要用的时候定义到类里，也会造成多个地方资源重复。

##### 4. 定义为整个项目的公用方法，定时更新

* 也就是将这块规划好，独立出来，提供给其他共同使用。
* 相当于做了两级缓存，**一级为JVM，二级为redis.**
* 这里使用了google guava的带有过期机制的Cache.
* 根据不同的使用场景，数据大小以及使用频繁度定义好各级缓存的过期时间。
* 当然如果数据量较小，访问不怎么频繁的数据就可以不使用JVM缓存了。