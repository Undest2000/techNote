 

# Shiro

认证登录的流程：

1拿到Subject门面对象

2调用isAuthenticate()方法来判断用户是否登录

3登录了就进入应用

4未登录就将用户输入的用户名和密码封装成一个token

5Subject调用login(token)方法

6找到我们自定义的realm

7调用里面的getAuthenticationInfo()方法拿到一个AuthenticationInfo对象

8调用CredentialsMatcher中的doCredentialsMatch方法将info和token进行对比

9有多种对比模式 简单对比、加盐对比 。。

10对比通过了仍要判断多个realm通过的策略是什么，是只要有一个比对通过就登录，还是

全部比对通过就登录？还是。。。

11满足的这些策略后就放行登录成功

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-15-09-38-29-image.png)

<img title="" src="file:///C:/Users/fengying/AppData/Roaming/marktext/images/2022-06-15-09-39-26-image.png" alt="" width="839">

```java
//1、获取SecurityManager工厂，此处使用Ini配置文件初始化SecurityManager     
Factory<org.apache.shiro.mgt.SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");

//2、得到SecurityManager实例    
org.apache.shiro.mgt.SecurityManager securityManager = factory.getInstance();

//3、设定全局的securityManager
SecurityUtils.setSecurityManager(securityManager);
```

代码接上的：

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-15-09-41-09-image.png)

自定义过滤器要注意：

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-15-11-32-57-image.png)

shiroFilter(在进入servlet之前)->interceptor(在进入controller之前)

两边都要放行过去

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-15-15-04-42-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-15-15-05-12-image.png)

```javascript
<!--引入弹窗组件start-->
<script type="text/javascript" src="plugins/attention/zDialog/zDrag.js"></script>
<script type="text/javascript" src="plugins/attention/zDialog/zDialog.js"></script>
<!--引入弹窗组件end-->
```

总结：subject.login（token）实际上调用了securityManager.login(subject,token)方法

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-01-09-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-02-23-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-02-40-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-04-14-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-05-09-image.png)

single:

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-06-39-image.png)

multi:

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-06-51-image.png)

到AuthenticatingRealm里了：

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-24-57-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-16-11-10-41-image.png)

（1）爷爷类 （2）父类 （3）子类

常用的AuthorizingRealm(2)继承了AuthenticatingRealm(1)，我们又继承了AuthorizingRealm(2)

，所以我们自定义的realm(3)里制定的credentialsMatcher就会一步步往上走

doGetAuthenticationInfo（）方法

声明在AuthenticatingRealm（1）里,

doGetAuthorizationInfo（）方法

声明在AuthorizingRealm（2）里

都是抽象方法

需要我们自己实现

通过验证的PRIN

我们在自定义Realm的doGetAuthorizationInfo（）方法里查询数据库拿到用户的角色和权限后给到

```java
simpleAuthorizationInfo
```

```java
 for (Role role : user.getRoles()) {
            //添加角色
            simpleAuthorizationInfo.addRole(role.getRoleName());
            //添加权限
            for (Permissions permissions : role.getPermissions()) {
                simpleAuthorizationInfo.addStringPermission(permissions.getPermissionsName());
            }
        }
        return simpleAuthorizationInfo;
```

完整代码：

```java
package com.wsl.shiro;

import com.wsl.bean.Permissions;
import com.wsl.bean.Role;
import com.wsl.bean.User;
import com.wsl.service.LoginService;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.util.StringUtils;

public class CustomRealm extends AuthorizingRealm {

    @Autowired
    private LoginService loginService;

/**      
@MethodName doGetAuthorizationInfo     
@Description 权限配置类     
@Param [principalCollection]     
@Return AuthorizationInfo     
@Author WangShiLin     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        //获取登录用户名
        String name = (String) principalCollection.getPrimaryPrincipal();
        //查询用户名称
        User user = loginService.getUserByName(name);
        //添加角色和权限
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
        for (Role role : user.getRoles()) {
            //添加角色
            simpleAuthorizationInfo.addRole(role.getRoleName());
            //添加权限
            for (Permissions permissions : role.getPermissions()) {
                simpleAuthorizationInfo.addStringPermission(permissions.getPermissionsName());
            }
        }
        return simpleAuthorizationInfo;
    }

/**   
@MethodName doGetAuthenticationInfo    
@Description 认证配置类     
@Param [authenticationToken]     
@Return AuthenticationInfo     
@Author WangShiLin     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        if (StringUtils.isEmpty(authenticationToken.getPrincipal())) {
            return null;
        }
        //获取用户信息
        String name = authenticationToken.getPrincipal().toString();
        User user = loginService.getUserByName(name);
        if (user == null) {
            //这里返回后会报出对应异常
            return null;
        } else {
            //这里验证authenticationToken和simpleAuthenticationInfo的信息
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(name, user.getPassword().toString(), getName());
            return simpleAuthenticationInfo;
        }
    }
}
```
