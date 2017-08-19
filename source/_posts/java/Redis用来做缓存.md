---
title: Redis用来做缓存
tags:
  - Java
categories:
  - Techonology
  - Java
date: 2016-07-01 14:56:12
---
Redis缓存 + Spring
<!-- more -->

***

### 引用
1. {% link Redis整合Spring结合使用缓存实例 http://blog.csdn.net/evankaka/article/details/50396325 Redis整合Spring结合使用缓存实例 %}
2. {% link Spring Data Redis简介以及项目Demo，RedisTemplate和 Serializer详解 http://www.cnblogs.com/edwinchen/p/3816938.html Spring Data Redis简介以及项目Demo，RedisTemplate和 Serializer详解 %}
3. {% link 深入理解Spring Redis的使用 http://www.cnblogs.com/luochengqiuse/p/4638988.html 深入理解Spring Redis的使用 %}


### Redis
Key-Value Nosql Database.

Redis数据库完全在内存中 --> 适合用作缓存

### Redis用作缓存实例
#### POM 引入jar包
{% codeblock lang:xml %}
<!--Redis start -->  
<dependency>  
    <groupId>org.springframework.data</groupId>  
    <artifactId>spring-data-redis</artifactId>  
    <version>1.6.1.RELEASE</version>  
</dependency>  

<dependency>  
    <groupId>redis.clients</groupId>  
    <artifactId>jedis</artifactId>  
    <version>2.7.3</version>  
</dependency>  
<!--Redis end -->  
{% endcodeblock %}

1. spring-data-redis是Spring官方推出，可以算是Spring框架集成Redis操作的一个子框架，封装了Redis的很多命令，可以很方便的使用Spring操作Redis数据库，Spring对很多工具都提供了类似的集成，如Spring Data MongDB…
2. Jedis是Redis官方推出的一款面向Java的客户端，提供了很多接口供Java语言调用。

这三个究竟有什么区别呢？可以简单的这么理解：
1. Redis是用ANSI C写的一个基于内存的Key-Value数据库；
2. Jedis是Redis官方推出的面向Java的Client，提供了很多接口和方法，可以让Java操作使用Redis；
3. Spring Data Redis是对Jedis进行了封装，集成了Jedis的一些命令和方法，可以与Spring整合。
4. 在后面的配置文件中可以看到，Spring是通过Jedis类来初始化connectionFactory的。

 
#### xml文件配置bean 

首先属性放在properties文件中
{% codeblock lang:css %}
#redis中心  
redis.host=10.75.202.11  
redis.port=6379  
redis.password=123456  
redis.maxIdle=100  
redis.maxActive=300  
redis.maxWait=1000  
redis.testOnBorrow=true  
redis.timeout=100000  
  
# 不需要加入缓存的类  
targetNames=xxxRecordManager,xxxSetRecordManager,xxxStatisticsIdentificationManager  
# 不需要缓存的方法  
methodNames=  
  
#设置缓存失效时间  
com.service.impl.xxxRecordManager= 60  
com.service.impl.xxxSetRecordManager= 60  
defaultCacheExpireTime=3600  
  
fep.local.cache.capacity =10000  
{% endcodeblock %}


{% codeblock lang:xml %}
<!-- jedis 配置 -->
<!-- 注意此处注入的是JedisPoolConfig，说明SDR还依赖与Jedis -->
<!-- 第一个poolconfig是对连接池的配置。包括最大连接数，队列数，存活时间，最大等待时间等等 -->
<bean id="poolConfig" class="redis.clients.jedis.JedisPoolConfig" >
      <property name="maxIdle" value="${redis.maxIdle}" />
      <property name="maxWaitMillis" value="${redis.maxWait}" />
      <property name="testOnBorrow" value="${redis.testOnBorrow}" />
</bean >

<!-- redis服务器中心 -->
<!-- Spring是通过Jedis类来初始化connectionFactory的 -->
<!-- 连接工厂，顾名思义，最基本的使用一定是对连接的打开和关闭。我们需要为其配置redis服务器的账户密码，端口号。 -->
<bean id="connectionFactory"  class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory" >
      <property name="poolConfig" ref="poolConfig" />
      <property name="port" value="${redis.port}" />
      <property name="hostName" value="${redis.host}" />
      <property name="password" value="${redis.password}" />
      <property name="timeout" value="${redis.timeout}" ></property>
</bean >

<!-- 这个类作为一个模版类，提供了很多快速使用redis的api，而不需要自己来维护连接，事务。 -->
<bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate" >
      <property name="connectionFactory" ref="connectionFactory" />
      <property name="keySerializer" >
          <bean class="org.springframework.data.redis.serializer.StringRedisSerializer" />
      </property>
      <!-- JdkSerializationRedisSerializer用于类的序列化 -->
      <property name="valueSerializer" >
          <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer" />
      </property>
</bean >

<!-- cache配置 -->
<bean id="methodCacheInterceptor" class="com.mucfc.msm.common.MethodCacheInterceptor" >
      <property name="redisUtil" ref="redisUtil" />
</bean >
<bean id="redisUtil" class="com.mucfc.msm.common.RedisUtil" >
      <property name="redisTemplate" ref="redisTemplate" />
</bean >
{% endcodeblock %}

- 为什么要使用Serializer
    * 因为redis是以key-value的形式将数据存在内存中，key就是简单的string，key似乎没有长度限制，不过原则上应该尽可能的短小且可读性强，无论是否基于持久存储，key在服务的整个生命周期中都会在内存中，因此减小key的尺寸可以有效的节约内存，同时也能优化key检索的效率。
    * value在redis中，存储层面仍然基于string，在逻辑层面，可以是string/set/list/map，不过redis为了性能考虑，使用不同的“encoding”数据结构类型来表示它们。(例如：linkedlist，ziplist等)。
    * 所以可以理解为，其实redis在存储数据时，都把数据转化成了byte[]数组的形式，那么在存取数据时，需要将数据格式进行转化，那么就要用到序列化和反序列化了，这也就是为什么需要配置Serializer的原因。

- Redistemplate
    + 通过Redistemplate可以调用ValueOperations和ListOperations等等方法，分别是对Redis命令的高级封装。

#### 业务service类
{% codeblock lang:java %}
public class DemoService{
    private static final Logger logger = LoggerFactory.getLogger(TTScenicRegionReadServiceClient.class); 
    
    private RedisManager redisManager;
    
    public CacheObject getObject(Long id){
        
        try{
            // 用id获取内存
            CacheObject obj = redisManager.getObjectCache(id);
            // 如果获取到了直接返回
            if(obj != null){
                return obj
            }
            // 如果获取不到 则用业务逻辑产生一个CacheObject
            // CacheObject objGetByService = 此处省略
            // 然后将objGetByService存入缓存中
            redisManager.setObjectCache(id, objGetByService);

            // 返回object
            return objGetByService;

        }catch (RedisException 
            ***
            Over！e) {
            // 处理exception 省略
        }
    }
}
{% endcodeblock %}

#### RedisManager接口
栗子：CacheObject 为被缓存的类
key为id

{% codeblock lang:java %}
public interface RedisManager {
    CacheObject getObjectCache(Long id) throws RedisException;
    void setObjectCache(Long id, Object data);
}
{% endcodeblock %}

#### RedisManager实现类


{% codeblock lang:java %}
public class RedisManagerImpl implements RedisManager {
    
    // 日志
    private static final Logger logger = LoggerFactory
        .getLogger(TTRedisManagerImpl.class);

    // RedisTemplate
    private RedisTemplate<String, Object> redisTemplate;

    public void setRedisTemplate(RedisTemplate<String, Object> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }
    
    CacheObject getObjectCache(Long id) throws RedisException{
        if( id == null ){
            return null;
        }
        ValueOperations<String, Object> valueOper = redisTemplate.opsForValue();
        
        Object obj = valueOper.get("CacheObjectCache_" + id);
        
        if (obj == null) {
            return null;
        }

        if (!(obj instanceof CacheObject)) {
            throw new RedisException(ErrorCodeContants.REDIS_DESERIALIZE_ERROR);
        }
        return (CacheObject) obj;
    }

    // 设置缓存，这是data应该是已经获取的object类
    void setObjectCache(Long id, Object data){
         if (id == null || data == null) {
            return ;
        }
    }

    ValueOperations<String, Object> valueOper = redisTemplate.opsForValue();
    
    valueOper.set("CacheObjectCache_" + id, data);
}
{% endcodeblock %}

通过Redistemplate可以调用ValueOperations和ListOperations等等方法，分别是对Redis命令的高级封装。

但是ValueOperations等等这些命令最终是要转化成为RedisCallback来执行的。也就是说通过使用RedisCallback可以实现更强的功能
***
Over！
























