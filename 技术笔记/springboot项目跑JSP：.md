### springboot项目跑JSP：

要在SPRINGBOOT项目里跑JSP：
1 新建一个WEBAPP
2 ECLIPSE里把它设为项目包
3 下面建JSP文件夹WEB-INF
4 下面建JSP文件
5 引入 tomcat-embed-jasper 和 jstl 的包
6 WEBAPP下可以放一个web.xml
7 配置文件里写试图解析器
spring:
 mvc:
  view:
   prefix: /WEB-INF/jsp
   suffix: .jsp

springmvc里的return注意：/寻找的是路径 不带/返回的是页面

两个div默认是各占一行的
同一行的话用bootstrap的row就可以 用col-sm-X来控制大小（加起来是12） 本质上是flex布局
如果不用就要设置为行内元素 inline 或者两者都用float
