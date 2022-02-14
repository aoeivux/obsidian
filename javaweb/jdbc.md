# JBBC

```java
package com.aoeivux.jdbc;  
  
import java.sql.*;  
  
public class TestJdbc {  
    public static void main(String[] args) throws ClassNotFoundException, SQLException {  
  
        //解决乱码问题和url配置  
 String url = "jdbc:mysql://localhost:3306/jdbc?userUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC&useSSL=false";  
        String userName = "root";  
        String passwd = "Atheonealone37.";  
  
        //加载驱动  
 Class.forName("com.mysql.cj.jdbc.Driver");  
        //连接数据库  
 Connection connection = DriverManager.getConnection(url,userName,passwd);  
        //require the statement to operate the mysql DB  
 Statement statement = connection.createStatement();  
  
        //编写sql语句  
 String sql = "select * FROM Users";  
  
       //执行sql语句  
 ResultSet rs = statement.executeQuery(sql);  
  
        while(rs.next()){  
            System.out.println("id="+rs.getObject("id"));  
            System.out.println("name="+rs.getObject("name"));  
            System.out.println("password="+rs.getObject("password"));  
            System.out.println("email="+rs.getObject("email"));  
            System.out.println("birthday="+rs.getObject("birthday"));  
            System.out.println("=======================");  
        }  
  
        //关闭资源，防止浪费  
 rs.close();  
        statement.close();  
        connection.close();  
  
    }  
}
```

# 预编译

```java
package com.aoeivux.jdbc;  
  
import java.sql.*;  
  
public class TestJdbc2 {  
    public static void main(String[] args) throws ClassNotFoundException, SQLException {  
        //解决乱码问题和url配置  
 String url = "jdbc:mysql://localhost:3306/jdbc?userUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC&useSSL=false";  
        String userName = "root";  
        String passwd = "Atheonealone37.";  
  
        //加载驱动  
 Class.forName("com.mysql.cj.jdbc.Driver");  
  
        Connection connection = DriverManager.getConnection(url,userName,passwd);  
  
        String sql = "insert into users (id, name, password, email, birthday) values (?,?,?,?,?)";  
  
        //预编译  
 PreparedStatement preparedStatement = connection.prepareStatement(sql);  
        preparedStatement.setInt(1,1212);  
        preparedStatement.setString(2,"zs");  
        preparedStatement.setString(3,"12345");  
        preparedStatement.setString(4,"GuiZhou");  
        preparedStatement.setDate(5,new Date(new java.util.Date().getTime()));  
  
        int i = preparedStatement.executeUpdate();  
          
        if (i > 0){  
            System.out.println("插入成功@");  
        }  
  
        preparedStatement.close();  
        connection.close();  
    }  
}
```


