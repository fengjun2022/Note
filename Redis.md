# Redis





![image-20230829211750117](C:\Users\32784\Desktop\笔记\image\image-20230829211750117.png)



## 启动

![image-20230829212657654](C:\Users\32784\Desktop\笔记\image\image-20230829212657654.png)



![image-20230829213438109](C:\Users\32784\Desktop\笔记\image\image-20230829213438109.png)

```

redis-cli.exe -h localhost -p 6379 -a 123456
#-h 地址  -p端口 -a密码
```



## Redis数据类型

![image-20230829215128497](C:\Users\32784\Desktop\笔记\image\image-20230829215128497.png)



![image-20230829215908366](C:\Users\32784\Desktop\笔记\image\image-20230829215908366.png)





##  字符串操作命令

![image-20230829221624986](C:\Users\32784\Desktop\笔记\image\image-20230829221624986.png) 





## 哈希操作命令

![image-20230829222544955](C:\Users\32784\Desktop\笔记\image\image-20230829222544955.png)

![image-20230829223343574](C:\Users\32784\Desktop\笔记\image\image-20230829223343574.png)





## 列表操作命令

![image-20230829223420282](C:\Users\32784\Desktop\笔记\image\image-20230829223420282.png) 





##  集合操作命令

![image-20230829224906841](C:\Users\32784\Desktop\笔记\image\image-20230829224906841.png)





## 有序集合操作命令

![image-20230829225802178](C:\Users\32784\Desktop\笔记\image\image-20230829225802178.png)







## 通用命令

![image-20230829230504805](C:\Users\32784\Desktop\笔记\image\image-20230829230504805.png)















## Spring Data Redis使用方式

![image-20230830150719911](C:\Users\32784\Desktop\笔记\image\image-20230830150719911.png)





pom引入

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

```





yml配置 与datasource平级

```yml
  redis:
    host: ${sky.redis.host}
    port: ${sky.redis.port}
    password: ${sky.redis.password}
    database: ${sky.redis.database}
    
    
      redis:
     host: localhost
     port: 6379
     password: 123456
     database: 0
```



配置类配置

```java
@Configuration
@Slf4j
public class RedisConfiguration {
    @Bean
    public RedisTemplate redisTemplate (RedisConnectionFactory redisConnectionFactory){
        RedisTemplate redisTemplate = new RedisTemplate();
        // 设置redos连接工厂对象
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        // 设置redis中key的序列化器
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}

```





```java
    public  void  test(){
        System.out.println(redisTemplate);
        // 操作字符串对象
        ValueOperations valueOperations = redisTemplate.opsForValue();
        // 操作哈希对象
        HashOperations hashOperations = redisTemplate.opsForHash();
        // 获得集合
        ListOperations listOperations = redisTemplate.opsForList();
        // 获得zset
        ZSetOperations zSetOperations = redisTemplate.opsForZSet();

    }
```









## Spring Cache

![image-20231010203546479](C:\Users\32784\Desktop\笔记\image\image-20231010203546479.png)



![image-20231010203619091](C:\Users\32784\Desktop\笔记\image\image-20231010203619091.png)



@Cacheble用法

![image-20231010210713966](C:\Users\32784\Desktop\笔记\image\image-20231010210713966.png)

@CachePut用法

![image-20231010204646249](C:\Users\32784\Desktop\笔记\image\image-20231010204646249.png)

@CacheEvict用法

![image-20231010210655053](C:\Users\32784\Desktop\笔记\image\image-20231010210655053.png)



![image-20231011220117474](C:\Users\32784\Desktop\笔记\image\image-20231011220117474.png)





```pom
   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
```

