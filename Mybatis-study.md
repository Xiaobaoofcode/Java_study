# Mybatis-9.28

**环境：**

- jdk15
- Mysql8.
- maven
- IDEA

**回顾：**

- JDBC
- Mysql
- Java基础
- Maven
- Junit

SSM框架：配置文件的，最好的方式：看官方文档

## 1、简介

### 1.1、什么事Mybatis

![image-20210201084529128](/Users/maczhen/Library/Application Support/typora-user-images/image-20210201084529128.png)

- MyBatis 是一款优秀的持久层框架。
- 它支持自定义 SQL、存储过程以及高级映射。
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
- MyBatis 本是apache的一个[开源项目](https://baike.baidu.com/item/开源项目/3406069)iBatis, 2010年这个[项目](https://baike.baidu.com/item/项目/477803)由apache software foundation 迁移到了[google code](https://baike.baidu.com/item/google code/2346604)，并且改名为MyBatis 。
- 2013年11月迁移到[Github](https://baike.baidu.com/item/Github/10145341)。

如何获得Mybatis？

- maven仓库

  ```java
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.6</version>
  </dependency>
  ```

- Github：https://github.com/mybatis/mybatis-3

- 中文文档：https://mybatis.org/mybatis-3/zh/index.html

### 1.2持久化

数据持久化:是将内存中的数据模型转换为存储模型，以及将存储模型转换为内存中的数据模型的统称。（例如文件的存储、数据的读取等都说数据持久化操作。数据模型可以是任何数据结构或对象模型，存储模型可以是关系模型，xml，二进制流等）

- 持久化就是将程序的数据持久状态和瞬时状态转化的过程
- 内存：断电即失
- 数据库：jdbc，io文件持久化
- 生活：冷藏、罐头

为什么需要持久化？

- 有一些对象，不能让他丢掉
- 内存太贵了

### 1.3持久层

Dao层、Service层和Controller层。。。

- 完成持久化的代码块
- 层界限十分明显

### 1.4为什么需要Mybatis？

- 方便
- 传统的jdbc代码太复杂，简化，框架和自动化。
- 不用Mybatis也可以，更容易上手，技术没有高低之分

## 2、第一个Mybatis程序

思路：搭建环境->导入Mybatis->编写代码->测试

### 2.1搭建环境

搭建数据库

```sql
create database mybatis；
use mybatis;

create table 'user'(
	'id' INT(20) NOT NULL PRIMARY KEY,
  'name' VARCHAR(30) DEFAULT NULL,
  'pwd' VARCHAR(30) DEFAULT NULL
)ENGINE=INNODB DEFAULT CHAREST=utf8;

INSERT INTO 'user'('id','name','pwd') values
(1,'zz你好','z1234'),
(2,'zz累计','z1234'),
(3,'zz累计','z1234')

```

新建项目

1.新建一个普通的maven项目

2.删除src目录

3.导入maven依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--父工程-->
    <groupId>org.example</groupId>
    <artifactId>Mybatis-study</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>15</maven.compiler.source>
        <maven.compiler.target>15</maven.compiler.target>
    </properties>


    <!--导入依赖-->
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
          	<groupId>org.mybatis</groupId>          
          	<artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>
    </dependencies>
</project>
```

### 2.2创建一个模块

- 编写mybatis的配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/><!--事物管理-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf-8"/>
                <property name="username" value="root"/>
                <property name="password" value="zz123456"/>
            </dataSource>
        </environment>
    </environments>

<!--    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>-->
</configuration>
```

- 编写mybatis的工具类

```java
package com.zz.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//从 SqlSessionFactory 中获取 SqlSession
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {
        try{
            //使用mybatis第一步：获取sqlsessionfactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        }catch (IOException e){
            e.printStackTrace();
        }
    }
    public static SqlSession getSqlsession(){
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //操作数据库
        return  sqlSession;
    }
}
```

### 2.3编写代码

- 实体类

```java
package com.zz.pojo;

public class User {
    private int id;
    private String name;
    public String pwd;

    public User() {
    }

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

}
```

- 接口

```java
package com.zz.dao;

import com.zz.pojo.User;

import java.util.List;

public interface UserDao {
    List<User> getUserlist();
}
```

- 接口实现类,由原来的impl文件转换为xml配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.zz.dao.UserDao">
      <select id="getUserlist" resultType="com.zz.pojo.User">
          select * from Blog where id = #{id}
      </select>
  </mapper>
  ```

### 2.4测试

junit测试

```java
package com.zz.dao;

import com.zz.pojo.User;
import com.zz.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionManager;
import org.junit.Test;

import java.util.List;

public class UserDaoTest {
    @Test
    public void  test(){
        //获得sqlSession对象
        SqlSession sqlsession = MybatisUtils.getSqlsession();
        //执行sql
        UserDao mapper = sqlsession.getMapper(UserDao.class);
        List<User> userlist = mapper.getUserlist();

        for(User user:userlist){
            System.out.println(user.pwd);
        }

        //关闭SqlSession
        sqlsession.close();
    }
}
```

```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>


            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>

```

## 3、CRUD

### 1.namespace

### 2.select

选择，查询语句：

- id：就是对应的namespace中的方法名
- resultType：Sql语句的返回值
- parameterType：参数类型



1.编写接口

```java
//根据id查询用户 ，返回值是个用户
User getUserByid(int id);
```

2.编写对应的mapper中的sql语句

```xml
<select id = "getUserByid" parameterType="int" resultType="com.zz.pojo.User">
    select  *from mybatis.user where id = #{id};
</select>
```

3.测试

```java
@Test
public  void getUserByid(){
    SqlSession sqlsession = MybatisUtils.getSqlsession();
    UserMapper mapper = sqlsession.getMapper(UserMapper.class);

    User user= mapper.getUserByid(1);
    System.out.printf(user.pwd);

    sqlsession.close();//关闭资源

}
```

### 3.insert

```xml
<!--对象中的属性可以直接取到-->
<insert id = "addUser" parameterType="com.zz.pojo.User">
    insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
</insert>
```

### 4.update

```xml
<update id = "updataUser" parameterType="com.zz.pojo.User">
    update mybatis.user set name=#{name},pwd = #{pwd} where id = #{id};
</update>
```

### 5.Delete

```xml
<delete id = "deletUser" parameterType="int">
    delete from mybatis.user where id = #{id};
</delete>
```

**注意：增删改需要提交事务！！！**

### 6.万能Map

```java
//用万能的map
int addUser2(Map<String,Object> map);
```

```xml
<insert id="addUser2" parameterType="map">
    insert into mybatis.user (id,name,pwd) values (#{userid},#{username},#{userpwd});

</insert>
```

```java
@Test
public void addUser2(){
    SqlSession sqlsession = MybatisUtils.getSqlsession();

    UserMapper mapper = sqlsession.getMapper(UserMapper.class);
    Map<String,Object> map = new HashMap<>();
    map.put("userid",6);
    map.put("username","地理");
    map.put("userpwd","zz12");
    int i = mapper.addUser2(map);
    //int i = mapper.addUser(new User(4, "中国", "12313"));

    if(i>0){
        System.out.printf("增加成功！");
    }
    //增删改 一定一定要提交事物！
    sqlsession.commit();
    sqlsession.close();

}
```

Map传递参数，直接在sql中取出key即可！【parameterType="map"】
对象传递参数，直接在sql中取对象的属性即可【pa rameter="Object"】

只有一个基本类型参数的情况下，可以直接在sql中取到！
多个参数用Map，或者注解！

### 7.模糊查询

模糊查询怎么查？

1.java代码执行的时候，传递通配符%%

```java
List<User> userlike = mapper.getUserlike("%地%");
```

2.在sql拼接中使用通配符

```xml
select * from mybatis.user where name like "%"#{value}"%";
```

## 4、配置解析

### 1.核心配置文件

- mybatis-config.xml
- Mybatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息

```
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
```

### 2.环境配置（environments）

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

**事务管理器（transactionManager）**

在 MyBatis 中有两种类型的事务管理器（也就是 type="[JDBC|MANAGED]"）：

- JDBC – 这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域。
- MANAGED – 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接。然而一些容器并不希望连接被关闭，因此需要将 closeConnection 属性设置为 false 来阻止默认的关闭行为。

**数据源（dataSource）**

dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。

- 大多数 MyBatis 应用程序会按示例中的例子来配置数据源。虽然数据源配置是可选的，但如果要启用延迟加载特性，就必须配置数据源。

有三种内建的数据源类型（也就是 type="[UNPOOLED|POOLED|JNDI]"）

默认的是：PPOOLED（连接池）-这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 **这种处理方式很流行，能使并发 Web 应用快速响应请求。**

Mybatis默认的事务是JDBC，连接池：POOLED

### 3.属性（properties）

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。



编写一个配置文件

```properties
driver = com.mysql.jdbc.Driver
url = jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8
username = root
password = zz123456
```

在核心配置文件中引入

```properties
<!--引入外部配置文件-->
<properties resource="db.properties">
    <property name="username" value="root"/>
    <property name="password" value="zz123456"/>
</properties>
```

- 可以直接引入
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，优先使用外部配置文件的！resource属性值优先级高于property子节点配置的值

### 4.类型别名（typeAliases）

- 类型别名是可以java类型设置一个短的名字
- 存在的意义仅在于用来减少类完全限定的冗余

```xml
<!--给实体类取别名-->
<typeAliases>
    <typeAlias type="com.zz.pojo.User" alias="user">
</typeAliases>
```

也可以指定一个包名，Mybatis会包在下面搜索需要的javabean，比如扫描实体类的包，它的默认别名就是为这个类名，首字母小写

```xml
<typeAliases>
  <package name="com.zz.pojo"/>
</typeAliases>
```

###  5.设置



### 6.其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)

### 7.映射器（mappers）

方法一：

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

方法二：使用class文件绑定注册

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

注意点：

- 接口和他的mapper配置文件必须同名！
- 接口和他的mapper配置文件必须在同一个包下

方法三：使用扫描包进行注入绑定

```xml
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

注意点：

- 接口和他的mapper配置文件必须同名！
- 接口和他的mapper配置文件必须在同一个包下

### 8.生命周期和作用与

#### SqlSessionFactoryBuilder

- 这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。 
- 局部变量

#### SqlSessionFactory

- 说白了就是数据库连接池
- SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。 
- 因此 SqlSessionFactory 的最佳作用域是应用作用域

#### SqlSession

- 对应着一次数据库会话，由于数据库会话不是永久的
- 每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。

![image-20210203163220905](/Users/maczhen/Library/Application Support/typora-user-images/image-20210203163220905.png)

## 5.解决属性名和字段名不一致的问题

数据库中的字段

新建一个项目，拷贝之前的

ResultMap结果集映射

```xml
<resultMap id="userlist" type="User">
    <!--column=数据库中的字段，property实体类中的属性-->
    <result column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>

<select id = "getUserlist" resultMap="userlist">
    select * from mybatis.user
</select>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素。
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。
- ResultMap最优秀的地方在于，虽然你已经相当了解了，但是根本不需要显式的用到他们。
- 如果世界都这么美好就好了！

![image-20210203183930966](/Users/maczhen/Library/Application Support/typora-user-images/image-20210203183930966.png)

## 6.日志

### 6.1日志工厂

如果一个数据库的操作出错了，需要排错，可以使用日志！

- SLF4J
- Apache Commons Logging
- Log4j 2
- Log4j【掌握】
- STDOUT_LOGGING【掌握】
- JDK logging

具体使用哪一个日志实现，在设置中设定！

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

![image-20210203193833726](/Users/maczhen/Library/Application Support/typora-user-images/image-20210203193833726.png)

### 6.2Log4j

1.先导包

-  Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件，甚至是套接口服务器、[NT](https://baike.baidu.com/item/NT/3443842)的事件记录器、[UNIX](https://baike.baidu.com/item/UNIX) [Syslog](https://baike.baidu.com/item/Syslog)[守护进程](https://baike.baidu.com/item/守护进程/966835)等；
- 我们也可以控制每一条日志的输出格式；通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。
- 最令人感兴趣的就是，这些可以通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码。

2.log4j.properties

```properties
### 配置根 ###
log4j.rootLogger = debug,console ,fileAppender,dailyRollingFile,ROLLING_FILE,MAIL,DATABASE

### 设置输出sql的级别，其中logger后面的内容全部为jar包中所包含的包名 ###
log4j.logger.org.apache=dubug
log4j.logger.java.sql.Connection=dubug
log4j.logger.java.sql.Statement=dubug
log4j.logger.java.sql.PreparedStatement=dubug
log4j.logger.java.sql.ResultSet=dubug
### 配置输出到控制台 ###
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern =  %d{ABSOLUTE} %5p %c{ 1 }:%L - %m%n

### 配置输出到文件 ###
log4j.appender.fileAppender = org.apache.log4j.FileAppender
log4j.appender.fileAppender.File = logs/log.log
log4j.appender.fileAppender.Append = true
log4j.appender.fileAppender.Threshold = DEBUG
log4j.appender.fileAppender.layout = org.apache.log4j.PatternLayout
log4j.appender.fileAppender.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n

### 配置输出到文件，并且每天都创建一个文件 ###
log4j.appender.dailyRollingFile = org.apache.log4j.DailyRollingFileAppender
log4j.appender.dailyRollingFile.File = logs/log.log
log4j.appender.dailyRollingFile.Append = true
log4j.appender.dailyRollingFile.Threshold = DEBUG
log4j.appender.dailyRollingFile.layout = org.apache.log4j.PatternLayout
log4j.appender.dailyRollingFile.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n### 配置输出到文件，且大小到达指定尺寸的时候产生一个新的文件 ###log4j.appender.ROLLING_FILE=org.apache.log4j.RollingFileAppender log4j.appender.ROLLING_FILE.Threshold=ERROR log4j.appender.ROLLING_FILE.File=rolling.log log4j.appender.ROLLING_FILE.Append=true log4j.appender.ROLLING_FILE.MaxFileSize=10KB log4j.appender.ROLLING_FILE.MaxBackupIndex=1 log4j.appender.ROLLING_FILE.layout=org.apache.log4j.PatternLayout log4j.appender.ROLLING_FILE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n

### 配置输出到邮件 ###
log4j.appender.MAIL=org.apache.log4j.net.SMTPAppender
log4j.appender.MAIL.Threshold=FATAL
log4j.appender.MAIL.BufferSize=10
log4j.appender.MAIL.From=chenyl@yeqiangwei.com
log4j.appender.MAIL.SMTPHost=mail.hollycrm.com
log4j.appender.MAIL.Subject=Log4J Message
log4j.appender.MAIL.To=chenyl@yeqiangwei.com
log4j.appender.MAIL.layout=org.apache.log4j.PatternLayout
log4j.appender.MAIL.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n

### 配置输出到数据库 ###
log4j.appender.DATABASE=org.apache.log4j.jdbc.JDBCAppender
log4j.appender.DATABASE.URL=jdbc:mysql://localhost:3306/mybatis
log4j.appender.DATABASE.driver=com.mysql.jdbc.Driver
log4j.appender.DATABASE.user=root
log4j.appender.DATABASE.password=zz123456
log4j.appender.DATABASE.sql=INSERT INTO LOG4J (Message) VALUES ('[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n')
log4j.appender.DATABASE.layout=org.apache.log4j.PatternLayout
log4j.appender.DATABASE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
log4j.appender.A1=org.apache.log4j.DailyRollingFileAppender
log4j.appender.A1.File=SampleMessages.log4j
log4j.appender.A1.DatePattern=yyyyMMdd-HH'.log4j'
log4j.appender.A1.layout=org.apache.log4j.xml.XMLLayout
```

3.配置log4j实现

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

- 在使用Log4j的类中，导入包import org.apache.log4j.Logger;
- 日志对象，参数为当前类的calss

```java
static Logger logger = Logger.getLogger(UserDaoTest.class);
```

- 日志级别

```java
@Test
public void testlog(){
    logger.info("info:进入了info的testlog4j");
}
```

## 7.分页

思考：为什么分页？

减少数据量，方便浏览！

### 7.1使用Limit分页

```sql
语法：SELECT *from user limit startIndex.pageSize;

SELECT *From user limit 3;#[0,n]
```

使用Mybatis实现分页，核心SQL

1.接口

```java
//分页
List<User> getUserBylimit(Map<String,Integer> map);
```

2.Mapper.xml

```xml
<select id="getUserBylimit" parameterType="map" resultMap="userlist">
    select *from mybatis.user limit #{startIndex},#{pageSize}
</select>
```

3.测试

```java
@Test
public void getUserBylimit(){
    SqlSession sqlsession = MybatisUtils.getSqlsession();
    UserMapper mapper = sqlsession.getMapper(UserMapper.class);
    Map<String,Integer> map= new HashMap<String,Integer>();
    map.put("startIndex",0);
    map.put("pageSize",2);

    List<User> userBylimit = mapper.getUserBylimit(map);
    for(User user:userBylimit){
        System.out.println(user.password);
    }
    sqlsession.close();
}
```

### 7.2RowBounds分页

不再使用sql实现分页

1.接口



2.Mapper.xml

```xml
<select id="getUserByRowBounds" parameterType="map" resultMap="userlist">
    select *from mybatis.user
</select>
```

3.测试

```java
@Test
public void getUserByRowBounds(){
    SqlSession sqlsession = MybatisUtils.getSqlsession();
    //RowBounds实现
    RowBounds rowBounds = new RowBounds(1, 2);


    //通过java代码实现分页
    List<User> userList = sqlsession.selectList("com.zz.dao.UserMapper.getUserByRowBounds",0,rowBounds);
    for(User user:userList){
        System.out.println(user.password);
    }
    sqlsession.close();
}
```

7.3分页插件

![image-20210204105401216](/Users/maczhen/Library/Application Support/typora-user-images/image-20210204105401216.png)

官网：https://pagehelper.github.io

![image-20210204105436324](/Users/maczhen/Library/Application Support/typora-user-images/image-20210204105436324.png)

了解即可！

## 8.使用注解开发

### 8.1面向接口编程

### 8.2使用注解

1.注解在接口上实现

```java
@Select("select *from mybatis.user")
List<User> getUser();
```

2.在核心配置文件中绑定接口！

```xml
<!--每一个Mapper。xml需要注册-->
<mappers>
    <mapper class="com.zz.dao.UserMapper"/>
    <!--<package name="com.zz.dao" />-->
</mappers>
```

3.测试

```java
@Test
public void getUserlike(){
    SqlSession sqlsession = MybatisUtils.getSqlsession();
    UserMapper mapper = sqlsession.getMapper(UserMapper.class);
    //底层主要应用反射
    List<User> userlike = mapper.getUser();
    for(User user:userlike){
        System.out.println(user.getName());
    }
    sqlsession.close();
}
```



本质：发射机制实现

底层：动态代理



Mybatis详细的执行流程！



### 8.3注解实现CRUD

我们可以在工具类创建的时候实现自动提交事务

```java
public static SqlSession getSqlsession(){
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    //操作数据库
    return  sqlSession;
}
```

编写接口

```java
package com.zz.dao;

import com.zz.pojo.User;
import org.apache.ibatis.annotations.*;

import java.util.List;
import java.util.Map;

public interface UserMapper {
    @Select("select *from mybatis.user")
    List<User> getUser();

    //方法存在多个参数，所有的参数前面必须要家@Param("")注解
    @Select("select *from mybatis.user where id = #{id}")
    User getUserByid(@Param("id")int id);

    //增加用户信息
    @Insert("insert into user(id,name,pwd) values(#{id},#{name},#{password})")
    int addUser(User user);

    //修改用户信息
    @Update("updata user set name=#{name},pwd=#{password} where id=#{id}")
    int updataUser(User user);

    //删除用户信息
    @Delete("delete from user where id=#{uid}")
    int deleteUser(@Param("uid") int id);
}
```

测试类

```java
package com.zz.dao;

import com.zz.pojo.User;
import com.zz.utils.MybatisUtils;
import org.apache.ibatis.session.RowBounds;
import org.apache.ibatis.session.SqlSession;
import org.apache.log4j.Logger;
import org.junit.Test;

import java.util.HashMap;
import java.util.List;
import java.util.Map;


public class UserDaoTest {
    static Logger logger = Logger.getLogger(UserDaoTest.class);

    @Test
    public void getUserlike(){
        SqlSession sqlsession = MybatisUtils.getSqlsession();
        UserMapper mapper = sqlsession.getMapper(UserMapper.class);
//        //底层主要应用反射
//        List<User> userlike = mapper.getUser();
//        for(User user:userlike){
//            System.out.println(user.getName());
//        }
//        User userByid = mapper.getUserByid(1);
        int i = mapper.addUser(new User(2, "中国好", "zz331"));
        System.out.println(i);
        sqlsession.close();
    }
}
```

【注意：我们必须要把接口绑定到核心配置文件中！】

**关于@Param()注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上！
- 我们在Sql中引用的就是我们这里的@Param（）中设定的属性名

#{}与${}的区别

## 9.Lombok插件

使用步骤：

1.在项目中安装Lombok的插件

2.在项目中导入jar包

```xml
<dependencies>
     <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.10</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

3.在实体类加注解

**@EqualsAndHashCode：作用于类，覆盖默认的equals和hashCode**

**@NonNull：主要作用于成员变量和参数中，标识不能为空，否则抛出空指针异常。**

@NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor：作用于类上，用于生成构造函数。有staticName、access等属性。

**staticName属性一旦设定，将采用静态方法的方式生成实例，access属性可以限定访问权限。**

**@NoArgsConstructor：生成无参构造器；**

**@RequiredArgsConstructor：生成包含final和@NonNull注解的成员变量的构造器；**

**@AllArgsConstructor：生成全参构造器**

**@Data：作用于类上，是以下注解的集合：@ToString @EqualsAndHashCode @Getter @Setter @RequiredArgsConstructor**

**@Builder：作用于类上，将类转变为建造者模式**

**@Log：作用于类上，生成日志变量。针对不同的日志实现产品，有不同的注解：**

```
@Data：无参构造、get、set、tostring、hashcode，equals

@AllArgsConstructor：有参构造

@NoArgsConstructor：无参构造
```



## 10.数据库复杂关系

### 10.1复杂关系搭建

创建数据表

```sql
CREATE TABLE IF NOT EXISTS `teacher`(
   `id` INT(10) NOT NULL,
   `name` VARCHAR(30) NOT NULL,
   PRIMARY KEY ( `id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO teacher(id,name) VALUES(1,'朱老师');

CREATE TABLE IF NOT EXISTS `student`(
	`id` INT(10) NOT NULL,
 `name` VARCHAR(30) DEFAULT NULL,
 `tid` INT(10) DEFAULT NULL,
  PRIMARY KEY(`id`),
  KEY `fktid`(`tid`),
  CONSTRAINT `fktid` FOREIGN KEY(`tid`) REFERENCES `teacher` (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `student`(id,name,tid) VALUES(1,`小米`,`1`);
INSERT INTO `student`(id,name,tid) VALUES(2,`小红`,`1`);
INSERT INTO `student`(id,name,tid) VALUES(3,`小洪`,`1`);
INSERT INTO `student`(id,name,tid) VALUES(4,`小百`,`1`);
INSERT INTO `student`(id,name,tid) VALUES(5,`小丽`,`1`);
```

测试环境搭建：

1.导入lombok

```xml
<artifactId>mybatis05</artifactId>
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>RELEASE</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

2.新建实体类

Student：

```java
package com.zz.pojo;

import lombok.Data;

@Data
public class Student {
    private int id;
    private String name;
    private Teacher teacher;
}
```

Teacher：

```java
package com.zz.pojo;


import lombok.Data;

@Data
public class Teacher {
    private int id;
    private String name;
}
```

3.建立Mapper接口

TeacherMapper：

```java
package com.zz.dao;

import com.zz.pojo.Teacher;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

public interface TeacherMapper {
    @Select("select *from mybatis.teacher where id=#{tid}")
    Teacher getTeacher(@Param("tid") int id);
}
```

4.建立Mapper.xml

Teacher.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zz.dao.TeacherMapper">

</mapper>
```

5.在核心配置文件中绑定注册

```xml
<!--每一个Mapper。xml需要注册-->
<mappers>
    <mapper class="com.zz.dao.TeacherMapper"/>
    <mapper class="com.zz.dao.StudentMapper"/>
</mappers>
```

6.测试查询能否成功

```java
package com.zz.dao;

import com.zz.pojo.Teacher;
import com.zz.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;

public class Teachertest {
    public static void main(String[] args) {
        SqlSession sqlsession = MybatisUtils.getSqlsession();
        TeacherMapper mapper = sqlsession.getMapper(TeacherMapper.class);

        Teacher teacher = mapper.getTeacher(1);
        System.out.println(teacher.getName());
        sqlsession.close();
    }
}
```

### 10.2按照查询嵌套处理

```xml
<select id="getStudent" resultMap="StudentTeacher">
    select *from student;
</select>
<resultMap id="StudentTeacher" type="Student">
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <!--复杂的属性，我们需要单独处理 对象使用association 集合使用collection-->
    <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
</resultMap>


<select id="getTeacher" resultType="Teacher">
    select *from teacher where id=#{id}
</select>
```

### 10.3按照结构嵌套处理

```xml
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname,t.name tname
    from student s,teacher t where s.tid = t.id;
</select>
<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```

Mysql多对一查询方式：

- 子查询
- 联表查询

## 11.一对多

比如：一个老师拥有多个学生！

对于老师而言，就是一对多的关系！

### 11.1搭建环境

编写实体类

Teacher：

```java
package com.zz.pojo;


import lombok.Data;

import java.util.List;

@Data
public class Teacher {
    private int id;
    private String name;

    private List<Student> students;

    @Override
    public String toString() {
        return "Teacher{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", students=" + students +
                '}';
    }
}

```

Student：

```java
package com.zz.pojo;

import lombok.Data;

@Data
public class Student {
    private int id;
    private String name;
    private int tid;

}
```



### 11.2按照结构嵌套处理

```sql
select s.id sid,s.name sname,t.name tname,t.id tid from student s,teacher t
where s.tid = t.id
```

1.编写接口

```
package com.zz.dao;

import com.zz.pojo.Teacher;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface TeacherMapper {
    //获取老师
//    List<Teacher> getTeacher();
    Teacher getTeacher(@Param("tid") int id);
}
```

2.编写xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zz.dao.TeacherMapper">

    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid,s.name sname,t.name tname,t.id tid from student s,teacher t
        where s.tid = t.id = #{tid}
    </select>
    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"/>
        <result property="name" column="tname"/>
        <collection property="students" ofType="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>
    </resultMap>
</mapper>
```

3.测试

```java
package com.zz.dao;

import com.zz.pojo.Teacher;
import com.zz.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class myTest {
    @Test
    public void test(){
        SqlSession sqlsession = MybatisUtils.getSqlsession();
        TeacherMapper mapper = sqlsession.getMapper(TeacherMapper.class);
        Teacher teacher = mapper.getTeacher(1);

        System.out.printf(teacher.toString());
        sqlsession.close();
    }
}
```

**小结：**

1.关联-association【多对一】

2.集合-collection【一对多】

3.javaType & offType

- ​	javaType用来指定实体类中属性的类型
- ​	offType用来指定映射到List集合中的poji类型，泛型中的约束类型！

**注意点：**

- 保证sql的可读性，尽量保证通俗易懂
- 注意一对多和多对一中，属性名和字段的问题！
- 如果问题不好排查错误，可以使用日志，建议使用Log4j

**面试高频：**

- Mysql引擎
- InnDB底层原理
- 索引
- 索引优化

## 12.动态SQL

动态 SQL 是 MyBatis 的强大特性之一,根据不同的条件生成不同的语句。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL 语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。

- if
- choose (when, otherwise)
- trim (where, set)
- foreach

### 12.1搭建环境

```sql
CREATE TABLE `blog`(
`id` VARCHAR(50) NOT NULL COMMENT '博客id',
`title` VARCHAR(100) NOT NULL COMMENT '博客标题',
`author` VARCHAR(30) NOT NULL COMMENT '博客作者',
`create_time` DATETIME NOT NULL COMMENT '创建时间',
`views` INT(30) NOT NULL COMMENT '浏览量'
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

创建一个基础工程

1.导包

```xml
<dependencies>
    <dependency>
      <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.10</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

2.编写配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--引入外部配置文件-->
    <properties resource="db.properties">
        <property name="username" value="root"/>
        <property name="password" value="zz123456"/>
    </properties>

    <!--日志-->
    <settings>
        <!--标准日志工厂实现-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
        <!--是否开启自动驼峰命名规则-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
    <typeAliases>
        <package name="com.zz.pojo"/>
    </typeAliases>


    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/><!--事物管理-->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="zz123456"/>
            </dataSource>
        </environment>
    </environments>

    <!--每一个Mapper。xml需要注册-->
    <!--每一个Mapper。xml需要注册-->
    <mappers>
        <mapper class="com.zz.dao.BlogMapper"/>
    </mappers>
<!--    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>-->
</configuration>
```

3.编写实体类

```java
package com.zz.pojo;

import lombok.Data;

import java.util.Date;

@Data
public class Blog {
    private String id;
    private String title;
    private String author;
    private Date createtime;//属性名和字段名不一样
    private int views;
}
```

4.编写接口和Mapper.xml

接口：

```java
package com.zz.dao;

import com.zz.pojo.Blog;

import java.util.List;
import java.util.Map;

public interface BlogMapper {
    //插入数据
    int addBlog(Blog blog);

    //查询
    List<Blog> getBolgBytitle(Map map);

    //查询choose
    List<Blog> getBlogBytitle(Map map);

    //更新数据
    int updataBlog(Map map);
}
```

xml：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zz.dao.BlogMapper">
    
</mapper>
```

### 12.2IF

```xml
<select id="getBolgBytitle" parameterType="map" resultType="blog">
    select *from mybatis.blog
    <where>
        <if test="title!=null">
            and title like #{title}
        </if>
        <if test="author!=null">
            and author = #{author}
        </if>
    </where>

</select>
```

### 12.3choose(when,otherwise)

```xml
<select id="getBlogBytitle" parameterType="map" resultType="blog">
    select *from mybatis.blog
    <where>
        <choose>
            <when test="title!=null">
                title = #{title}
            </when>
            <when test="author!=null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```

### 12.4Trim(where,set)

```xml
<update id="updataBlog" parameterType="map">
    update mybatis.blog
    <set>
        <if test="title!=null">
            title = #{title},
        </if>
        <if test="author!=null">
            author = #{author},
        </if>
        <if test="views!=null">
            views = #{views}
        </if>
    </set>
    where id = #{id}
</update>】
```

所谓动态Sql，本质还是Sql语句，只是我们可以在sql侧层面，去执行一个逻辑代码！



SQL片段

有时候，我们可能会需要取出一些公用的操作,方便。

1.使用SQL标签抽取公共的部分

```xml
<sql id="if-title-author">
    <if test="title!=null">
        and title like #{title}
    </if>
    <if test="author!=null">
        and author = #{author}
    </if>
</sql>
```

2.需要使用的地方使用include标签引用即可

```xml
<select id="getBolgBytitle" parameterType="map" resultType="blog">
    select *from mybatis.blog
    <where>
        <include refid="if-title-author"/>
    </where>

</select>
```

注意事项：

- 最好基于单表来定义SQL片段
- 不要存在where标签

### 12.5foreach

```xml
<select id="queryBlogForeach" parameterType="map" resultType="blog">
    select *from mybatis.blog
    <where>
        <foreach collection="ids" item="id"
                 open="and (" close=")" separator="or">
            id = #{id}
        </foreach>
    </where>
</select>
```

动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL格式，去排列组合就可以了！

建议：先在Mysql中写出完整的Sql语句，再去对应的修改我们的动态SQL实现通用即可！

## 13.缓存

### 13.1简介

1.什么是缓存【Cache】？

- 存在内存中的临时数据
- 将用户经常查询的数据存放在内存中，用户去查询速降就不用从磁盘上（关系型数据库文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2.为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统的效率

3.什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据【可以使用缓存】

### 13.2Mybatil缓存

- MyBatis 内置了一个强大的事务性查询缓存机制，它可以非常方便地配置和定制。 为了使它更加强大而且易于配置，我们对 MyBatis 3 中的缓存实现进行了许多改进。

- Mybatis系统中默认定义了两级缓存：一级缓存和二级缓存
  - 默认情况下，只有一级缓存开启。
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高扩展性，Mybatis定义了缓存接口Cache，我们可以实现接口自定义缓存

13.3一级缓存

一级缓存也叫本地缓存：SqlSession

与数据库同一次会话查询到的数据会放在本地缓存中。

以后如果需要获取相同的数据，直接从那个缓存中取，没必要再次去查询数据库！



测试步骤：

1.开启日志

2.测试在一个Session中查询两次记录

3.查看日志输出

![image-20210206105610688](/Users/maczhen/Library/Application Support/typora-user-images/image-20210206105610688.png)

缓存失效

1.查询不同的东西

2.增删改操作，可能会改变原来的数据，所以必会刷新缓存！

![image-20210206105052452](/Users/maczhen/Library/Application Support/typora-user-images/image-20210206105052452.png)

3.查询不同的mapper

4.手动清理缓存

小结：

一级缓存是默认存在的！

13.4二级缓存

二级缓存也叫全局缓存，一级缓存作用区域太低了，所以诞生了二级缓存。

基于namespace级别的缓存，一个名称空间，对应一个二级缓存；

工作机制：

- 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
- 如果当前会话关闭了，这个会会话关闭了一级缓存就没了，但是我们想要的是，会话关闭了，一级缓存保存到二级缓存中。
- 新的会话查询信息，就可以从二级缓存中获取内容；
- 不同的mapper查出数据会放在对应的缓存（map）中

步骤：

1.开启全局缓存

```xml
<!--默认开启全局缓存-->
<setting name="cacheEnabled" value="true"/>
```

2.在要使用二级缓存的Mapper中开启

```xml
<cache
        eviction="FIFO"
        flushInterval="60000"
        size="512"
        readOnly="true"/>
```

3.测试

1.问题：我们需要将实体类序列化，否则就会报错！

```java
Caused by: java.io.NotSerializableException: com.zz.pojo.User
```

2.解决：

```java
public class User implements Serializable {
    private int id;
    private String name;
    private String pwd;

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

小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会先放在一级缓存中
- 只有当会话提交，或者关闭的时候，才会提交到二级 

### 13.5缓存原理

![image-20210206120321461](/Users/maczhen/Library/Application Support/typora-user-images/image-20210206120321461.png)

### 13.6自定义缓存-ehcache

Ehcache是一种广泛使用的开源的Java分布式缓存，主要是面向通用缓存



在程序中使用

1.导入jar包

2.编写配置文件ehcache.xml

3.测试



在实际开发中使用Radis！