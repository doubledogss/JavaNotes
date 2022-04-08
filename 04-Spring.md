---
title: 04-Spring
date: 2022-02-09 21:23:45
tags: [Java]
---

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220211131746044.png)



# 第1章 Spring的IoC和DI

## 一、简介

> Spring是分层的 Java SE/EE应用 full-stack 轻量级开源框架，以 **IoC**（Inverse Of Control：反转控制）和**AOP**（Aspect Oriented Programming：面向切面编程）为内核。
>
> 提供了**表现层 SpringMVC** 和**持久层 Spring JDBCTemplate** 以及**业务层事务管理**等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的Java EE 企业应用开源框架。

### 1. 优势

**1）方便解耦，简化开发**

通过 Spring 提供的 IoC容器，可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造成的过度耦合。

用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用。

**2）AOP 编程的支持**

通过 Spring的 AOP 功能，方便进行面向切面编程，许多不容易用传统 OOP 实现的功能可以通过 AOP 轻松实现。

**3）声明式事务的支持**

可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务管理，提高开发效率和质量。

**4）方便程序的测试**

可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情。

**5）方便集成各种优秀框架**

Spring对各种优秀框架（Struts、Hibernate、Hessian、Quartz等）的支持。

**6）降低 JavaEE API 的使用难度**

Spring对 JavaEE API（如 JDBC、JavaMail、远程调用等）进行了薄薄的封装层，使这些 API 的使用难度大为降低。

**7）Java 源码是经典学习范例**

Spring的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对Java 设计模式灵活运用以及对 Java技术的高深造诣。它的源代码无意是 Java 技术的最佳实践的范例。



### 2. 体系结构

![](https://gitee.com/doubledogss/image/raw/master/img/image-20220209213557508.png)



## 二、Spring程序开发步骤

### 1. 导入 Spring 开发的基本包坐标

```xml
<properties> 
    <spring.version>5.0.5.RELEASE</spring.version>
</properties> 

<dependencies>
    <!--导入spring的context坐标，context依赖core、beans、expression--> 
    <dependency> 
        <groupId>org.springframework</groupId> 
        <artifactId>spring-context</artifactId> 
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```



### 2.编写 Dao 接口和实现类

```java
public interface UserDao {
	public void save();
}

public class UserDaoImpl implements UserDao {
	@Override
	public void save() {
		System.out.println("UserDao save method running....");
    } 
}
```



### 3. 创建 Spring 核心配置文件并配置

在类路径下（resources）创建applicationContext.xml配置文件,在 Spring 配置文件中配置 UserDaoImpl

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDao" class="com.lak.dao.impl.UserDaoImpl"></bean>
</beans>
```



### 4. 使用 Spring 的 API 获得 Bean 实例

```java
package com.lak.demo;

import com.lak.dao.UserDao;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserDaoDemo {
    public static void main(String[] args) {
        ApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao = (UserDao) app.getBean("userDao");
        userDao.save();
    }
}
```

<hr>

**Spring的开发步骤**

① 导入坐标

② 创建Bean

③ 创建applicationContext.xml

④ 在配置文件中进行配置

⑤ 创建ApplicationContext对象getBean



## 三、配置文件

### 1. Bean标签基本配置

用于配置对象交由**Spring** 来创建。

默认情况下它调用的是类中的**无参构造函数**，如果没有无参构造函数则不能创建成功。

基本属性：

- **id**：Bean实例在Spring容器中的唯一标识
-  **class**：Bean的全限定名称



### 2.Bean标签范围配置

scope：指对象的作用范围，取值如下：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220211141302939.png)

**1）当scope的取值为singleton时**

​	Bean的实例化个数：1个

​	Bean的实例化时机：当Spring核心文件被加载时，实例化配置的Bean实例

​	Bean的生命周期：

- 对象创建：当应用加载，创建容器时，对象就被创建了
- 对象运行：只要容器在，对象一直活着
- 对象销毁：当应用卸载，销毁容器时，对象就被销毁了

**2）当scope的取值为prototype时**

​	Bean的实例化个数：多个

​	Bean的实例化时机：当调用getBean()方法时实例化Bean

​	Bean的生命周期：

- 对象创建：当使用对象时，创建新的对象实例

- 对象运行：只要对象在使用中，就一直活着

- 对象销毁：当对象长时间不用时，被 Java 的垃圾回收器回收了



### 3.Bean生命周期配置

- **init-method**：指定类中的初始化方法名称

- **destroy-method**：指定类中销毁方法名称

```xml
 <bean id="userDao" class="com.lak.dao.impl.UserDaoImpl" scope="singleton" init-method="init" destroy-method="destroy"></bean>
```



### 4.Bean实例化三种方式

#### 4.1 无参构造实例化

它会根据默认无参构造方法来创建类对象，如果bean中没有默认无参构造函数，将会创建失败

```xml
<bean id="userDao" class="com.lak.dao.impl.UserDaoImpl"/>
```



#### 4.2 工厂**静态**方法实例化

```java
public class StaticFactory {
    public static UserDao getUserDao() {
        return new UserDaoImpl();
    }
}
```

```xml
<bean id="userDao" class="com.lak.factory.StaticFactory" factory-method="getUserDao"></bean>
```



#### 4.3 工厂**实例**方法实例化

工厂的非静态方法返回Bean实例

```java
public class DynamicFactory {
    public UserDao getUserDao() {
        return new UserDaoImpl();
    }
}
```

```xml
<bean id="factory" class="com.lak.factory.DynamicFactory"/>
<bean id="userDao" factory-bean="factory" factory-method="getUserDao"/>
```



### 5. Bean的依赖注入

#### 5.1  概念

> 依赖注入（**Dependency Injection**）：它是 Spring 框架核心 IOC 的具体实现。
>
> 在编写程序时，通过控制反转，把对象的创建交给了 Spring，但是代码中不可能出现没有依赖的情况。
>
> IOC 解耦只是降低他们的依赖关系，但不会消除。例如：业务层仍会调用持久层的方法。
>
> 那这种业务层和持久层的依赖关系，在使用 Spring 之后，就让 Spring 来维护了。
>
> 简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取



#### 5.2 注入方式

##### 5.2.1 set方法注入

1. 在UserServiceImpl中添加setUserDao方法

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao;
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save() {
        userDao.save();
    }
}
```

2. 配置Spring容器调用set方法进行注入

```xml
<bean id="userDao" class="com.lak.dao.impl.UserDaoImpl"></bean>
<bean id="userService" class="com.lak.service.impl.UserServiceImpl">
   <property name="userDao" ref="userDao"/>
</bean>
```

**p命名空间注入**

P命名空间注入本质也是set方法注入，但比起上述的set方法注入更加方便，主要体现在配置文件中，如下：

- 首先，需要引入P命名空间：

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

- 其次，需要修改注入方式

```xml
<bean id="userService" class="com.lak.service.impl.UserServiceImpl" p:userDao-ref="userDao"/>
```



##### 5.2.2 构造方法注入

创建有参构造

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao;

    //有参构造
    public UserServiceImpl(UserDao userDao) {
        this.userDao = userDao;
    }

    public UserServiceImpl() {
    }

    public void save() {
        userDao.save();
    }
}
```

<hr>

配置Spring容器调用有参构造时进行注入

```xml
<bean id="userDao" class="com.lak.dao.impl.UserDaoImpl"></bean>
<bean id="userService" class="com.lak.service.impl.UserServiceImpl" >
    <constructor-arg name="userDao" ref="userDao"></constructor-arg>
</bean>
```

<hr>

测试

```java
public class UserController {
    public static void main(String[] args) {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService= (UserService) app.getBean("userService");
        userService.save();
    }
}
```

<hr>

#### 5.3 注入的数据类型

​	上面的操作，都是注入的引用Bean，除了对象的引用可以注入，普通数据类型，集合等都可以在容器中进行注入。

注入数据的三种数据类型

- **普通数据类型**

- **引用数据类型**

- **集合数据类型**

​	其中引用数据类型，此处就不再赘述了，之前的操作都是对UserDao对象的引用进行注入的，下面将以set方法注入为例，演示普通数据类型和集合数据类型的注入。



##### 5.3.1 普通数据类型

```java
public class UserDaoImpl implements UserDao {

    private String username;
    private int age;

    public void setUsername(String username) {
        this.username = username;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public UserDaoImpl() {
        System.out.println("UserDaoImpl创建...");
    }

    public void save() {
        System.out.println(username+"=="+age);
    }
}
```



```xml
<bean id="userDao" class="com.lak.dao.impl.UserDaoImpl" >
       <property name="username" value="李桉轲"></property>
       <property name="age" value="23"></property>
</bean>
```



##### 5.3.2 集合数据类型

`List<String>`

```java
public class UserDaoImpl implements UserDao {

    private List<String> stringList;

    public void setStringList(List<String> stringList) {
        this.stringList = stringList;
    }

    public void save() {
        System.out.println(stringList);
    }
}
```



```xml
    <bean id="userDao" class="com.lak.dao.impl.UserDaoImpl" >
        <property name="stringList">
            <list>
                <value>aaa</value>
                <value>bbb</value>
                <value>ccc</value>
            </list>
        </property>
    </bean>
```

<hr>

`List<User>`

```java
public class UserDaoImpl implements UserDao {
	private List<User> userList;
	public void setUserList(List<User> userList) {
		this.userList = userList;
    }
    
	public void save() {
		System.out.println(userList);
    } 
}
```



```xml
<bean id="u1" class="com.lak.domain.User">
    <property name="name" value="tom"/>
    <property name="age" value="18"/>
</bean>

<bean id="u2" class="com.lak.domain.User">
    <property name="name" value="jack"/>
    <property name="age" value="20"/>    
</bean>

<bean id="userDao" class="com.lak.dao.impl.UserDaoImpl"> 
    <property name="userList"> 
        <list>
    		<ref bean="u1"/>
			<ref bean="u2"/>
		</list>
	</property>
</bean>
```

<hr>

`Map<String,User>`

```java
public class UserDaoImpl implements UserDao {
    private Map<String,User> userMap;
    public void setUserMap(Map<String, User> userMap) {
        this.userMap = userMap;
    }
    
    public void save() {
        System.out.println(userMap);
    } 
}
```



```xml
<bean id="u1" class="com.lak.domain.User">
    <property name="name" value="tom"/>
    <property name="age" value="18"/>
</bean>

<bean id="u2" class="com.lak.domain.User">
    <property name="name" value="jack"/>
    <property name="age" value="20"/>    
</bean>

<bean id="userDao" class="com.lak.dao.impl.UserDaoImpl"> 
    <property name="userMap"> 
        <map>
            <entry key="user1" value-ref="u1"/>
            <entry key="user2" value-ref="u2"/>
        </map>
	</property>
</bean>
```

<hr>

`Properties`

```java
public class UserDaoImpl implements UserDao {
    private Properties properties;
    public void setProperties(Properties properties) {
        this.properties = properties;
    }
    
    public void save() {
        System.out.println(properties);
    } 
}
```



```xml
<bean id="userDao" class="com.lak.dao.impl.UserDaoImpl"> 
    <property name="properties"> 
        <props> 
            <prop key="p1">ppp1</prop> 
            <prop key="p2">ppp2</prop> 
            <prop key="p3">ppp3</prop>
		</props>
	</property>
</bean>
```



### 6. 引入其他配置文件（分模块开发）

实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载

```xml
<import resource="applicationContext-xxx.xml"/>
```



Spring重点配置总结

```sh
<bean>标签
	id属性:在容器中Bean实例的唯一标识，不允许重复
	class属性:要实例化的Bean的全限定名
	scope属性:Bean的作用范围，常用是Singleton(默认)和prototype
	<property>标签：属性注入
		name属性：属性名称
		value属性：注入的普通属性值
		ref属性：注入的对象引用值
		<list>标签
		<map>标签
		<properties>标签
	<constructor-arg>标签
<import>标签:导入其他的Spring的分文件
```



## 四、API

### 1. ApplicationContext的继承体系

> applicationContext：接口类型，代表应用上下文，可以通过其实例获得 Spring 容器中的 Bean 对象

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220211181111982.png)



### 2. ApplicationContext的实现类

**1）ClassPathXmlApplicationContext**

它是从类的根路径下加载配置文件，推荐使用这种

**2）FileSystemXmlApplicationContext**

它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。

**3）AnnotationConfigApplicationContext**

当使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。



### 3. getBean()

```java
public Object getBean(String name) throws BeansException {
	assertBeanFactoryActive();
	return getBeanFactory().getBean(name);
}

public <T> T getBean(Class<T> requiredType) throws BeansException {
	assertBeanFactoryActive();
	return getBeanFactory().getBean(requiredType);
}
```

```java

ApplicationContext applicationContext = new 	
    ClassPathXmlApplicationContext("applicationContext.xml");

UserService userService1 = (UserService) 
applicationContext.getBean("userService");

UserService userService2 = applicationContext.getBean(UserService.class);
```

- 当参数的数据类型是字符串时，表示根据Bean的id从容器中获得Bean实例，返回是Object，需要强转。
- 当参数的数据类型是Class类型时，表示根据类型从容器中匹配Bean实例，当容器中相同类型的Bean有多个时，则此方法会报错。



# 第2章 IoC和DI注解开发

## 一、Spring配置数据源

### 1. 数据源（连接池）的作用

- 数据源(连接池)是提高程序性能而出现的

- 事先实例化数据源，初始化部分连接资源

- 使用连接资源时从数据源中获取

- 使用完毕后将连接资源归还给数据源

常见的数据源(连接池)：**DBCP、C3P0、BoneCP、Druid**等

<hr>

数据源的创建步骤

① 导入数据源的坐标和数据库驱动坐标

② 创建数据源对象

③ 设置数据源的基本连接数据

④ 使用数据源获取连接资源和归还连接资源

<hr>

### 2. 数据源的手动创建

#### 2.1 导入c3p0和druid的坐标 

*shift+Tab 左移删除空格*

*alt+左键 从光标处选中多行* 

```xml
<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1.2</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.23</version>
</dependency>
```

####	2.2 导入mysql数据库驱动坐标

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.32</version>
</dependency>
```



#### 2.3 创建C3P0或Druid连接池

```java
@Test
//测试手动创建c3p0数据源
public void testC3P0() throws Exception {
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    dataSource.setDriverClass("com.mysql.jdbc.Driver");
    dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");
    dataSource.setUser("root");
    dataSource.setPassword("123456");
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
    connection.close();
}
```

​	创建Druid连接池

```java
@Test
public void testDruid() throws Exception {
	//创建数据源
	DruidDataSource dataSource = new DruidDataSource();
	//设置数据库连接参数
	dataSource.setDriverClassName("com.mysql.jdbc.Driver");
	dataSource.setUrl("jdbc:mysql://localhost:3306/test");
	dataSource.setUsername("root");
	dataSource.setPassword("123456");
    //获得连接对象
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
    connection.close();
}
```



#### 2.4 提取jdbc.properties配置文件

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=123456
```



#### 2.5 读取jdbc.properties配置文件创建连接池

```java
@Test
public void testC3P0ByProperties() throws Exception {
	//加载类路径下的jdbc.properties
    ResourceBundle rb = ResourceBundle.getBundle("jdbc");
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    dataSource.setDriverClass(rb.getString("jdbc.driver"));
    dataSource.setJdbcUrl(rb.getString("jdbc.url"));
    dataSource.setUser(rb.getString("jdbc.username"));
    dataSource.setPassword(rb.getString("jdbc.password"));
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
}
```



### 3. Spring配置数据源

可以将DataSource的创建权交由Spring容器去完成

- DataSource有无参构造方法，而Spring默认就是通过无参构造方法实例化对象的

- DataSource要想使用需要通过set方法设置数据库连接信息，而Spring可以通过set方法进行字符串注入

#### 3.1 applicationContext.xml配置

```xml
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
    <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test"></property>
    <property name="user" value="root"></property>
    <property name="password" value="123456"></property>
</bean>
```



#### 3.2 测试

```java
@Test
//测试Spring容器产生数据源对象
public void test() throws Exception {
    ApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
    DataSource dataSource = app.getBean(DataSource.class);
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
    connection.close();
}
```



#### 3.3 抽取jdbc配置文件

applicationContext.xml加载jdbc.properties配置文件获得连接信息。

首先，需要引入context命名空间和约束路径：

- 命名空间：xmlns:context="http://www.springframework.org/schema/context"

- 约束路径：http://www.springframework.org/schema/context   http://www.springframework.org/schema/context/spring-context.xsd

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"> 		<property name="driverClass" value="${jdbc.driver}"/>
    <property name="jdbcUrl" value="${jdbc.url}"/>
    <property name="user" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
```

<hr>

**Spring容器加载properties文件**

```xml
<context:property-placeholder location="xx.properties"/>
<property name=" " value="${key}"/>
```



## 二、Spring注解开发

> Spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，注解代替xml配置文件可以简化配置，提高开发效率

### 1. Spring原始注解

Spring原始注解主要是替代`<Bean>`的配置

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220211212002525.png)



**注意：**使用注解进行开发时，需要在applicationContext.xml中配置组件扫描，作用是指定哪个包及其子包下的Bean，需要进行扫描以便识别使用注解配置的类、字段和方法。

```xml
<!--注解的组件扫描--> 
<context:component-scan base-package="com.lak"></context:component-scan>
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220211230842738.png)

<hr>

- 使用@Compont或@Repository标识UserDaoImpl需要Spring进行实例化

```java
//@Component("userDao")
@Repository("userDao")
public class UserDaoImpl implements UserDao {
	@Override
	public void save() {
		System.out.println("save running... ...");
    } 
}
```

- 使用@Compont或@Service标识UserServiceImpl需要Spring进行实例化

<hr>

- 使用@Autowired或者@Autowired+@Qulifier或者@Resource进行userDao的注入

```java
@Component("userService")
public class UserServiceImpl implements UserService {
    /* @Autowired
    @Qualifier("userDao") */
    @Resource(name="userDao")
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save() {
        userDao.save();
    }
}
```

<hr>

- 使用@Value进行字符串的注入

```java
@Repository("userDao")
public class UserDaoImpl implements UserDao {
    @Value("注入普通数据")
    private String str;
    @Value("${jdbc.driver}")
    private String driver;
    @Override
    public void save() {
        System.out.println(str);
        System.out.println(driver);
        System.out.println("save running... ...");
    } 
}
```

<hr>

- 使用@Scope标注Bean的范围

```java
//@Scope("prototype")
@Scope("singleton")
public class UserDaoImpl implements UserDao {
	//此处省略代码
}
```

<hr>

- 使用@PostConstruct标注初始化方法，使用@PreDestroy标注销毁方法

```java
@PostConstruct
public void init(){
	System.out.println("初始化方法....");
}

@PreDestroy
public void destroy(){
	System.out.println("销毁方法.....");
}
```

<hr>



### 2. Spring新注解

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220212083403431.png)

<hr>

- @Configuration
- @ComponentScan
- @Import

```java
// 标志该类是Spring核心配置类
@Configuration
// <context:component-scan base-package="com.lak"></context:component-scan>
@ComponentScan("com.lak")
@Import(DataSourceConfiguration.class)
public class SpringConfiguration {
}
```

<hr>

- @PropertySource

- @value

```java
@PropertySource("classpath:jdbc.properties")
public class DataSourceConfiguration {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;
}
```

- @Bean

```java
@Bean(name="dataSource")
public DataSource getDataSource() throws PropertyVetoException {
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    dataSource.setDriverClass(driver);
    dataSource.setJdbcUrl(url);
    dataSource.setUser(username);
    dataSource.setPassword(password);
    return dataSource; 
}
```

<hr>

**测试试加载核心配置类创建Spring容器**

```java
@Test
public void testAnnoConfiguration() throws Exception {
    
	ApplicationContext app = new 
		AnnotationConfigApplicationContext(SpringConfiguration.class);
    
    UserService userService = (UserService)app.getBean("userService");
    userService.save();
    
    DataSource dataSource = (DataSource)app.getBean("dataSource");
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
}
```



## 三、Spring整合Junit

### 1. 原始Junit测试Spring的问题及解决

- 问题

在测试类中，每个测试方法都有以下两行代码：

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
IAccountService as = ac.getBean("accountService",IAccountService.class);
```

这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

- 解决
  - 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它
  - 将需要进行测试Bean直接在测试类中进行注入



### 2. Spring集成Junit步骤

① 导入spring集成Junit的坐标

② 使用@Runwith注解替换原来的运行期

③ 使用@ContextConfiguration指定配置文件或配置类

④ 使用@Autowired注入需要测试的对象

⑤ 创建测试方法进行测试

**代码实现**

#### 2.1 导入spring集成Junit的坐标

```xml
<!--此处需要注意的是，spring5 及以上版本要求 junit 的版本必须是 4.12 及以上--> 
<dependency> 
    <groupId>org.springframework</groupId> 
    <artifactId>spring-test</artifactId> 
    <version>5.0.2.RELEASE</version>
</dependency> 

<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId> 
    <version>4.12</version> 
    <scope>test</scope>
</dependency>
```



#### 2.2 使用@Runwith注解替换原来的运行期

```java
@RunWith(SpringJUnit4ClassRunner.class)
public class SpringJunitTest {
}
```



#### 2.3 使用@ContextConfiguration指定配置文件或配置类

```java
@RunWith(SpringJUnit4ClassRunner.class)
//加载spring核心配置文件
//@ContextConfiguration(value = {"classpath:applicationContext.xml"})
//加载spring核心配置类
@ContextConfiguration(classes = {SpringConfiguration.class})
public class SpringJunitTest {
}
```



#### 2.4 使用@Autowired注入需要测试的对象

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {SpringConfiguration.class})
public class SpringJunitTest {
    @Autowired
    private UserService userService; 
}
```



#### 2.5 创建测试方法进行测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {SpringConfiguration.class})
public class SpringJunitTest {
    @Autowired
    private UserService userService;
    @Test
    public void testUserService(){
    	userService.save();
    } 
}
```

# 第3章 Spring集成Web环境

## 一、ApplicationContext应用上下文获取方式

- 应用上下文对象是通过new ClasspathXmlApplicationContext(spring配置文件)方式获取的，但是每次从容器中获得Bean时都要编写new ClasspathXmlApplicationContext(spring配置文件) ，这样的弊端是配置文件加载多次，应用上下文对象创建多次。

- 在Web项目中，可以使用ServletContextListener监听Web应用的启动，我们可以在Web应用启动时，就加载Spring的配置文件，创建应用上下文对象ApplicationContext，在将其存储到最大的域servletContext域中，这样就可以在任意位置从域中获得应用上下文ApplicationContext对象了。



## 二、 Spring提供获取应用上下文的工具

​	Spring提供了一个监听器<font color='red'>ContextLoaderListener</font>就是对上述功能的封装，该监听器内部加载Spring配置文件，创建应用上下文对象，并存储到ServletContext域中，提供了一个客户端工具<font color='red'>WebApplicationContextUtils</font>供使用者获得应用上下文对象。
​	所以我们需要做的只有两件事：
① 在web.xml中配置ContextLoaderListener监听器（导入spring-web坐标）
② 使用WebApplicationContextUtils获得应用上下文对象ApplicationContext

**步骤**

1. 导入Spring集成web的坐标

```xml
<dependency> 
    <groupId>org.springframework</groupId> 
    <artifactId>spring-web</artifactId> 
    <version>5.0.5.RELEASE</version>
</dependency>
```



2.  配置ContextLoaderListener监听器

```xml
<!--全局参数--> 
<context-param> 
    <param-name>contextConfigLocation</param-name> 
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>

<!--Spring的监听器--> 
<listener> 
    <listener-class>
        org.springframework.web.context.ContextLoaderListener
    </listener-class>
</listener>
```



3. 通过工具获得应用上下文对象

```java
ApplicationContext app = WebApplicationContextUtils.getWebApplicationContext(servletContext);
UserService userService = app.getBean(UserService.class);
```

