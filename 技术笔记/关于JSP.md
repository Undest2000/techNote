### 关于JSP

JSP的标签头要记得：

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
 pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
```

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
<base href="<%=basePath%>">//改变JSP的初始化路径
```

一个参考的JSP模板

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + path + "/";
%>
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<base href="<%=basePath%>">
<!-- jsp文件头和头部 -->
<%@ include file="../system/admin/top.jsp"%>
<link rel="stylesheet" href="static/css/my.css" />
</head>
<body>
</body>
</html>


```
