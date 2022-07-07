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

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-21-15-03-48-image.png)

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

用Bootstrap-table做前端页面 实现分页

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
        integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <link rel="stylesheet" href="https://unpkg.com/bootstrap-table@1.20.2/dist/bootstrap-table.min.css">
</head>

<div class="container-fluid">
    <div class="panel panel-default">
        <div class="panel-heading">
            查询条件
        </div>
        <div class="panel-body form-group" style="margin-bottom:0px;">
            <label class="col-sm-1 control-label" style="text-align: right; margin-top:5px">姓名：</label>
            <div class="col-sm-2">
                <input type="text" class="form-control" name="userName" id="search_name" />
            </div>
            <label class="col-sm-1 control-label" style="text-align: right; margin-top:5px">年龄：</label>
            <div class="col-sm-2">
                <input type="text" class="form-control" name="age" id="search_age" />
            </div>
            <div class="col-sm-1 col-sm-offset-4">
                <button class="btn btn-primary" id="search_btn">查询</button>
            </div>

        </div>
        <table id="mytab" class="table table-hover "></table>
    </div>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"
        integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1"
        crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"
        integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"
        crossorigin="anonymous"></script>
    <script src="https://unpkg.com/bootstrap-table@1.20.2/dist/bootstrap-table.min.js"></script>
    <script>
        $('#mytab').bootstrapTable({
            method: 'get',
            url: "/fenye",//请求路径
            striped: true, //是否显示行间隔色
            pageNumber: 1, //初始化加载第一页
            pagination: true,//是否分页
            sidePagination: 'server',//server:服务器端分页|client：前端分页
            pageSize: 3,//单页记录数
            pageList: [5, 10, 20, 30],//可选择单页记录数
            showRefresh: true,//刷新按钮
            queryParams: function (params) {//上传服务器的参数
                var temp = {//如果是在服务器端实现分页，limit、offset这两个参数是必须的
                    limit: params.limit, // 每页显示数量
                    offset: params.offset, // SQL语句起始索引
                    page: (params.offset / params.limit) + 1, //当前页码 
                    userName: $('#search_name').val(),
                    age: $('#search_age').val()
                };
                return temp;
            },
            columns: [{
                field: 'checked',
                checkbox: true,
                align: 'center',
                valign: 'middle',
            }, {
                title: '用户名',
                field: 'userName',
            }, {
                title: '密码',
                field: 'password',
            },
            {
                title: '年龄',
                field: 'age',
                sortable: true
            }, {
                title: '角色',
                field: 'roleName',
            }, {
                title: '操作',
                field: 'id',
                formatter: operation,//对资源进行操作
            }]
        });
        //value代表该列的值，row代表当前对象
        //function formatSex(value, row, index) {
        //return value == 1 ? "男" : "女";
        //或者 return row.sex == 1 ? "男" : "女";
        //}
        //删除、编辑操作
        function operation(value, row, index) {
            var htm = "<button>删除</button><button>修改</button>"
            return htm;
        }
        //查询按钮事件
        $('#search_btn').click(function () {
            $('#mytab').bootstrapTable('refresh', {
                url: '/fenye'
            });
        })
    </script>
    </body>

</html>
```

```java
    @GetMapping("/fenye")
    @ResponseBody
    public MyPage<User> getUserListPage(User user) {
        if(user.getUserName().equals("")) {
            user.setUserName(null);
        }
        // 统计总记录数 page-current limit-size
        Map<String,Object> map = new HashMap<>();
        map.put("userName", user.getUserName());
        map.put("age",user.getAge());
        Page<User> userPage = userMapper.selectPage(new Page<User>(user.getPage(), user.getLimit()),
                new QueryWrapper<User>().allEq(map,false));
        MyPage<User> myPage = new MyPage<User>();
        myPage.setRows(userPage.getRecords());
        myPage.setTotal(userPage.getTotal());
        return myPage;
    }
```

```java
package com.example.demo.pojo;


import java.io.Serializable;
import com.baomidou.mybatisplus.annotation.TableField;
import com.example.demo.pojo.common.Page;

import lombok.AllArgsConstructor;


@AllArgsConstructor
public class User extends Page implements Serializable  {
    /**
     * 
     */
    private static final long serialVersionUID = 2809352790942997573L;

    private Long id;

    @TableField(value = "userName")
    private String userName;

    private String password;

    private Integer age;

    @TableField(value = "roleName")
    private String roleName;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }


}
```

```java
package com.example.demo.pojo.common;

import com.baomidou.mybatisplus.annotation.TableField;

/*
 * 这个类的主要作用是让User继承它 并且控制器一个变量就能接收完毕
 */
public class Page {

    //每页显示数量
    @TableField(exist=false)
    private int limit;

    //页码
    @TableField(exist=false)
    private int page;

    //sql语句起始索引
    @TableField(exist=false)
    private int offset;

    public int getLimit() {

        return limit;

    }

    public void setLimit(int limit) {

        this.limit = limit;

    }

    public int getPage() {

        return page;

    }

    public void setPage(int page) {

        this.page = page;

    }

    public int getOffset() {

        return offset;

    }

    public void setOffset(int offset) {

        this.offset = offset;

    }



}
```

返回前端的页面，要有total和rows两个属性

```java
package com.example.demo.pojo.common;

import java.util.ArrayList;

import java.util.List;

public class MyPage<T> {

    // 实体类集合

    private List<T> rows = new ArrayList<T>();

    // 数据总条数

    private long total;

    public MyPage() {

        super();

    }

    public List<T> getRows() {

        return rows;

    }

    public void setRows(List<T> rows) {

        this.rows = rows;

    }

    public long getTotal() {

        return total;

    }

    public void setTotal(long total) {

        this.total = total;

    }

}
```
