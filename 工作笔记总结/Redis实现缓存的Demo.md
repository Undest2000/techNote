一个缓存实现的Demo

Redis工具类:

```java
package com.example.demo.common.jedis;

import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

import javax.annotation.Resource;

import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;


//包装RedisTemplate的工具类
@Component
public class RedisUtil {
@Resource
private RedisTemplate<String, Object> redisTemplate;

public Set<String> keys(String keys) {
    try {
        return redisTemplate.keys(keys);
    } catch (Exception e) {
        e.printStackTrace();
        return null;
    }
}

/**
 * 指定缓存失效时间
 *
 * @param key  键
 * @param time 时间(秒)
 * @return
 */
public boolean expire(String key, long time) {
    try {
        if (time > 0) {
            redisTemplate.expire(key, time, TimeUnit.SECONDS);
        }
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 根据key 获取过期时间
 *
 * @param key 键 不能为null
 * @return 时间(秒) 返回0代表为永久有效
 */
public long getExpire(String key) {
    return redisTemplate.getExpire(key, TimeUnit.SECONDS);
}

/**
 * 判断key是否存在
 *
 * @param key 键
 * @return true 存在 false不存在
 */
public boolean hasKey(String key) {
    try {
        return redisTemplate.hasKey(key);
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 删除缓存
 *
 * @param key 可以传一个值 或多个
 */
@SuppressWarnings("unchecked")
public void del(String... key) {
    if (key != null && key.length > 0) {
        if (key.length == 1) {
            redisTemplate.delete(key[0]);
        } else {
            redisTemplate.delete((Collection<String>) CollectionUtils.arrayToList(key));
        }
    }
}

/**
 * 普通缓存获取
 *
 * @param key 键
 * @return 值
 */
public Object get(String key) {
    return key == null ? null : redisTemplate.opsForValue().get(key);
}

/**
 * 普通缓存放入
 *
 * @param key   键
 * @param value 值
 * @return true成功 false失败
 */
public boolean set(String key, Object value) {
    try {
        redisTemplate.opsForValue().set(key, value);
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 普通缓存放入并设置时间
 *
 * @param key   键
 * @param value 值
 * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
 * @return true成功 false 失败
 */
public boolean set(String key, Object value, long time) {
    try {
        if (time > 0) {
            redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
        } else {
            set(key, value);
        }
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 递增
 *
 * @param key   键
 * @param delta 要增加几(大于0)
 * @return
 */
public long incr(String key, long delta) {
    if (delta < 0) {
        throw new RuntimeException("递增因子必须大于0");
    }
    return redisTemplate.opsForValue().increment(key, delta);
}

/**
 * 递减
 *
 * @param key   键
 * @param delta 要减少几(小于0)
 * @return
 */
public long decr(String key, long delta) {
    if (delta < 0) {
        throw new RuntimeException("递减因子必须大于0");
    }
    return redisTemplate.opsForValue().increment(key, -delta);
}

/**
 * HashGet
 *
 * @param key  键 不能为null
 * @param item 项 不能为null
 * @return 值
 */
public Object hget(String key, String item) {
    return redisTemplate.opsForHash().get(key, item);
}

/**
 * 获取hashKey对应的所有键值
 *
 * @param key 键
 * @return 对应的多个键值
 */
public Map<Object, Object> hmget(String key) {
    return redisTemplate.opsForHash().entries(key);
}

/**
 * HashSet
 *
 * @param key 键
 * @param map 对应多个键值
 * @return true 成功 false 失败
 */
public boolean hmset(String key, Map<String, Object> map) {
    try {
        redisTemplate.opsForHash().putAll(key, map);
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * HashSet 并设置时间
 *
 * @param key  键
 * @param map  对应多个键值
 * @param time 时间(秒)
 * @return true成功 false失败
 */
public boolean hmset(String key, Map<String, Object> map, long time) {
    try {
        redisTemplate.opsForHash().putAll(key, map);
        if (time > 0) {
            expire(key, time);
        }
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 向一张hash表中放入数据,如果不存在将创建
 *
 * @param key   键
 * @param item  项
 * @param value 值
 * @return true 成功 false失败
 */
public boolean hset(String key, String item, Object value) {
    try {
        redisTemplate.opsForHash().put(key, item, value);
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 向一张hash表中放入数据,如果不存在将创建
 *
 * @param key   键
 * @param item  项
 * @param value 值
 * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
 * @return true 成功 false失败
 */
public boolean hset(String key, String item, Object value, long time) {
    try {
        redisTemplate.opsForHash().put(key, item, value);
        if (time > 0) {
            expire(key, time);
        }
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 删除hash表中的值
 *
 * @param key  键 不能为null
 * @param item 项 可以使多个 不能为null
 */
public void hdel(String key, Object... item) {
    redisTemplate.opsForHash().delete(key, item);
}

/**
 * 判断hash表中是否有该项的值
 *
 * @param key  键 不能为null
 * @param item 项 不能为null
 * @return true 存在 false不存在
 */
public boolean hHasKey(String key, String item) {
    return redisTemplate.opsForHash().hasKey(key, item);
}

/**
 * hash递增 如果不存在,就会创建一个 并把新增后的值返回
 *
 * @param key  键
 * @param item 项
 * @param by   要增加几(大于0)
 * @return
 */
public double hincr(String key, String item, double by) {
    return redisTemplate.opsForHash().increment(key, item, by);
}

/**
 * hash递减
 *
 * @param key  键
 * @param item 项
 * @param by   要减少记(小于0)
 * @return
 */
public double hdecr(String key, String item, double by) {
    return redisTemplate.opsForHash().increment(key, item, -by);
}

/**
 * 根据key获取Set中的所有值
 *
 * @param key 键
 * @return
 */
public Set<Object> sGet(String key) {
    try {
        return redisTemplate.opsForSet().members(key);
    } catch (Exception e) {
        e.printStackTrace();
        return null;
    }
}

/**
 * 根据value从一个set中查询,是否存在
 *
 * @param key   键
 * @param value 值
 * @return true 存在 false不存在
 */
public boolean sHasKey(String key, Object value) {
    try {
        return redisTemplate.opsForSet().isMember(key, value);
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 将数据放入set缓存
 *
 * @param key    键
 * @param values 值 可以是多个
 * @return 成功个数
 */
public long sSet(String key, Object... values) {
    try {
        return redisTemplate.opsForSet().add(key, values);
    } catch (Exception e) {
        e.printStackTrace();
        return 0;
    }
}

/**
 * 将set数据放入缓存
 *
 * @param key    键
 * @param time   时间(秒)
 * @param values 值 可以是多个
 * @return 成功个数
 */
public long sSetAndTime(String key, long time, Object... values) {
    try {
        Long count = redisTemplate.opsForSet().add(key, values);
        if (time > 0) {
            expire(key, time);
        }
        return count;
    } catch (Exception e) {
        e.printStackTrace();
        return 0;
    }
}

/**
 * 获取set缓存的长度
 *
 * @param key 键
 * @return
 */
public long sGetSetSize(String key) {
    try {
        return redisTemplate.opsForSet().size(key);
    } catch (Exception e) {
        e.printStackTrace();
        return 0;
    }
}

/**
 * 移除值为value的
 *
 * @param key    键
 * @param values 值 可以是多个
 * @return 移除的个数
 */
public long setRemove(String key, Object... values) {
    try {
        Long count = redisTemplate.opsForSet().remove(key, values);
        return count;
    } catch (Exception e) {
        e.printStackTrace();
        return 0;
    }
}
// ===============================list=================================

/**
 * 获取list缓存的内容
 *
 * @param key   键
 * @param start 开始
 * @param end   结束 0 到 -1代表所有值
 * @return
 */
public List<Object> lGet(String key, long start, long end) {
    try {
        return redisTemplate.opsForList().range(key, start, end);
    } catch (Exception e) {
        e.printStackTrace();
        return null;
    }
}

/**
 * 获取list缓存的长度
 *
 * @param key 键
 * @return
 */
public long lGetListSize(String key) {
    try {
        return redisTemplate.opsForList().size(key);
    } catch (Exception e) {
        e.printStackTrace();
        return 0;
    }
}

/**
 * 通过索引 获取list中的值
 *
 * @param key   键
 * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
 * @return
 */
public Object lGetIndex(String key, long index) {
    try {
        return redisTemplate.opsForList().index(key, index);
    } catch (Exception e) {
        e.printStackTrace();
        return null;
    }
}

/**
 * 将list放入缓存
 *
 * @param key   键
 * @param value 值
 * @return
 */
public boolean lSet(String key, Object value) {
    try {
        redisTemplate.opsForList().rightPush(key, value);
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 将list放入缓存
 *
 * @param key   键
 * @param value 值
 * @param time  时间(秒)
 * @return
 */
public boolean lSet(String key, Object value, long time) {
    try {
        redisTemplate.opsForList().rightPush(key, value);
        if (time > 0) {
            expire(key, time);
        }
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 将list放入缓存
 *
 * @param key   键
 * @param value 值
 * @return
 */
public boolean lSet(String key, List<Object> value) {
    try {
        redisTemplate.opsForList().rightPushAll(key, value);
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 将list放入缓存
 *
 * @param key   键
 * @param value 值
 * @param time  时间(秒)
 * @return
 */
public boolean lSet(String key, List<Object> value, long time) {
    try {
        redisTemplate.opsForList().rightPushAll(key, value);
        if (time > 0) {
            expire(key, time);
        }
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 根据索引修改list中的某条数据
 *
 * @param key   键
 * @param index 索引
 * @param value 值
 * @return
 */
public boolean lUpdateIndex(String key, long index, Object value) {
    try {
        redisTemplate.opsForList().set(key, index, value);
        return true;
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
}

/**
 * 移除N个值为value
 *
 * @param key   键
 * @param count 移除多少个
 * @param value 值
 * @return 移除的个数
 */
public long lRemove(String key, long count, Object value) {
    try {
        Long remove = redisTemplate.opsForList().remove(key, count, value);
        return remove;
    } catch (Exception e) {
        e.printStackTrace();
        return 0;
    }
}
}
```

Controller类

```java
package com.example.demo.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.ResponseBody;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.example.demo.common.jedis.RedisUtil;
import com.example.demo.mapper.UserMapper;
import com.example.demo.pojo.User;

@Controller
public class UserController {

    @Autowired
    private UserMapper userMapper;
    @Autowired
    private RedisUtil redisUtil;

    //都加上ResponseBody防止不走试图解析器Springboot一直报没有模板的错误
    @PostMapping("/set")
    @ResponseBody
    public void set(@RequestBody User user){
        List<User> userList = userMapper.selectList(null);
        int flag = 0; //1已经有，0还没有
        for(User person:userList) {
            if(person.getId() == user.getId()) {
                System.out.println("There is already the same user!");
                redisUtil.hset("id", person.getId()+"", user);
                System.out.println("put the result to redis success!");
                flag = 1;
            }
        }
        if(flag == 0) {
            userMapper.insert(user);
            redisUtil.hdel("id", user.getId()+"");
            System.out.println("data save success!");
        }
    }

    @GetMapping("/get/{key}")
    @ResponseBody
    public User get(@PathVariable("key") String id) {
        try {
            User user = (User) redisUtil.hget("id", id);
            if (user != null) {
                System.out.println("read from redis success!");
                return user;
            }
        } catch (Exception e) {
            System.out.println("exception in reading from redis");
            e.printStackTrace();
        }
        System.out.println("the key is not in the redis");
        User user = userMapper.selectById(id);
        if (user != null) {
            System.out.println("put the result to redis...");
            try {
                redisUtil.hset("id", id, user);
                System.out.println("put the result to redis success!");
            } catch (Exception e) {
                System.out.println("exception in putting the result to the redis");
                e.printStackTrace();
            }
            System.out.println("read from mysql success!");
            return user;
        } else {
            System.out.println("the data does not exist");
        }
        return null;
    }
//这部分是我用来测试Mybatis-plus的条件构造器写的
    @GetMapping("/query/{id}")
    @ResponseBody
    public void  query(@PathVariable int id) {
        QueryWrapper<User> queryWrapper = new QueryWrapper<User>();
        queryWrapper.eq("id",id);
        User user = userMapper.selectOne(queryWrapper);
        System.out.println(user);
    }
}
```

Mapper类：

```java
package com.example.demo.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.demo.pojo.User;

public interface UserMapper extends BaseMapper<User> {

}
```

User类：

```java
package com.example.demo.pojo;


import java.io.Serializable;
//实现序列化接口才能存入Redis
public class User implements Serializable {
    /**
     * 
     */
    private static final long serialVersionUID = 2809352790942997573L;
    private int id;
    private String name;
    private int age;
    private String email;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getEmail() {
        return email;
    }
    public void setEmail(String email) {
        this.email = email;
    }
    public User(int id,String name,int age,String email) {
        this.id = id;
        this.age = age;
        this.name = name;
        this.email = email;
    }
    @Override
    public String toString() {
        return "User [id=" + id + ", name=" + name + ", age=" + age + ", email=" + email + "]";
    }

}
```

配置文件application.yml:

```yml
spring:
  redis:
    database: 0
    host: localhost
    port: 6379
    password: root
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/lin?Unicode=true&characterEncoding=utf8&autoReconnect=true&failOverReadOnly=false
    username: root
    password: root
server:
  port: 8089
```

表和记录的创建SQL语句

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
    id BIGINT(20) NOT NULL COMMENT '主键ID',
    name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
    age INT(11) NULL DEFAULT NULL COMMENT '年龄',
    email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY (id)
);
```

```sql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

maven依赖:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>11</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```
