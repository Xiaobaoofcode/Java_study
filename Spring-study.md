## Spring

## 1.1简介

Spring：春天---》给软件行业带来了春天！

2002，首次推出Spring框架的雏形

SSH：Struct2+Spring+Hibernate！

SSM：SpringMvc+Spring+Mybatis！

官网：https://docs.spring.io/spring-framework/docs/current/reference/html/

github：

导包：

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.3</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.3</version>
</dependency>
```

### 1.2优点

- Spring是一个开源的免费框架（容器）
- Spring是一个轻量级的，非入侵的框架
- 控制反转IOC，面向切面编程（AOP）
- 支持事务处理，对框架整合的支持

总结：Spring就是一个轻量级的控制反转（IOC）和面向切面（AOP）编程的框架！

### 1.3组成

![image-20210206175826397](/Users/maczhen/Library/Application Support/typora-user-images/image-20210206175826397.png)

### 1.4拓展

- SpringBoot
  - 一个快速开发的脚手架
  - 基于SpringBoot可以快速的开发单个微服务
  - 约定大于配置！

- SpringCloud
  - SpringCloud是基于SpringBoot实现的

因为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Spring及SpringMVC！承上启下！

弊端：

发展太久，违背了原来的理念，配置十分繁琐！配置地狱！

## 2.IOC理论推导

1.UserDao接口

2.UserDaoImpl实现类

3.UserService接口

4.UserServiceImpl实现类

在我们之前的业务中，用户的需求可能会影响原来的代码，我们需要根据用户的需求去修改原代码，如果程序代码十分大，修改一次的成本代价十分昂贵



我们使用一个set接口实现。发生的变化很大！

```java
//利用set动态注入
public void setUserdao(Userdao userdao) {
    this.userdao = userdao;
}
```

**之前的程序是主动创建对象，主动权在程序员手上。**

使用set注入后，程序不在具有主动性，而是变成了被动的接受对象！

这种思想，从本质上解决了问题，我们程序员不再去管理对象的创建了，系统的耦合性大大降低，可以更加专注在业务的实现上！这是IOC的原型！

## 3.SpringHello

配置xml文件，我们只需要在xml文件中修改代码，不需要修改原来的代码！所谓的IoC，一句话搞定：对象有Spring来创建，管理和装配！

配置beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--使用spring创建对象，在Spring这些称为bean
    原来创建对象：类型 变量名 = new 类型（）-》Hello hello = new Hello（）
    现在：
    id = 变量名   class = new 的对象  property 相当于给对象中的属性设置一个值
    -->
    <bean id="hello" class="com.zz.pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>

</beans>
```

测试：

```java
package com.zz.pojo;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class mytest {
    public static void main(String[] args) {
        //获取Spring的上下文对象！
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        //我们的对象现在都在spring中管理了。我们可以直接使用
        Hello hello = (Hello) context.getBean("hello");
        System.out.printf(hello.toString());
    }
}
```

思考问题？

Hello对象是谁创建的？ Spring创建的

Hello对象的属性是怎么设置的？ hello对象的属性由Spring容器设置的。

这个过程叫控制反转：

控制：谁来控制对象的创建，传统应用程序的对象由程序本身控制创建的，使用Spring后，对象由Spring来创建的

反转：程序本身不创建对象，而变成被动的接收对象。

依赖注入：就是利用set方法来进行注入的

IOC是一种编程思想，由主动的编程变成被动的接收

可以通过new ClassPathApplicationContext去浏览一下底层源码。



## 4.IoC创建对象的方式

1.使用无参构造方法，默认！

2.使用有参构造方法

​	1.下标赋值

```xaml
<!-- 第一种方式 下标赋值-->
 <bean id="user" class="com.zz.pojo.User">
     <constructor-arg index="0" value="kaung"/>
 </bean>
```

​	2.通过创建类型

```xml
<!--第二种方式 通过类型创建 不建议使用！-->
<bean id="user" class="com.zz.pojo.User">
    <constructor-arg type="java.lang.String" value="你好"/>
</bean>
```

​	3.直接通过参数名

```xml
<!--第三种直接通过参数名来
-->
<bean id="user" class="com.zz.pojo.User">
    <constructor-arg name="name" value="诸葛"/>
</bean>
```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了！

## 5.Sping的配置

5.1别名

```xml
!<!--别名，配置了别名，可以通过别名或者原名获取对象-->
<alias name="user" alias="userName"/>
```

5.2bean的配置

```xml
<!--
id：bean的唯一标识符，也就是相当于我们学的对象名
class：bean对象所对应的全限定名，包名+类型
name：也就是别名，而且name可以同时取多个别名
-->
<bean id="user" class="com.zz.pojo.User" name="user2">
    <constructor-arg name="name" value="你好"/>
</bean>
```

5.3import

这个import一般用于团队开发使用。他可以将多个配置文件，导入合并为一个。

假设，现在项目中有多个人开放，这三个人负责不同的类开放，不同的类需要注册在不同的bean中，我们可以利用import将所有的人的beans.xml合并为一个总的！

```xml
<import resource="beans.xml"/>
  <import resource="beans1.xml"/>
```

- ​	张三
- ​	李四
- ​	王五
- application.xml

使用的时候，直接使用总的配置就可以了

## 6.DI依赖注入

6.1构造器注入

前面已经说过

6.2Set放入注入【重点】

【环境搭建】

1.复杂类型

```java
package com.zz.pojo;

public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

2.真实测试对象

```java
private String name;
private Address address;
private String[]books;
private List<String> hobby;
private Map<String,String> card;
private Set<String> games;
private String wife;
private Properties info;
```

3.beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--第一种注入！-->
    <bean id="student" class="com.zz.pojo.Student">
        <property name="name" value="你好！"/>
    </bean>

</beans>
```

4.测试类

```java
import com.zz.pojo.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class mytest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        Student student = (Student) context.getBean("student");
        System.out.printf(student.getName());//student.getName();
    }
}
```

完善注入信息：

```xml
<bean id="address" class="com.zz.pojo.Address">
    <property name="address" value="中国"></property>
</bean>


<bean id="student" class="com.zz.pojo.Student">
    <!--第一种注入！普通的value和name-->
    <property name="name" value="你好！"/>
    <!--第二种注入，Bean注入，ref-->
    <property name="address" ref="address"/>

    <!--第三种注入数组-->
    <property name="books">
        <array>
            <value>红楼梦</value>
            <value>西游记</value>
            <value>三国演义</value>
            <value>水浒传</value>
        </array>
    </property>

    <!--list-->
    <property name="hobby">
        <list>
            <value>听歌</value>
            <value>打篮球</value>
            <value>踢足球</value>
        </list>
    </property>

    <!--Map-->
    <property name="card">
        <map>
            <entry key="身份证" value="13213"/>
            <entry key="手机壳" value="312321"/>
        </map>
    </property>
    <!--Set-->
    <property name="games">
        <set>
            <value>LOL</value>
            <value>CoC</value>
        </set>
    </property>

    <!--null-->
    <property name="wife">
        <null/>
    </property>
    <!--Properties-->
    <property name="info">
        <props>
            <prop key="学号">123123</prop>
            <prop key="性别">男</prop>
            <prop key="url">http:2132/com</prop>
            <prop key="userword">123</prop>
            <prop key="password">zz1321</prop>
        </props>
    </property>

</bean>
```

### 6.3拓展方式注入

我们可以使用p和c命名空间

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

```xml
<bean id="user" class="com.zz.pojo.Usertest" p:age="19" p:name="诸葛亮"/>

<bean id="user2" class="com.zz.pojo.Usertest" c:age="20" c:name="礼拜"/>
```

```java
public void test(){
    ApplicationContext context = new ClassPathXmlApplicationContext("Userbeans.xml");
    Usertest user = context.getBean("user2", Usertest.class);
    System.out.printf(user.toString());
}
```

注意点：p命名和c命名必须要导入约束！

### 6.4Bean的作用域

![image-20210207164114540](/Users/maczhen/Library/Application Support/typora-user-images/image-20210207164114540.png)

1.单例模式（Spring默认机制）

```xml
<bean id="user2" class="com.zz.pojo.User" c:age="19" c:name="诸葛" scope="singleton"/>
```

2.原型模式：每次从容器中get的时候，都会产生一个新对象

```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

## 7.Bean的自动装配

 自动装配时Spring满足bean依赖一种方式！

Spring会在上下文自动寻找，并自动给bean装配属性



在Spring中有三种装配方式

1.在xml中显示的配置

2.在java中显示的配置

3.隐式的自动装配bean【重要】

### 7.1测试

### 7.2ByName自动装配

```xml
<!--byname:会自动在容器上下文中查找，和自己对象set方法后的值对应beanid！
-->
<bean id="people" class="com.zz.pojo.People" autowire="byname">
  <property name="name" value="诸葛量"/>
</bean>
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.zz.pojo.Dog"/>
    <bean id="cat" class="com.zz.pojo.Cat"/>

    <bean id="people" class="com.zz.pojo.People" autowire="byName">
        <property name="name" value="诸葛亮">
        </property>

    </bean>


</beans>
```

### 7.3ByType自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.zz.pojo.Dog"/>
    <bean id="cat212" class="com.zz.pojo.Cat"/>

    <bean id="people" class="com.zz.pojo.People" autowire="byType">
        <property name="name" value="诸葛亮">
        </property>
    </bean>
</beans>
```

小结：

byname的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致！

bytype的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性类型一致！

### 7.4.使用注解实现

jdk1.5支持的注解，spring2.5就支持了！

 The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML. 



要使用注解须知：

1导入约束：context

2配置注解的支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

**@Autowired**

直接在属性上使用即可，也可以在set方法上使用！

使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IoC（Spring）容器中存在，且符合名字byname！

科普“

```
@Nullable 字段标记了这个注解，说明这个字段可以为null
```

```java
@Documented
public @interface Autowired {
    boolean required() default true;
}
```

测试代码：

```java
private String name;
//如果显示定义了Autowired的required属性为false，说明这个对象可以为null，否则必须要赋值
@Autowired(required = false)
private Dog dog;
@Autowired
private Cat cat;
```

如果@Autowired自动配置的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用@Qualifier(value="dog22")配合@Autowired使用

```java
private String name;
//如果显示定义了Autowired的required属性为false，说明这个对象可以为null，否则必须要赋值
@Autowired(required = false)
@Qualifier(value = "dog214")
private Dog dog;
@Autowired
@Qualifier(value = "cat217")
private Cat cat;
```

@Resource属性

```java
private String name;
//如果显示定义了Autowired的required属性为false，说明这个对象可以为null，否则必须要赋值
@Resource(name = "dog2")
private Dog dog;
@Resource(name="cat2")
private Cat cat;
```



小结：

Resource和Autowired的区别

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过bytype的方式实现，而且必须要求这个对象存在！
- @Resource默认通过byname的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况，就报错！
- 执行顺序不同，@Autowired通过byType的方式实现，@Resource默认通过byname的方式实现。

## 8.使用注解开发

在spring4之后，要保证aop的包导入！

1.bean

2.属性如何注解

```java
@Component
public class User {
    @Value("哀鸿")
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

3.衍生的注解

@Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层！

- ​	dao【@Repository】
- ​	service【@Service】
- ​	controller【@Controller】

这四个注解的功能是一样的，都是代表将某个类注册到Spring中，装配Bean

4.自动装配置

```
-@Autowired:自动装配通过类型，名字
-@Nullable:字段标记了这个注解，说明这个字段可以为空
-@Resource:自动装配通过名字，类型
```

5.作用域

```java
@Component
@Scope("prototype")
public class User {
    @Value("哀鸿")
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

6.小结

xml与注解

- ​	xml更加万能，适合用于任何场合！维护简单方便
- ​	注解 不是自己的类使用不了，维护相对复杂！

xml与注解最佳实践

- ​	xml用来管理bean；
- ​	注解只负责完成属性的注入；
- ​	我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持！

## 9.使用Java的方式配置Spring

实体类

```java
package com.zz.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }
    @Value("nihao!")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置文件

```java
package com.zz.config;

import com.zz.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.zz.pojo")
public class Userconfig {

    @Bean //这个方法的名字就相当于bean标签的id属性  方法的返回值相当于bean标签中的clss、属性
    public User getUser(){
        return new User();
    }
}
```

测试类

```java
package com.zz.pojo;

import com.zz.config.Userconfig;
import org.springframework.aop.support.annotation.AnnotationClassFilter;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class test {
    public static void main(String[] args) {

        ApplicationContext context = new AnnotationConfigApplicationContext(Userconfig.class);
        User user = context.getBean("user", User.class);
        System.out.printf(user.getName());
    }
}
```

这种纯java的配置方式，在SpringBoot中常见！

## 10.代理模式

为什么要学习代理模式，因为这就是SpringAOP的底层！【SpringAOP和SpringMVC】

### 10.1静态代理

角色分析

- ​	抽象角色：一般会使用接口或者抽象类来解决
- ​	真实角色：被代理的角色
- ​	代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- ​	客户：访问代理对象的人

代码步骤：

1.接口

```java
package com.zz.clien;

public interface rent {
    public void rent();
}
```

2.真实角色

```java
package com.zz.clien;

public class Host implements rent{
    @Override
    public void rent() {
        System.out.printf("我是房东！");
    }
}
```

3.代理角色

```java
package com.zz.clien;

public class Proxy implements rent{
    private Host host;

    public Proxy() {
    }

    public Proxy(Host host) {
        this.host = host;
    }
    public void rent(){
        seeHouse();
        host.rent();
        hetong();
        fare();
    }
    //看房
    public void seeHouse(){
        System.out.printf("中介带你看房！");
    }

    //看房
    public void hetong(){
        System.out.printf("等待租房合同！");
    }
    //收中介费
    public void fare(){
        System.out.printf("收中介费！");
    }
}
```

4.客户端访问代理角色

```java
package com.zz.clien;

public class Clien {
    public static void main(String[] args) {
        //房东要来租房子
        Host host = new Host();
        //代理，中介都房东租房子，但是呢，代理一般会有一些附属操作！
        Proxy proxy = new Proxy(host);
        proxy.rent();
    }
}
```

代理模式的好处

- 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公共也可以交给代理角色，实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！

缺点：

- ​	一个真实角色就会产生一个代理角色：代码量会翻倍-开发效率

10.2加深理解

### 10.3动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的
- 动态代理分为两类：基于接口和基于类的动态代理
  - ​	基于接口--jdk动态代理【我们这里使用】
  - ​	基于类：cglib
  - ​	java字节码：javasist

需要两个类：Proxy：代理 InvocationHandler

## 11、AOP

### 11.1什么是AOP



### 11.2Aop在Spring中的作用

### 11.3使用Spring实现Aop

方式一：使用Spring的API接口【主要SpringAPI】

方式二：使用Spring的自定义【主要是切面定义】

方法三：使用注解实现【】

## 12整合Mybatis

步骤：

1.导入相关jar包

​	junit

​	mybatis

​	mysql数据库

​	spring相关的

​	aop织入

​	mybatis-spring【new】

2.编写配置文件

3.测试

### 12.1回忆mybatis

1.编写实体类

2.编写核心配置文件

3.编写接口

4.编写mapper.xml

5.测试



### 12.2Mybatis-Spring

直接看官方文档，内容较少，类似Mybatis！

## 13.声明式事物

### 1、回顾事物

- ​	把一组业务当成一个业务来做，要么都成功，要么都失败！
- ​	事物在项目开发中，十分的重要，涉及到数据一致性问题，不能马虎！
- ​	确保完整性和一致性：



事物ACID原则：

- ​	原子性
- ​	一致性
- ​	隔离性
  - 多个业务可能操作同一个资源，防止数据损坏
- ​	持久性
  - 事务一旦提交



### 2、Spring中的事务管理

- ​	声明式事务：基于AOP
- ​	编程式事务：需要再代码中，进行事务的管理！

​	

思考：

为什么需要事务？

- 如果不配置事务，可能存在数据提交不一致的情况；
- 如果我们不在SPRING中去配置声明式事务，我们就需要在代码中手动去配置事务！
- 事务在项目的开发中十分重要，设计到数据一致性和完整性的问题，不容马虎！		