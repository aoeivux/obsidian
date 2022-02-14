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

### 编写mybatis核心配置文件

```xml

<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
 "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <environments default="development">  
        2  
        <environment id="development">  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
                <property name="url" value="useSSL=true&amp;characterEncoding=UTF-8&amp;useUnicode=true&amp;serverTimezone=GMT"/>  
                <property name="username" value="root"/>  
                <property name="password" value="Atheonealone37."/>  
            </dataSource>  
        </environment>  
    </environments>  
    <mappers>  
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>  
    </mappers>  
</configuration>

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


## 3、测试


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



 ## 4、常错点

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
		
	