### 关于JSP

JSP的标签头要记得

JSP是先翻译成SERVLET然后再编译SERVLET的
<%! %>声明
<% %>JAVA语句
<%= %>取值
<%@ %>定义
常用声明:

```java
<%
 String path = request.getContextPath();
 String basePath = request.getScheme() + "://"
 + request.getServerName() + ":" + request.getServerPort()
 + path + "/";
%>
<base href="<%=basePath%>">改变JSP的初始化路径
```
