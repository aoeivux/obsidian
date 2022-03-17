
ServletContent()

```java
package com.aoeivux.servlet;  
  
import javax.servlet.RequestDispatcher;  
import javax.servlet.ServletContext;  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import java.io.IOException;  
  
public class ServletDemo03 extends HttpServlet {  
    @Override  
 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ServletContext servletContext = this.getServletContext();  
        RequestDispatcher requestDispatcher = servletContext.getRequestDispatcher("/d2");  
        requestDispatcher.forward(req,resp); //forward()实现分组转发  
  
 }  
  
    @Override  
 protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}

```

## HttpResponse下载应用
* 获取下载路径
* 获取下载文件名
* 设置下载头信息resp.setHeader("Content-Disposition", "attachment;filename" + URLEncoder.encode(fileName,"UTF-8"))
* 获取输入流new FileInputStream();
* 获取输出流resp.getOutputStream();
* 创建缓存区int len = 0; byte[] buffer = ne wbyte[1024];
* 将OutPutStream()写入缓存区

```java
while ((len=in.read(buffer)) > 0) {
	out.write(buffer, 0, len);
}  

in.close();
out.close();

```


## Session会话自动过期
```xml
<session-config>  
  <session-timeout>5</session-timeout>  
</session-config>
```