
## Pom依赖

### Maven模块Servlet-Pom依赖

```xml
<!-- 数据库连接池 -->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.21</version>
</dependency>
<!-- 链接MySQL数据库 -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.15</version>
</dependency>

<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope>
</dependency>

<dependency>
  <groupId>javax</groupId>
  <artifactId>javaee-web-api</artifactId>
  <version>6.0</version>
  <scope>provided</scope>
</dependency>

<dependency>
  <groupId>jstl</groupId>
  <artifactId>jstl</artifactId>
  <version>1.2</version>
</dependency>
```



---

### Thymeleaf-Pom依赖

```xml
xmlns:th="http://www.http://www.thymeleaf.org"
<!-- Thymeleaf -->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf</artifactId>
    <version>3.0.11.RELEASE</version>
</dependency>
```



---

### Jackson JSON API
```xml
<dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.11.0</version>
</dependency>
```




---

## Utils工具类

### DBUtils

```java
import com.alibaba.druid.pool.DruidDataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.util.Properties;

/**
 * @author orange
 * @create 2020-07-21 1:45 下午
 */
public class DBUtils {
    private static DruidDataSource ds;
    static {
        Properties p = new Properties();
        InputStream ips = DBUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
        //让对象和输入流关联
        try{
            p.load(ips);
        }catch (IOException e){
            e.printStackTrace();
        }
        //开始读取数据
        String driver = p.getProperty("db.driver");
        String url = p.getProperty("db.url");
        String name = p.getProperty("db.username");
        String password = p.getProperty("db.password");
        //读取最大链接数量和初始链接数量
        //从配置文件中 只能读取字符串
        String maxSize = p.getProperty("db.maxActive");
        String initSize = p.getProperty("db.initialSize");
        //创建链接池对象
        ds = new DruidDataSource();
        //设置链接信息
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(name);
        ds.setPassword(password);
        //设置初始和最大数值
        ds.setInitialSize(Integer.parseInt(initSize));
        ds.setMaxActive(Integer.parseInt(maxSize));
    }
    public static Connection getConn() throws Exception {
        return ds.getConnection();
    }
}

```



---

### ThUtils

```java
import org.thymeleaf.TemplateEngine;
import org.thymeleaf.context.Context;
import org.thymeleaf.templateresolver.ClassLoaderTemplateResolver;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author orange
 * @create 2020-07-22 10:42 上午
 */
public class ThUtils {
    private static TemplateEngine te;
    static {
        //创建模版引擎对象
        te = new TemplateEngine();
        //创建解析器 该解析器会自动查找src/main/resources目录下的模版页面
        ClassLoaderTemplateResolver r = new ClassLoaderTemplateResolver();
        //设置字符集
        r.setCharacterEncoding("utf-8");
        //让解析器和模版引擎关联
        te.setTemplateResolver(r);
    }
    //Context导包org.thymeleaf.Context
    public static void print(String fileName, Context context, HttpServletResponse response) throws IOException {
        //将页面和数据整合到一起到一个新的html字符串
        String html = te.process(fileName,context);
        //把得到的新的html返回给浏览器
        response.setContentType("text/html;charset=utf-8");
        PrintWriter pw = response.getWriter();
        pw.print(html);
        pw.close();
    }
}

```

---

## resources文件

### jdbc.properties

```properties
db.driver=com.mysql.cj.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/newdb3?characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai&rewriteBatchedStatements=true
db.username=root
db.password=root
db.maxActive=10
db.initialSize=2
```



---

## 创建工程分包

| 包名              | 分类                |
| :---------------- | :------------------ |
| cn.xxx.utils      | 工程中所有的工具类  |
| cn.xxx.controller | 工程中所有的Servlet |
| cn.xxx.dao        | 工程中所有的Dao     |
| cn.xxx.entity     | 工程中所有的实体类  |

---

