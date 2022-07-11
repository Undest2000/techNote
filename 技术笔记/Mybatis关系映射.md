## Mybatis

### 一对一

设：一个老师对应一个班级 一个班级对应一个老师

```java
package one.to.one;

public class Teacher {
    private int tid;
    private String tname;
    private Classes classes;

    public int getTid() {
        return tid;
    }
    public void setTid(int tid) {
        this.tid = tid;
    }
    public String getTname() {
        return tname;
    }
    public void setTname(String tname) {
        this.tname = tname;
    }
    public Classes getClasses() {
        return classes;
    }
    public void setClasses(Classes classes) {
        this.classes = classes;
    }
    @Override
    public String toString() {
        return "Teacher [tid=" + tid + ", tname=" + tname + ", classes=" + classes + "]";
    }


}
```

```java
package one.to.one;

public class Classes {
    private int cid;
    private String cname;
    private Teacher teacher;

    public int getCid() {
        return cid;
    }
    public void setCid(int cid) {
        this.cid = cid;
    }
    public String getCname() {
        return cname;
    }
    public void setCname(String cname) {
        this.cname = cname;
    }
    public Teacher getTeacher() {
        return teacher;
    }
    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }
    @Override
    public String toString() {
        return "Classes [cid=" + cid + ", cname=" + cname + ", teacher=" + teacher + "]";
    }

}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="one.to.one.classesMapper">
    <!--
         方式一：嵌套结果：使用嵌套结果映射来处理重复的联合结果的子集
                 封装联表查询的数据(去除重复的数据)
         select * from classes c, teacher t where c.tid=t.tid and c.tid=#{tid}
     -->
    <select id="getClasses" resultMap="getClassesMap" parameterType="int">
        select * from classes c ,teacher t
            where c.tid=t.tid and c.tid=#{tid}
    </select>
    <resultMap type="one.to.one.Classes" id="getClassesMap">
        <id column="cid" property="cid"/>
        <result column="cname" property="cname"/>
        <association property="teacher" javaType="one.to.one.Teacher">
            <id column="tid" property="tid"></id>
            <result column="tname" property="tname"/>
        </association>
    </resultMap>
    <!--
         方式一：嵌套结果：使用嵌套结果映射来处理重复的联合结果的子集
                 封装联表查询的数据(去除重复的数据)
         select * from teacher t,classes c where t.cid = c.cid and t.cid=#{cid}
     -->
    <select id="getTeacher" resultMap="getTeacherMap" parameterType="int">
        select * from teacher t,classes c
            where t.cid = c.cid and t.cid=#{cid}
    </select>
    <resultMap type="one.to.one.Teacher" id="getTeacherMap">
        <id column="tid" property="tid"/>
        <result column="tname" property="tname"/>
        <association property="classes" javaType="one.to.one.Classes">
            <id column="cid" property="cid"/>
            <result column="cname" property="cname"/>
        </association>
    </resultMap>


    <!--
         方式二：嵌套查询：通过执行另外一个SQL映射语句来返回预期的复杂类型
         SELECT * FROM classes WHERE cid=1;
         SELECT * FROM teacher WHERE tid=1   //1 是上一个查询得到的tid的值
         property:别名(属性名)    column：列名 -->
          <!-- 把teacher的字段设置进去 -->
    <select id="getClasses2" resultMap="getClassesMap2">
        select * from classes c where c.cid = #{cid}
    </select>
    <resultMap type="one.to.one.Classes" id="getClassesMap2">
        <id column="cid" property="cid"/>
        <result column="cname" property="cname"/>
        <collection property="teacher" column="tid" select="getTeacherCollection">
        </collection>
    </resultMap>
    <select id="getTeacherCollection" resultType="one.to.one.Teacher">
        select tid tid,tname tname from teacher where tid=#{tid}
    </select>

</mapper>
```

一对多和多对多用的不多，再上网搜就行了
