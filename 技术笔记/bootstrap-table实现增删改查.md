## bootstrap-table实现增删改查

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

<div class="container">
    <div class="panel panel-default">
        <div class="panel-heading">
            查询条件
        </div>
        <div class="panel-body form-group" style="margin-bottom:0px;">
            <div class="row">
                <label class="col-sm-1 control-label" style="text-align: right; margin-top:5px">姓名：</label>
                <div class="col-sm-2">
                    <input type="text" class="form-control" name="search_name" id="search_name" />
                </div>
                <label class="col-sm-1 control-label" style="text-align: right; margin-top:5px">年龄：</label>
                <div class="col-sm-2">
                    <input type="text" class="form-control" name="search_age" id="search_age" />
                </div>
                <div class="col-sm-2">
                    <button class="btn btn-primary" id="search_btn">查询</button>
                    <button class='btn btn-primary' data-toggle="modal" data-target="#addModal">新增</button>
                </div>
            </div>
        </div>
        <button class="btn btn-primary" id="delete_btn" onclick="batchDelete()">批量删除</button>
        <table id="mytab" class="table table-hover "></table>
    </div>

    <!-- addModal -->
    <div>
        <div class="modal fade " id="addModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">新增用户</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">×</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        用户名：<input type="text" class="form-control" name="userName" id="userName" />
                        密码：<input type="text" class="form-control" name="password" id="password" />
                        年龄:<input type="text" class="form-control" name="age" id="age" />
                        <div style="height:100px;overflow:auto;display:inline-block;margin-top:20px">
                            角色:
                            <select id="roleSelect" class="form-select form-select-lg mb-3"
                                aria-label=".form-select-lg example">
                                <option selected value="admin">admin</option>
                                <option value="user">user</option>
                            </select>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button>
                            <button type="button" class="btn btn-primary" onclick="addUser()">保存</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- updateModal -->
        <div>
            <div class="modal fade " id="updateModal" tabindex="-1" aria-labelledby="exampleModalLabel"
                aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="exampleModalLabel">修改用户信息</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                <span aria-hidden="true">×</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <input type="hidden" class="form-control" name="id" id="Uid" />
                            用户名：<input type="text" class="form-control" name="userName" id="UuserName" />
                            密码：<input type="text" class="form-control" name="password" id="Upassword" />
                            年龄:<input type="text" class="form-control" name="age" id="Uage" />
                            <div style="height:100px;overflow:auto;display:inline-block;margin-top:20px">
                                角色:
                                <select id="UroleSelect" class="form-select form-select-lg mb-3"
                                    aria-label=".form-select-lg example">
                                    <option selected value="admin">admin</option>
                                    <option value="user">user</option>
                                </select>
                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button>
                                <button type="button" class="btn btn-primary" onclick="updateUser()">保存</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
            <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"
                integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1"
                crossorigin="anonymous"></script>
            <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"
                integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"
                crossorigin="anonymous"></script>
            <script src="https://unpkg.com/bootstrap-table@1.20.2/dist/bootstrap-table.min.js"></script>
            <script
                src="https://cdn.bootcdn.net/ajax/libs/bootstrap-table/1.20.2/bootstrap-table-locale-all.js"></script>
            <script>
                $('#mytab').bootstrapTable({
                    locale: 'zh-CN',
                    method: 'get',
                    url: "/selectUserByPage",//请求路径
                    striped: true, //是否显示行间隔色
                    pageNumber: 1, //初始化加载第一页
                    pagination: true,//是否分页
                    sidePagination: 'server',//server:服务器端分页|client：前端分页
                    pageSize: 5,//单页记录数
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
                //删除、编辑操作
                function operation(value, row, index) {
                    //因为row是一个object,要转成JSON字符串
                    var curRow = JSON.stringify(row);
                    var html = `
            <button class='btn btn-primary' onclick='deleteUser("${row.id}")'>删除</button>
            <button class='btn btn-primary' onclick='toModal(${curRow})' data-toggle='modal' data-target='#updateModal'>修改</button>`;
                                    return html;
                }
                //查询按钮事件
                $('#search_btn').click(function () {
                    $('#mytab').bootstrapTable('refresh', {
                        url: 'selectUserByPage'
                    });
                })
                //生成随机id
                function uuid() {
                    var s = [];
                    var hexDigits = "0123456789abcdef";
                    for (var i = 0; i < 36; i++) {
                        s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
                    }
                    s[14] = "4";  // bits 12-15 of the time_hi_and_version field to 0010
                    s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1);  // bits 6-7 of the clock_seq_hi_and_reserved to 01
                    s[8] = s[13] = s[18] = s[23] = "-";

                    var uuid = s.join("");
                    return uuid;
                }

                function addUser() {
                    var userName = $('#userName').val();
                    var password = $('#password').val();
                    var age = $('#age').val();
                    var roleName = $('#roleSelect').val();
                    var flag = true;
                    var errorMsg = null;
                    if (userName == null || $.trim(userName) == "") {
                        flag = false;
                        errorMsg = "用户名不能为空！"
                    }

                    if (flag) {
                        if (password == null || $.trim(password) == "") {
                            flag = false;
                            errorMsg = "密码不能为空！"
                        }
                    }

                    if (flag) {
                        if (age == null || $.trim(age) == "") {
                            flag = false;
                            errorMsg = "年龄不能为空！"
                        }
                    }

                    if (flag) {
                        $.ajax({
                            method: "get",
                            url: "/addUser",
                            data: {
                                userName: userName,
                                password: password,
                                age: age,
                                roleName: roleName
                            },
                            success: function (data) {
                                errorMsg = data;
                                alert(errorMsg);
                                $('#mytab').bootstrapTable('refresh', {
                                    url: '/selectUserByPage'
                                });
                                $('#addModal').modal('hide');
                            },
                            error: function () {
                                errorMsg = "添加用户失败！";
                                alert(errorMsg);
                            },
                        });
                    } else {
                        alert(errorMsg);
                    }
                }

                function deleteUser(id) {
                    var flag = confirm("确定要删除数据吗？");
                    if (flag) {
                        $.ajax({
                            method: "get",
                            url: "/deleteUser",
                            data: {
                                id: id,
                            },
                            success: function (data) {
                                errorMsg = data;
                                alert(errorMsg);
                                $('#mytab').bootstrapTable('refresh', {
                                    url: '/selectUserByPage'
                                });
                                $('#addModal').modal('hide');
                            },
                            error: function () {
                                errorMsg = "删除用户失败！";
                                alert(errorMsg);
                            },
                        });
                    }
                }

                function updateUser() {
                    var flag = true;
                    if (flag) {
                        $.ajax({
                            method: "get",
                            url: "/updateUser",
                            data: {
                                userName: $('#UuserName').val(),
                                password: $('#Upassword').val(),
                                age: $('#Uage').val(),
                                roleName: $('#UroleSelect').val(),
                                id: $('#Uid').val()
                            },
                            success: function (data) {
                                errorMsg = data;
                                alert(errorMsg);
                                $('#mytab').bootstrapTable('refresh', {
                                    url: '/selectUserByPage'
                                });
                                $('#updateModal').modal('hide');
                            },
                            error: function () {
                                errorMsg = "更新用户失败！";
                                alert(errorMsg);
                            },
                        });
                    } else {
                        alert(errorMsg);
                    }

                }

                function toModal(obj) {
                    $('#Uid').val(obj.id);
                    $('#UuserName').val(obj.userName);
                    $('#Upassword').val(obj.password);
                    $('#Uage').val(obj.age);
                    obj2 = document.getElementById('UroleSelect');
                    for (i = 0; i < obj2.length; i++) {
                        if (obj2[i].value == obj.roleName) {
                            obj2[i].selected = true;
                        }
                    }
                }

                function batchDelete() {
                    var rows = $("#mytab").bootstrapTable("getSelections");
                    var idArray = [];
                    for(var i = 0;i<rows.length;i++){
                        idArray.push(rows[i].id);
                    }
                    var flag = confirm("确定要删除数据吗？");
                    if (flag) {
                        $.ajax({
                            method: "post",
                            url: "/deleteUserBatch",
                            data: {
                                idArray: idArray.toLocaleString(),
                            },
                            success: function (data) {
                                errorMsg = data;
                                alert(errorMsg);
                                $('#mytab').bootstrapTable('refresh', {
                                    url: '/selectUserByPage'
                                });
                                $('#addModal').modal('hide');
                            },
                            error: function () {
                                errorMsg = "删除用户失败！";
                                alert(errorMsg);
                            },
                        });
                    }
                }
            </script>
            </body>

</html>
```

```java
package com.example.demo.pojo;


import java.io.Serializable;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.example.demo.pojo.common.Page;

import lombok.AllArgsConstructor;


@AllArgsConstructor
public class User extends Page implements Serializable  {
    /**
     * 
     */
    private static final long serialVersionUID = 2809352790942997573L;

    @TableId(type=IdType.AUTO)
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
package com.example.demo.controller;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import lombok.extern.slf4j.Slf4j;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.util.StringUtils;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.example.demo.common.jedis.RedisUtil;
import com.example.demo.mapper.UserMapper;
import com.example.demo.pojo.User;
import com.example.demo.pojo.common.MyPage;
import org.springframework.web.servlet.ModelAndView;

@Controller
@Slf4j
public class UserController {
    private final static Logger logger = LoggerFactory.getLogger(UserController.class);
    @Autowired
    private UserMapper userMapper;
    @Autowired
    private RedisUtil redisUtil;

    @PostMapping("/set")
    @ResponseBody
    public void set(@RequestBody User user) {
        List<User> userList = userMapper.selectList(null);
        int flag = 0; // 1已经有，0还没有
        for (User person : userList) {
            if (person.getId() == user.getId()) {
                logger.info("已经有一个ID相同的用户了!");
                redisUtil.hset("id", person.getId() + "", user);
                System.out.println("成功把结果放入REDIS缓存!");
                flag = 1;
            }
        }
        if (flag == 0) {
            userMapper.insert(user);
            redisUtil.hdel("id", user.getId() + "");
            System.out.println("数据保存成功!");
        }
    }

    @GetMapping("/get/{key}")
    @ResponseBody
    public User get(@PathVariable("key") String id) {
        try {
            User user = (User) redisUtil.hget("id", id);
            redisUtil.expire("id", 60);
            if (user != null) {
                System.out.println("成功从REDIS缓存读取数据!");
                return user;
            }
        } catch (Exception e) {
            System.out.println("从REDIS缓存读取数据过程中发生异常！");
            e.printStackTrace();
        }
        System.out.println("REDIS中没有该数据缓存");
        User user = userMapper.selectById(id);
        if (user != null) {
            System.out.println("成功从MYSQL读取数据!");
            System.out.println("开始把数据存入REDIS缓存中...");
            try {
                redisUtil.hset("id", id, user);
                redisUtil.expire("id", 60);
                System.out.println("把数据存入REDIS缓存中成功!");
            } catch (Exception e) {
                System.out.println("把数据存入REDIS缓存过程中发生异常！");
                e.printStackTrace();
            }
            return user;
        } else {
            System.out.println("该数据不存在！");
        }
        return null;
    }

    @GetMapping("/query/{id}")
    @ResponseBody
    public void query(@PathVariable int id) {
        QueryWrapper<User> queryWrapper = new QueryWrapper<User>();
        queryWrapper.eq("id", id);
        User user = userMapper.selectOne(queryWrapper);
        System.out.println(user);
    }

    @GetMapping("/selectUserByPage")
    @ResponseBody
    public MyPage<User> getUserListPage(User user) {
        if(user.getUserName().equals("")) {
            user.setUserName(null);
        }
        // 统计总记录数 page-current limit-size
        Map<String,Object> map = new HashMap<>();
        map.put("age",user.getAge());
        Page<User> userPage = userMapper.selectPage(new Page<User>(user.getPage(), user.getLimit()),
                new QueryWrapper<User>().allEq(map,false).like(StringUtils.hasText(user.getUserName()),"userName", user.getUserName()));
        MyPage<User> myPage = new MyPage<User>();
        myPage.setRows(userPage.getRecords());
        myPage.setTotal(userPage.getTotal());
        return myPage;
    }

    @RequestMapping("/index")
    public ModelAndView index() {
        ModelAndView mv = new ModelAndView();
        mv.setViewName("index");
        return mv;
    }

    @GetMapping("/addUser")
    @ResponseBody
    public String addUser(User user) {
        try{
            List<User> userList = userMapper.selectList(null);
            for(User person:userList) {
                if(person.getUserName().equals(user.getUserName())) {
                    throw new Exception("已经存在一个相同用户名的用户了！");
                }
            }
            userMapper.insert(user);
        }catch (Exception e) {
            return "添加用户失败！原因是"+e.getMessage();
        }
        return "添加用户成功！";
    }

    @GetMapping("/deleteUser")
    @ResponseBody
    public String deleteUser(Long id) {
        try{
            userMapper.deleteById(id);
        }catch (Exception e) {
            return "删除用户失败！原因是"+e.getMessage();
        }
        return "删除用户成功！";
    }

    @PostMapping("/deleteUserBatch")
    @ResponseBody
    public String deleteUserBatch(@RequestParam(value = "idArray") List<Long> idArray) {
        try{
            userMapper.deleteBatchIds(idArray);
        }catch (Exception e) {
            return "删除用户失败！原因是"+e.getMessage();
        }
        return "删除用户成功！";
    }
    @GetMapping("/updateUser")
    @ResponseBody
    public String updateUser(User user) {
        try{
            userMapper.updateById(user);
        }catch (Exception e) {
            return "更新用户信息失败！原因是"+e.getMessage();
        }
        return "更新用户信息成功！";
    }
}
```

```java
package com.example.demo.configuration;

import java.util.List;

import org.springframework.context.annotation.Configuration;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.http.converter.json.MappingJackson2HttpMessageConverter;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.module.SimpleModule;
import com.fasterxml.jackson.databind.ser.std.ToStringSerializer;

/**
 * Created by nqq on 2019/1/17.
 * 解决后端Long型数据到前端JS精度丢失的问题
 */
@EnableWebMvc
@Configuration
public class WebJsonConverterConfig extends WebMvcConfigurerAdapter {
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        MappingJackson2HttpMessageConverter jackson2HttpMessageConverter = new MappingJackson2HttpMessageConverter();
        ObjectMapper objectMapper = new ObjectMapper();
        SimpleModule simpleModule = new SimpleModule();
        simpleModule.addSerializer(Long.class, ToStringSerializer.instance);
        simpleModule.addSerializer(Long.TYPE, ToStringSerializer.instance);
        objectMapper.registerModule(simpleModule);
        jackson2HttpMessageConverter.setObjectMapper(objectMapper);
        converters.add(jackson2HttpMessageConverter);
    }
}
```
