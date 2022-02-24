# 1、Mybatis初认识

## 1.1、什么是Mybatis

MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

![[temp/Pasted image 20220211193106.png]]


数据持久化
- IO文件持久化
- 数据库JDBjC
- 内存：``断电就没了``

## 1.2、为什么需要持久化
- 因为有一些对象因为需要，不能断掉
-  内存太贵

## 1.3、持久层

Dao层、Service层、Controller层



持久化是一种动作，而持久层是一种状态。


## 1.4、为什么需要Mybatis？

- 方便
- 传统的JDBC代码太复杂，简化框架，自动化
- 帮助将数据存入数据库中
- 优点：
	+ 简单
	+ 灵活
	+ sql和代码分离，提高可维护性
	+ 提供映射标签
	+ 提供对象关系映射标签
	+ 提供xml标签，支持编写动态sql



# 2、第一个Mybatis

思路：创建环境---》导入Mybatis---》编写代码---》测试

## 2.1、 搭建环境

### 2.1.1、搭建数据库
```sql
CREATE DATABASE	`mybatis`;

USE `mybatis`;

CREATE TABLE `user`(
 `id` INT(20) NOT NULL PRIMARY KEY,
 `name` VARCHAR (30) DEFAULT NULL,
 `pwd` VARCHAR (30) DEFAULT NULL
)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `user` (`id`, `name`, `pwd`) VALUES
(1,'zs','123456'),
(2,'li','123456'),
(3,'ww','123456')
```

### 2.1.2、新建项目

1、新建一个普通的Maven项目
2、删除src
3、导入依赖

## 2.2、 创建一个普通Maven模块

### 2.2.1、编写mybatis核心配置文件

```xml

<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
 "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <environments default="development">  
          
        <environment id="development">  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&characterEncoding=UTF-8&useUnicode=true&serverTimezone=GMT "/>  
                <property name="username" value="root"/>  
                <property name="password" value="Atheonealone37."/>  
            </dataSource>  
        </environment>  
    </environments>  

<!-- 重要: 每一个Mapper.xml都需要绑定mybatis核心配置文件mybatis-config.xml-->
    <mappers>  
        <mapper resource="com/aoeivux/dao/UserMapper.xml"/>  
    </mappers>  

</configuration>

```

### 2.2.2、通过db.properties外部配置values
```properties
username=root  
password=Atheonealone37.  
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&characterEncoding=UTF-8&useUnicode=true&serverTimezone=GMT  
driver=com.mysql.cj.jdbc.Driver
```

## 2.3、Maven导入相应的依赖
dependencies依赖
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0</modelVersion>  
  
  
             <!--父pom-->  
 <groupId>com.aoeivux</groupId>  
    <artifactId>mybatis</artifactId>  
    <packaging>pom</packaging>  
    <version>1.0-SNAPSHOT</version>  
    <modules>  
        <module>mybatis-01</module>  
    </modules>  
  
    <dependencies>  
        <!--单元测试-->  
 <dependency>  
            <groupId>junit</groupId>  
            <artifactId>junit</artifactId>  
            <version>4.12</version>  
        </dependency>  
  
        <!-- mysql -->  
 <dependency>  
            <groupId>mysql</groupId>  
            <artifactId>mysql-connector-java</artifactId>  
            <version>8.0.22</version>  
        </dependency>  
  
        <!--mybatis-->  
 <dependency>  
            <groupId>org.mybatis</groupId>  
            <artifactId>mybatis</artifactId>  
            <version>3.5.7</version>  
        </dependency>  
  
    </dependencies>  
  
</project>
```


## 2.4、新建一个子Module

## 2.5、编写实体类实现数据库的字段


```java
package com.aoeivux.pojo;  
  
public class User {  
    private int id;  
    private String name;  
    private String pwd;  
  
    public User(){}  
  
    public User(int id, String name, String pwd) {  
        this.id = id;  
        this.name = name;  
        this.pwd = pwd;  
    }  
  
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
  
    public String getPwd() {  
        return pwd;  
    }  
  
    public void setPwd(String pwd) {  
        this.pwd = pwd;  
    }  
  
    @Override  
 public String toString() {  
        return "User{" +  
                "id=" + id +  
                ", name='" + name + '\'' +  
                ", pwd='" + pwd + '\'' +  
                '}';  
    }  
  
}
```


## 2.6、编写dao类：dao接口和接口实现Mapper.xml
userDao接口
```java
package com.aoeivux.dao;  
   import com.aoeivux.pojo.User;  
  
import java.util.List;  
  
public interface UserDao {  
    List<User> getUser();  
}
```

userMapper.xml实现接口
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<!--绑定接口-->  
<mapper namespace="com.aoeivux.dao.UserDao">  
  
     <!--接口方法、返回类型-->  
 <select id="getUser" resultType="com.aoeivux.pojo.User">  
 select * from user; </select>  
  
</mapper>
```


## 2.7、 编写工具类防止重复使用提高效率

util--->MybatisUtil

```java
package com.aoeivux.util;  
  
import org.apache.ibatis.io.Resources;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;  
  
import java.io.IOException;  
import java.io.InputStream;  
  
public class MybatisUtil {  
  
    private static SqlSessionFactory sqlSessionFactory;  
  
    static{  
        try {  
            //使用mybatis的第一步，获取sqlSessionFactory  
 String resource = "mybatis-config.xml";  
            InputStream inputStream = Resources.getResourceAsStream(resource);  
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
    }  
    //返回sqlSession对象  
 public static SqlSession getSqlSession(){  
        return sqlSessionFactory.openSession();  
    }  
}
```


# 3、测试


与main目录同级
![[temp/Pasted image 20220214111005.png]]![[temp/Pasted image 20220214111015.png]]

## 3.1 、编写测试类
```java
package com.aoeivux.dao;  
  
import com.aoeivux.pojo.User;  
import com.aoeivux.util.MybatisUtil;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.junit.Test;  
  
import java.util.List;  
  
public class UserDaoTest {  
  
    @Test  
 public void test() {  
  
  
        SqlSession sqlSession = MybatisUtil.getSqlSession();  
        UserDao mapper = sqlSession.getMapper(UserDao.class);  
        List<User> user = mapper.getUser();  
  
        for (User userList : user) {  
            System.out.println(userList);  
        }  
  
        sqlSession.close();  
    }  
}
```



 # 4、常错点

 ### 1、 mapper未绑定

 每一个mapper.xml文件都需要在mybatis核心配置文件中注册！
在mybatis-config.xml中

```xml
<!--每一个mapper.xml文件都需要在mybatis核心配置文件中进行注册！-->  
 <mappers>  
        <mapper resource="com/aoeivux/dao/UserMapper.xml"/>  
    </mappers>
```



 ### 2、maven配置资源过滤问题
```xml
<!--在maven中配置资源过滤问题，防止资源导出错误-->  
 <build>  
        <resources>  
            <resource>  
                <directory>src/main/java</directory>  
                <includes>  
                    <include>**/*.properties</include>  
                    <include>**/*.xml</include>  
                </includes>  
                <filtering>true</filtering>  
            </resource>  
  
            <resource>  
                <directory>src/main/resources</directory>  
                <includes>  
                    <include>**/*.properties</include>  
                    <include>**/*.xml</include>  
                </includes>  
                <filtering>true</filtering>  
            </resource>  
        </resources>  
    </build>
```

### 3、编码错误问题

```java
java.lang.ExceptionInInitializerError
	at com.aoeivux.dao.UserDaoTest.test(UserDaoTest.java:17)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:50)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:47)
	at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
	at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:78)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:57)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:69)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater$1.execute(IdeaTestRunner.java:38)
	at com.intellij.rt.execution.junit.TestsRepeater.repeat(TestsRepeater.java:11)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:35)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:235)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:54)
Caused by: org.apache.ibatis.exceptions.PersistenceException: 
### Error building SqlSession.
### Cause: org.apache.ibatis.builder.BuilderException: Error creating document instance.  Cause: org.xml.sax.SAXParseException; lineNumber: 18; columnNumber: 7; 1 字节的 UTF-8 序列的字节 1 无效。
	at org.apache.ibatis.exceptions.ExceptionFactory.wrapException(ExceptionFactory.java:30)
	at org.apache.ibatis.session.SqlSessionFactoryBuilder.build(SqlSessionFactoryBuilder.java:80)
	at org.apache.ibatis.session.SqlSessionFactoryBuilder.build(SqlSessionFactoryBuilder.java:64)
	at com.aoeivux.util.MybatisUtil.<clinit>(MybatisUtil.java:20)
	... 25 more
Caused by: org.apache.ibatis.builder.BuilderException: Error creating document instance.  Cause: org.xml.sax.SAXParseException; lineNumber: 18; columnNumber: 7; 1 字节的 UTF-8 序列的字节 1 无效。
	at org.apache.ibatis.parsing.XPathParser.createDocument(XPathParser.java:263)
	at org.apache.ibatis.parsing.XPathParser.<init>(XPathParser.java:127)
	at org.apache.ibatis.builder.xml.XMLConfigBuilder.<init>(XMLConfigBuilder.java:82)
	at org.apache.ibatis.session.SqlSessionFactoryBuilder.build(SqlSessionFactoryBuilder.java:77)
	... 27 more
Caused by: org.xml.sax.SAXParseException; lineNumber: 18; columnNumber: 7; 1 字节的 UTF-8 序列的字节 1 无效。
	at com.sun.org.apache.xerces.internal.util.ErrorHandlerWrapper.createSAXParseException(ErrorHandlerWrapper.java:204)
	at com.sun.org.apache.xerces.internal.util.ErrorHandlerWrapper.fatalError(ErrorHandlerWrapper.java:178)
	at com.sun.org.apache.xerces.internal.impl.XMLErrorReporter.reportError(XMLErrorReporter.java:400)
	at com.sun.org.apache.xerces.internal.impl.XMLErrorReporter.reportError(XMLErrorReporter.java:306)
	at com.sun.org.apache.xerces.internal.impl.XMLDocumentFragmentScannerImpl$FragmentContentDriver.next(XMLDocumentFragmentScannerImpl.java:3156)
	at com.sun.org.apache.xerces.internal.impl.XMLDocumentScannerImpl.next(XMLDocumentScannerImpl.java:605)
	at com.sun.org.apache.xerces.internal.impl.XMLDocumentFragmentScannerImpl.scanDocument(XMLDocumentFragmentScannerImpl.java:507)
	at com.sun.org.apache.xerces.internal.parsers.XML11Configuration.parse(XML11Configuration.java:867)
	at com.sun.org.apache.xerces.internal.parsers.XML11Configuration.parse(XML11Configuration.java:796)
	at com.sun.org.apache.xerces.internal.parsers.XMLParser.parse(XMLParser.java:142)
	at com.sun.org.apache.xerces.internal.parsers.DOMParser.parse(DOMParser.java:247)
	at com.sun.org.apache.xerces.internal.jaxp.DocumentBuilderImpl.parse(DocumentBuilderImpl.java:339)
	at org.apache.ibatis.parsing.XPathParser.createDocument(XPathParser.java:261)
	... 30 more
Caused by: com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: 1 字节的 UTF-8 序列的字节 1 无效。
	at com.sun.org.apache.xerces.internal.impl.io.UTF8Reader.invalidByte(UTF8Reader.java:702)
	at com.sun.org.apache.xerces.internal.impl.io.UTF8Reader.read(UTF8Reader.java:568)
	at com.sun.org.apache.xerces.internal.impl.XMLEntityScanner.load(XMLEntityScanner.java:1905)
	at com.sun.org.apache.xerces.internal.impl.XMLEntityScanner.scanData(XMLEntityScanner.java:1385)
	at com.sun.org.apache.xerces.internal.impl.XMLScanner.scanComment(XMLScanner.java:801)
	at com.sun.org.apache.xerces.internal.impl.XMLDocumentFragmentScannerImpl.scanComment(XMLDocumentFragmentScannerImpl.java:1036)
	at com.sun.org.apache.xerces.internal.impl.XMLDocumentFragmentScannerImpl$FragmentContentDriver.next(XMLDocumentFragmentScannerImpl.java:2984)
	... 38 more

```

原因:

        由于在mybatis核心配置文件mybatis-config.xml和mapper.xml配置文件中有中文注释或者中文的字符

解决：

		将mybatis核心配置文件mybatis-config.xml和mapper.xml配置文件中的中文字符删除
		



# 5、CRUD增删改查

## 5.1、mapper   namespace 
namespace中绑定的包名要和dao/mapper中的接口名字一致，如图：
![[temp/Pasted image 20220214161641.png]]


![[temp/Pasted image 20220214161651.png]]




*注意: 增删改需要进行事务提交*
## 5.2 、insert
	增加插入语句：
```xml
	<insert id="insertUser" parameterType="com.aoeivux.pojo.User">  
insert into user (id, name, pwd) values (#{id}, #{name}, #{pwd});</insert>
```

## 5.3、delete
	删除语句：
```xml
<delete id="deleteUser" parameterType="int">  
 delete from USER where id = #{id}</delete>
```

## 5.4、update
	修改语句：
```xml
<update id="updateUser" parameterType="com.aoeivux.pojo.User">  
 update user set name = #{name}, pwd = #{pwd} where id = #{id}</update>
```

## 5.5、select
	查询全部用户语句：
```xml
<select id="getUser" resultType="com.aoeivux.pojo.User">  
 select * from user;</select>  
```


	查询指定条件用户：
```xml
<select id="getUserByID" parameterType="int" resultType="com.aoeivux.pojo.User">  
 select * from USER where id = #{id}</select>
```

## 5.6、万能的Map

```java
<select id="getUserMap" parameterType="map" resultType="com.aoeivux.pojo.User">  
 select * from USER where id = #{demoId}
 </select>
```
根据Map的键进行查找等操作，适合于字段多属性多的情况

## 5.7、模糊查询

*interface*
```java
//模糊查询  
List<User> userLike(String value);
```


*mapper.xml*

```xml
<select id="userLike" parameterType="String" resultType="com.aoeivux.pojo.User">  
 select * from user where name like #{value}
</select>
```

*test*
```java
@Test  
public void getUserLike(){  
    SqlSession sqlSession = MybatisUtil.getSqlSession();  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    List<User> users = mapper.userLike("%李%");  
  
    for (User user : users) {  
        System.out.println(user);  
    }  
}
```



 
## 5.8、小结

*层级目录*
![[temp/Pasted image 20220214174132.png]]


*参数问题*

- Mapper.xml语句中通过#{}传递参数，{}内的参数是pojo实体类里面的id、name、pwd

- where id = #{id} 中第一个id是数据库里面的id，第二个是pojo实体类里面的id


*步骤*

1.  DAO里面的UserMapper接口编写抽象类
```java
package com.aoeivux.dao;  
  
import com.aoeivux.pojo.User;  
  
import java.util.List;  
  
public interface UserMapper {  
  
    //查询全部用户  
 List<User> getUser();  
  
    //根据用户ID查询用户  
 User getUserByID(int id);  
  
    //插入一个用户  
 int insertUser(User user);  
  
    //修改一个User  
 int updateUser(User user);  
  
    //删除一个用户  
 int deleteUser(int id);  
  
}
```


2. UserMapper.xml指定接口与编写sql语句
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<mapper namespace="com.aoeivux.dao.UserMapper">  
  
    <select id="getUser" resultType="com.aoeivux.pojo.User">  
 select * from user; </select>  
  
  
    <select id="getUserByID" parameterType="int" resultType="com.aoeivux.pojo.User">  
 select * from USER where id = #{id} </select>  
  
    <insert id="insertUser" parameterType="com.aoeivux.pojo.User">  
 insert into user (id, name, pwd) values (#{id}, #{name}, #{pwd}); </insert>  
  
    <update id="updateUser" parameterType="com.aoeivux.pojo.User">  
 update user set name = #{name}, pwd = #{pwd} where id = #{id} </update>  
  
    <delete id="deleteUser" parameterType="int">  
 delete from USER where id = #{id} </delete>  
  
  
</mapper>
```


3. 测试类

```java
package com.aoeivux.dao;  
  
import com.aoeivux.pojo.User;  
import com.aoeivux.util.MybatisUtil;  
import org.apache.ibatis.session.SqlSession;  
import org.junit.Test;  
  
import java.util.List;  
  
public class UserDaoTest {  


//查询全部用户
    @Test  
 public void test() {  
  
        SqlSession sqlSession = MybatisUtil.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        List<User> user = mapper.getUser();  
  
        for (User userList : user) {  
            System.out.println(userList);  
        }  
  
        sqlSession.close();  
    }  


//查询指定ID用户
    @Test  
 public void getUserByID(){  
        SqlSession sqlSession = MybatisUtil.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
  
        User userByID = mapper.getUserByID(2);  
        System.out.println(userByID);  
  
        sqlSession.close();  
  
    }  


//增加插入新用户
    @Test  
 //insert  
 public void insertUser(){  
        SqlSession sqlSession = MybatisUtil.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        int res = mapper.insertUser(new User(5, "aoeivux", "1212121312128"));  
  
        if (res > 0)  
            System.out.println("insert ok!");  
  
        sqlSession.commit();  
        sqlSession.close();  
  
        }  


//修改用户指定字段语句
        @Test  
 public void updateUser(){  
        SqlSession sqlSession = MybatisUtil.getSqlSession();  
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
            int res = mapper.updateUser(new User(4, "zyx", "adsada"));  
            if (res > 0 ){  
                System.out.println("更新成功");  
            }else{  
                System.out.println("no");  
            }  
  
            sqlSession.commit();  
            sqlSession.close();  
  
        }  


//删除指定用户的语句
        @Test  
 public void deleteUser(){  
            SqlSession sqlSession = MybatisUtil.getSqlSession();  
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
  
            int i = mapper.deleteUser(5);  
  
            if( i > 0 ){  
                System.out.println("ok");  
            }  
  
            sqlSession.commit();  
            sqlSession.close();  
        }  
    }
```

	
# 6、配置解析
## 1、核心配置文件
*官网给出的配置结构：*
![[temp/Pasted image 20220214213924.png]]
 mybatis-config.xml 


## 2、环境配置(Enviroment)

## 3、 类型别名(typeAliases)
```xml
<typeAliases>  
 <typeAlias type="com.aoeivux.pojo.User" alias="User"/>  
</typeAliases>
```





# 7、日志
设置名：logImpl   

设置值： SLF4J
				 LOG4J(deprecated since 3.5.9) 
				 LOG4J2
				 JDK_LOGGING 
				 COMMONS_LOGGING 
				 STDOUT_LOGGING （日志工厂）
				 NO_LOGGING


## 7.1 日志工厂
```xml
<settings>  
 <setting name="logImpl" value="STDOUT_LOGGING"/>  
</settings>
```


# 8、分页
```java
select * from mybatis limit 0 2
```
- 使用java层面的代码
- 使用sql语句
- 使用插件
- 使用rowbounds



# 9、使用注解开发


## 9.1、面向接口编程
开发中，很多时候我们会选择面向接口编程

根本原因 :  解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好

在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的，对系统设计人员来讲就不那么重要了；

各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程

## 9.2、关于接口的理解
接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

接口的本身反映了系统设计人员对系统的抽象理解。

接口应有两类：

第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；

第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

一个体有可能有多个抽象面。抽象体与抽象面是有区别的。

## 9.3、三个面向区别

面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法。

面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现。

接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题。更多的体现就是对系统整体的架构。




















