Mybatis-Plus实现物理分页

```java
    @GetMapping("/fenye")
    @ResponseBody
    public void fenye() {
        //不用写SQL，第一个参数是分页条件page，第二个是查询条件包装类对象
        Page<User> userPage = userMapper.selectPage(new Page<User>(1,3),
                 new QueryWrapper<User>()
                    .between("age",18,50)
            );
        //自定义XML分页,自己写SQL
        IPage<User> myPage = userMapper.selectPageVo(new Page<User>(2,3));
        System.out.println("MP自带分页-------------"+userPage.getRecords());
        System.out.println("XML自定义分页-------------"+myPage.getRecords());
    }
```

```java
public interface UserMapper extends BaseMapper<User> {
    public User getOneById(int id);

    IPage<User> selectPageVo(Page<?> page);//XML自己写分页
}
```

```xml
<mapper namespace="com.example.demo.mapper.UserMapper">
    <select id="getOneById" parameterType="int" resultType="com.example.demo.pojo.User">
        SELECT * FROM user WHERE id=#{id}
    </select>

    <select id="selectPageVo" resultType="com.example.demo.pojo.User">
        SELECT * FROM user WHERE age between 18 and 50
    </select>
</mapper>
```

```yml
mybatis-plus:
  mapper-locations: classpath*:mapper/*.xml
```

```java
@SpringBootApplication
@MapperScan("com.example.demo.mapper")
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

MP实现的分页是物理分页 实际上还是用limit来查询

物理查询：根据limit条件查询多次，内存负担小，服务器负担大

逻辑查询（内存查询）：一次性把所有数据都放到内存里再做筛选，内存负担大，服务器负担小

物理查询的实时性强，数据量过大的时候的优解（MP和MH都是）

逻辑查询适用于数据量不大而且变化不大的情况

MP多表的时候各个表一定要用别名as 要重新引入新的MP依赖
