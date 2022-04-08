---
title: 06-MyBatis
date: 2022-02-14 07:48:43
tags: [Java]
---

# Mybatis

## 一、概述

### 1.  Mybatis概念

> mybatis 是一个优秀的基于java的持久层框架，它内部封装了jdbc，使开发者只需要关注sql语句本身，而不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。
>
> mybatis通过xml或注解的方式将要执行的各种statement配置起来，并通过java对象和statement中sql的动态参数进行映射生成最终执行的sql语句。
>
> 最后mybatis框架执行sql并将结果映射为java对象并返回。采用ORM思想解决了实体和数据库映射的问题，对jdbc 进行了封装，屏蔽了jdbc api 底层访问细节，使我们不用与jdbc api打交道，就可以完成对数据库的持久化操作。
>
> 官网：https://mybatis.org/mybatis-3/zh/index.html 

**持久层：**

* 负责将数据到保存到数据库的那一层代码。以后开发我们会将操作数据库的Java代码作为持久层。而Mybatis就是对jdbc代码进行了封装。

* JavaEE三层架构：表现层、业务层、持久层




### 2.  JDBC 缺点

- 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能

* 硬编码

  * 注册驱动、获取连接

    连接数据库的四个基本信息，以后如果要将Mysql数据库换成其他的关系型数据库的话，这四个地方都需要修改，如果放在此处就意味着要修改我们的源代码。

  * 如果表结构发生变化，SQL语句就要进行更改。这也不方便后期的维护。

* 操作繁琐

  * 手动设置参数

  * 手动封装结果集

  * 查询操作时，需要手动将结果集中的数据手动封装到实体中。插入操作时，需要手动将实体的数据设置到sql语句的占位

    符位置

    

应对上述问题给出的解决方案：

① 使用数据库连接池初始化连接资源

② 将sql语句抽取到xml配置文件中

③ 使用反射、内省等底层技术，自动将实体与表进行属性与字段的自动映射



### 3.  Mybatis 优化

* 硬编码可以配置到==配置文件==
* 操作繁琐的地方mybatis都==自动完成==

​	如图所示

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726204849309.png" alt="image-20210726204849309" style="zoom:80%;" />



## 二、快速入门

**需求：查询user表中所有的数据**

### 1. 导入MyBatis的坐标和其他相关坐标

在创建好的模块中的`pom.xml`配置文件中添加依赖的坐标

```xml
<dependencies>
    <!--mybatis 依赖-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.5</version>
    </dependency>

    <!--mysql 驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.46</version>
    </dependency>

    <!--junit 单元测试-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        <scope>test</scope>
    </dependency>

    <!-- 添加slf4j日志api -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.20</version>
    </dependency>
    
    <!-- 添加logback-classic依赖 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    
    <!-- 添加logback-core依赖 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.2.3</version>
    </dependency>
</dependencies>
```

注意：需要在项目的 resources 目录下创建logback的配置文件



### 2. 建user表

创建user表，添加数据

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214080314994.png)



### 3. 编写UserMapper映射文件 

> 统一管理sql语句，解决硬编码问题

在模块的 `resources/com.lak.mapper` 目录下创建映射配置文件 `UserMapper.xml`，内容如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="userMapper">
    <select id="selectAll" resultType="com.lak.domain.User">
        select * from user
    </select>
</mapper>
```



### 4. 编写MyBatis核心配置文件

> 替换连接信息 解决硬编码问题

* 在模块下的 resources 目录下创建mybatis的配置文件 `sqlMapConfig.xml`，内容如下：

  ```xml
  <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd"> 
  <configuration> 
      <!--数据源环境-->
      <environments default="development"> 
          <environment id="development"> 
              <transactionManagertype="JDBC"/>
              <dataSource type="POOLED"> 
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql:///test"/>
                  <property name="username" value="root"/>
                  <property name="password" value="123456"/>
  			</dataSource>
  		</environment>
  	</environments> 
      
      <!--加载映射文件-->
      <mappers> 
          <mapper resource="com/itheima/mapper/UserMapper.xml"/> 
      </mappers>
  </configuration>
  ```



### 5. 编写测试代码

在 `com.itheima.domain` 包下创建 User类

```java
public class User {
    private int id;
    private String username;
    private String password;
    //省略get set方法
}
```



在 `com.itheima.test` 包下编写 MybatisDemo 测试类

```java
public class MyBatisDemo {

    public static void main(String[] args) throws IOException {
        //1. 加载mybatis的核心配置文件 
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        
        //2. 获得SqlSessionFactory工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        
        //3. 获取SqlSession会话对象，用它来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        
        //4. 执行sql语句
        //参数是一个字符串，该字符串必须是映射配置文件的namespace.id
        List<User> userList = sqlSession.selectList("userMapper.selectAll"); 
        
        System.out.println(userList);
        
        //5. 释放资源
        sqlSession.close();
    }
}
```



**解决SQL映射文件的警告提示：**

在入门案例映射配置文件中存在报红的情况。问题如下：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726212621722.png" alt="image-20210726212621722" style="zoom:80%;" />

* 产生的原因：Idea和数据库没有建立连接，不识别表信息。但是它并不影响程序的执行。
* 解决方式：在Idea中配置MySQL数据库连接。



### 6. IDEA中配置MySQL数据库连接

* 点击IDEA右边框的 `Database` ，在展开的界面点击 `+` 选择 `Data Source` ，再选择 `MySQL`

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726213046072.png" alt="image-20210726213046072" style="zoom:80%;" />

* 在弹出的界面进行基本信息的填写

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726213305893.png" alt="image-20210726213305893" style="zoom:80%;" />

* 点击完成后就能看到如下界面

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726213541418.png" alt="image-20210726213541418" style="zoom:80%;" />

  而此界面就和 `navicat` 工具一样可以进行数据库的操作。也可以编写SQL语句

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726213857620.png" alt="image-20210726213857620" style="zoom:80%;" />



## 三、增删改查操作

### 1.  MyBatis的插入数据操作

**1.1 编写UserMapper映射文件**

```xml
<mapper namespace="userMapper"> 
    <insert id="add" parameterType="com.itheima.domain.User">
        insert into user values(#{id},#{username},#{password})
    </insert>
</mapper>
```



**1.2 编写插入实体User的代码**

```java
InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
SqlSession sqlSession = sqlSessionFactory.openSession();
int insert = sqlSession.insert("userMapper.add", user);
System.out.println(insert);
//提交事务
sqlSession.commit();
sqlSession.close();
```



**1.3 插入操作注意问题**

• 插入语句使用insert标签

• 在映射文件中使用parameterType属性指定要插入的数据类型

• Sql语句中使用#{实体属性名}方式引用实体中的属性值

• 插入操作使用的API是sqlSession.insert(“命名空间.id”,实体对象);

• 插入操作涉及数据库数据变化，所以要使用sqlSession对象显示的提交事务，即sqlSession.commit() 

<hr>

### 2. MyBatis的修改数据操作

**2.1 编写UserMapper映射文件**

```xml
<mapper namespace="userMapper"> 
    <update id="update" parameterType="com.itheima.domain.User">
        update user set username=#{username},password=#{password} where id=#{id}
	</update>
</mapper>
```



**2.2 编写修改实体User的代码**

```java
InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
SqlSession sqlSession = sqlSessionFactory.openSession();
int update = sqlSession.update("userMapper.update", user);
System.out.println(update);
sqlSession.commit();
sqlSession.close();
```



**2.3 修改操作注意问题**

• 修改语句使用update标签

• 修改操作使用的API是sqlSession.update(“命名空间.id”,实体对象);

<hr>

### 3. MyBatis的删除数据操作

**3.1 编写UserMapper映射文件**

```xml
<mapper namespace="userMapper"> 
    <delete id="delete" parameterType="java.lang.Integer">
        delete from user where id=#{id}
	</delete>
</mapper>
```



**3.2 编写删除数据的代码**

```java
InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
SqlSession sqlSession = sqlSessionFactory.openSession();
int delete = sqlSession.delete("userMapper.delete",3);
System.out.println(delete);
sqlSession.commit();
sqlSession.close();
```



**3.3 删除操作注意问题**

• 修改语句使用update标签

• 修改操作使用的API是sqlSession.update(“命名空间.id”,实体对象);

<hr>

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214085545487.png)



##  四、MyBatis相应API

### 1. SqlSession工厂构建器SqlSessionFactoryBuilder

常用API：SqlSessionFactory build(InputStream inputStream)

通过加载mybatis的核心文件的输入流的形式构建一个SqlSessionFactory对象

```java
String resource = "org/mybatis/builder/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
SqlSessionFactory factory = builder.build(inputStream);
```

其中， Resources 工具类，这个类在 org.apache.ibatis.io 包中。Resources 类帮助你从类路径下、文件系统或一个 web URL 中加载资源文件。



### 2. SqlSession工厂对象SqlSessionFactory

SqlSessionFactory 有多个个方法创建 SqlSession 实例。常用的有如下两个：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214093421920.png)



### 3. SqlSession会话对象

SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有执行语句、提交或回滚事务和获取映射器实例的方法。

执行语句的方法主要有：

```java
<T> T selectOne(String statement, Object parameter) 
<E> List<E> selectList(String statement, Object parameter) 
int insert(String statement, Object parameter) 
int update(String statement, Object parameter) 
int delete(String statement, Object parameter)
```

操作事务的方法主要有:

```java
void commit()
void rollback()
```



## 五、核心配置文件

**MyBatis核心配置文件层级关系**

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726221454927.png" alt="image-20210726221454927" style="zoom:80%;" />



### 1.  environments标签

在核心配置文件的 `environments` 标签中其实是可以配置多个 `environment` ，使用 `id` 给每段环境起名，在 `environments` 中使用 `default="环境id"` 来指定使用哪儿段配置。我们一般就配置一个 `environment` 即可。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214090849264.png)

其中，事务管理器（transactionManager）类型有两种：

- JDBC：这个配置就是直接使用了JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。

- MANAGED：这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如JEE应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。



其中，数据源（dataSource）类型有三种：

- UNPOOLED：这个数据源的实现只是每次被请求时打开和关闭连接。

- POOLED：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来。

- JNDI：这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。



### 2. mapper标签

该标签的作用是加载映射的，加载方式有如下几种：

- 使用相对于类路径的资源引用，例如：`<mapper resource="org/mybatis/builder/AuthorMapper.xml"/>`

- 使用完全限定资源定位符（URL），例如：`<mapper url="file:///var/mappers/AuthorMapper.xml"/>`

- 使用映射器接口实现类的完全限定类名，例如：`<mapper class="org.mybatis.builder.AuthorMapper"/>`

- 将包内的映射器接口实现全部注册为映射器，例如：`<package name="org.mybatis.builder"/>`



### 3. Properties标签

实际开发中，习惯将数据源的配置信息单独抽取成一个properties文件，该标签可以加载额外配置的properties文件

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214091043257.png)



### 4. typeAliases标签

类型别名是为Java 类型设置一个短的名字。原来的类型名称配置如下

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214092245680.png)

配置typeAliases，为com.itheima.domain.User定义别名为user

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214092311532.png)

上面我们是自定义的别名，mybatis框架已经为我们设置好的一些常用的类型的别名

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214092336744.png)

<hr>

别名idea提示报错但运行成功：插件问题——安装MybatisCodeHelper

<hr>



### 5.typeHandlers标签

无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器（截取部分）。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214101311475.png)



可以重写类型处理器或创建自己的类型处理器来处理不支持的或非标准的类型。

具体做法为：实现org.apache.ibatis.type.TypeHandler 接口， 或继承一个很便利的类 org.apache.ibatis.type.BaseTypeHandler， 然后可以选择性地将它映射到一个JDBC类型。

例如需求：一个Java中的Date数据类型，想将之存到数据库的时候存成一个1970年至今的毫秒数，取出来时转换成java的Date，即java的Date与数据库的varchar毫秒值之间转换。



开发步骤：

① 定义转换类继承类`BaseTypeHandler<T>`。

② 覆盖4个未实现的方法，其中`setNonNullParameter`为java程序设置数据到数据库的回调方法，`getNullableResult`为查询时 mysql的字符串类型转换成 java的Type类型的方法。

③ 在MyBatis核心配置文件中进行注册。

④ 测试转换是否正确。

```java
public class MyDateTypeHandler extends BaseTypeHandler<Date> {
    //j
    public void setNonNullParameter(PreparedStatement preparedStatement, int i, Date date, JdbcType type) {
        preparedStatement.setString(i,date.getTime()+"");
    }
    
	public Date getNullableResult(ResultSet resultSet, String s) throws SQLException {
		return new Date(resultSet.getLong(s));
    }
    
	public Date getNullableResult(ResultSet resultSet, int i) throws SQLException {
		return new Date(resultSet.getLong(i));
    }
	public Date getNullableResult(CallableStatement callableStatement, int i) throws SQLException {
		return callableStatement.getDate(i);
    } 
}
```

```xml
<!--注册类型自定义转换器--> 
<typeHandlers> 
    <typeHandler handler="com.itheima.typeHandlers.MyDateTypeHandler"></typeHandler>
</typeHandlers>
```

测试添加操作：

```java
user.setBirthday(new Date());
userMapper.add2(user);
```

数据库数据：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214101449870.png)



测试查询操作：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214101509261.png)



### 6. plugins标签

MyBatis可以使用第三方的插件来对功能进行扩展，分页助手PageHelper是将分页的复杂操作进行封装，使用简单的方式即可获得分页的相关数据

开发步骤：

① 导入通用PageHelper的坐标

```xml
<!-- 分页助手 --> 
<dependency> 
    <groupId>com.github.pagehelper</groupId> 
    <artifactId>pagehelper</artifactId> <version>3.7.5</version>
</dependency> 

<dependency> 
    <groupId>com.github.jsqlparser</groupId> 
    <artifactId>jsqlparser</artifactId> 
    <version>0.9.1</version>
</dependency>
```



② 在mybatis核心配置文件中配置PageHelper插件

```xml
<!-- 注意：分页助手的插件 配置在通用馆mapper之前 --> 
<plugin interceptor="com.github.pagehelper.PageHelper">
<!-- 指定方言 --> 
    <property name="dialect" value="mysql"/>
</plugin>
```



③ 测试分页数据获取

```java
@Test
public void testPageHelper(){
//设置分页参数
    PageHelper.startPage(1,2);
    List<User> select = userMapper2.select(null);
    for(User user : select){
    System.out.println(user);
    } 
}
```



获得分页相关的其他参数

```java
//其他分页的数据
PageInfo<User> pageInfo = new PageInfo<User>(select);
System.out.println("总条数："+pageInfo.getTotal());
System.out.println("总页数："+pageInfo.getPages());
System.out.println("当前页："+pageInfo.getPageNum());
System.out.println("每页显示长度："+pageInfo.getPageSize());
System.out.println("是否第一页："+pageInfo.isIsFirstPage());
System.out.println("是否最后一页："+pageInfo.isIsLastPage());
```



## 六、MyBatis的dao层实现

### 1. 代理开发方式

*传统开发方式*

*1) 编写UserDao接口*

*2) 编写UserDaoImpl实现*

<hr>

>采用 Mybatis 的代理开发方式实现 DAO 层的开发，这种方式是我们后面进入企业的主流。
>
>Mapper 接口开发方法只需要程序员编写Mapper 接口（相当于Dao 接口），由Mybatis 框架根据接口定义创建接口的动态代理对象，代理对象的方法体同上边Dao接口实现类方法。

Mapper 接口开发需要遵循以下规范：

- Mapper.xml文件中的namespace与mapper接口的全限定名相同

- Mapper接口方法名和Mapper.xml中定义的每个statement的id相同

- Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同

- Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同



### 2. Mapper代理开发

#### 2.1 概述

之前我们写的代码是基本使用方式，它也存在硬编码的问题，如下：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726214648112.png" alt="image-20210726214648112" style="zoom:80%;" />

这里调用 `selectList()` 方法传递的参数是映射配置文件中的 namespace.id值。这样写也不便于后期的维护。如果使用 Mapper 代理方式（如下图）则不存在硬编码问题。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726214636108.png" alt="image-20210726214636108" style="zoom:80%;" />

通过上面的描述可以看出 Mapper 代理方式的目的：

* 解决原生方式中的硬编码
* 简化后期执行SQL



#### 2.2 要求

使用Mapper代理方式，必须满足以下要求：

* 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下。如下图：

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726215946951.png" alt="image-20210726215946951" style="zoom:80%;" />

* 设置SQL映射文件的namespace属性为Mapper接口全限定名

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726220053883.png" alt="image-20210726220053883" style="zoom:80%;" />

* 在 Mapper 接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726223216517.png" alt="image-20210726223216517" style="zoom:70%;" />



#### 3. 代码实现

* 在 `com.itheima.mapper` 包下创建 UserMapper接口，代码如下：

  ```java
  public interface UserMapper {
      List<User> selectAll();
      User selectById(int id);
  }
  ```

* 在 `resources` 下创建 `com/itheima/mapper` 目录，并在该目录下创建 UserMapper.xml 映射配置文件

  ```xml
  <!--
      namespace:名称空间。必须是对应接口的全限定名
  -->
  <mapper namespace="com.itheima.mapper.UserMapper">
      <select id="selectAll" resultType="com.itheima.pojo.User">
          select *
          from tb_user;
      </select>
  </mapper>
  ```

* 在 `com.itheima` 包下创建 MybatisDemo2 测试类，代码如下：

  ```java
  /**
   * Mybatis 代理开发
   */
  public class MyBatisDemo2 {
  
      public static void main(String[] args) throws IOException {
  
          //1. 加载mybatis的核心配置文件，获取 SqlSessionFactory
          String resource = "mybatis-config.xml";
          InputStream inputStream = Resources.getResourceAsStream(resource);
          SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  
          //2. 获取SqlSession对象，用它来执行sql
          SqlSession sqlSession = sqlSessionFactory.openSession();
          //3. 执行sql
          //3.1 获取UserMapper接口的代理对象
          UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
          List<User> users = userMapper.selectAll();
  
          System.out.println(users);
          //4. 释放资源
          sqlSession.close();
      }
  }
  ```

==注意：==

如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简化SQL映射文件的加载。也就是将核心配置文件的加载映射配置文件的配置修改为

```xml
<mappers>
    <!--加载sql映射文件-->
    <!-- <mapper resource="com/itheima/mapper/UserMapper.xml"/>-->
    <!--Mapper代理方式-->
    <package name="com.itheima.mapper"/>
</mappers>
```



## 七、映射文件

### 1. 概述

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214084130347.png)

### 2. 动态sql语句

#### 2.1 `<if>`

我们根据实体类的不同取值，使用不同的 SQL语句来进行查询。比如在 id如果不为空时可以根据id查询，如果username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到。

```xml
<select id="findByCondition" parameterType="user" resultType="user">
    select * from User
    <where> 
        <if test="id!=0">
            and id=#{id}
        </if> 
        <if test="username!=null">
            and username=#{username}
        </if>
    </where>
</select>
```



- 当查询条件id和username都存在时，控制台打印的sql语句如下：

```java
//获得MyBatis框架生成的UserMapper接口的实现类
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
User condition = new User();
condition.setId(1);
condition.setUsername("lucy");
User user = userMapper.findByCondition(condition);
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214100419863.png)



- 当查询条件只有id存在时，控制台打印的sql语句如下：

```java
//获得MyBatis框架生成的UserMapper接口的实现类
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
User condition = new User();
condition.setId(1);
User user = userMapper.findByCondition(condition);
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214100548164.png)

<hr>

查询条件为中文时查不出结果

`jdbc.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=UTF-8`

<hr>

先写UserMapper接口里的方法，然后自动生成UserMapper.xml里的sql语句

<hr>



#### 2.2 `<foreach>`

循环执行sql的拼接操作，例如：SELECT * FROM USER WHERE id IN (1,2,5)。 

```xml
<select id="findByIds" parameterType="list" resultType="user">
    select * from User
    <where> 
        <foreach collection="list" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

```java
//测试代码
//获得MyBatis框架生成的UserMapper接口的实现类
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
List<Integer> ids = new ArrayList<Integer>();
ids.add(1);
ids.add(2);
List<User> userList= mapper.findByIds(ids);
System.out.println(userList);
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220214100858561.png)

foreach标签的属性含义如下：

`<foreach>`标签用于遍历集合，它的属性：

- collection：代表要遍历的集合元素，注意编写时不要写#{}

- open：代表语句的开始部分

- close：代表结束部分

- item：代表遍历集合的每个元素，生成的变量名

- sperator：代表分隔符



### 3. sql片段抽取

`<sql>`标签

Sql 中可将重复的 sql 提取出来，使用时用 include 引用即可，最终达到 sql 重用的目的

```xml
<mapper namespace="com.lak.dao.UserMapper">

    <!--抽取sql片段简化编写-->
    <sql id="selectUser">SELECT * FROM user</sql>
    
    <select id="selectByCondition" parameterType="user" resultType="user">
        <!--select * from user-->
        <include refid="selectUser"></include>
        <where>
            <if test="id!=0">
                and id=#{id}
            </if>
            <if test="username!=null">
                and username=#{username}
            </if>

        </where>
    </select>

    <select id="findByIds" resultType="com.lak.domain.User" parameterType="list">
        <!--select * from user-->
        <include refid="selectUser"></include>
        <where>
            <foreach collection="list" open="id in(" close=")" item="id" separator=",">#{id}</foreach>
        </where>
    </select>
</mapper>
```



## 八、多表操作

### 1. 一对一查询

#### 1.1 查询模型

用户表和订单表的关系为，一个用户有多个订单，一个订单只从属于一个用户

一对一查询的需求：查询一个订单，与此同时查询出该订单所属的用户

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219112527188.png)



#### 1.2 查询语句

```sql
select * from orders o,user u where o.uid=u.id;
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219112334164.png)



#### 1.3 创建Order和User实体

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219122422666.png)



#### 1.4 创建OrderMapper接口

```java
public interface OrderMapper {
    List<Order> findAll();
}
```



#### 1.5 配置OrderMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.lak.dao.OrderMapper">
    <resultMap id="orderMap" type="order">
        <!--手动指定字段与实体属性的映射关系
            column: 数据表的字段名称
            property：实体的属性名称
            -->
        <id column="oid" property="id"></id>
        <result column="ordertime" property="ordertime"></result>
        <result column="total" property="total"></result>

		<!--        
			<result column="uid" property="user.id"></result>
      		<result column="username" property="user.username"></result>
      	 	<result column="password" property="user.password"></result>
		-->

        <!--
            property: 当前实体（order）中的属性名称（private User user）
            javaType: 当前实体（order）中的属性的类型（User）
        -->
        <association property="user" javaType="user">
            <id column="uid" property="id"></id>
            <result column="username" property="username"></result>
            <result column="password" property="password"></result>
        </association>
    </resultMap>

    <select id="findAll" resultMap="orderMap">
        select *,o.id oid from orders o,user u where o.uid=u.id;
    </select>
</mapper>
```



#### 1.6 测试结果

```java
@Test
public void test2() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("Config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    OrderMapper mapper = sqlSession.getMapper(OrderMapper.class);
    List<Order> orderList= mapper.findAll();
    for (Order order : orderList) {
        System.out.println(order);
    }
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219122827854.png)



### 2. 一对多查询

#### 2.1 查询模型

一对多查询的需求：查询一个用户，与此同时查询出该用户具有的订单



#### 2.2 查询语句

```sql
select *,o.id oid from user u left join orders o on u.id=o.uid;
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219132018970.png)



#### 2.3 修改User实体

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219132244652.png)



#### 2.4 创建UserMapper接口

```java
public interface UserMapper {   
    List<User> findAll();
}
```



#### 2.5 配置UserMapper.xnl

```xml
<mapper namespace="com.lak.dao.UserMapper">
    <resultMap id="userMap" type="user">
        <id column="id" property="id"></id>
        <result column="username" property="username"></result>
        <result column="password" property="password"></result>
        <!--配置集合信息
            property: 集合名称
            ofType: 当前集合中的数据类型
        -->
        <collection property="orderList" ofType="order">
            <result column="oid" property="id"></result>
            <result column="ordertime" property="ordertime"></result>
            <result column="total" property="total"></result>
        </collection>
    </resultMap>

    <select id="findAll" resultMap="userMap">
        select *,o.id oid from user u left join orders o on u.id=o.uid;
    </select>
</mapper>
```



#### 2.6 测试结果

```java
@Test
public void test3() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("Config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList= mapper.findAll();
    for (User user : userList) {
        System.out.println(user);
    }

}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219135222056.png)



### 3. 多对多查询

#### 3.1 查询模型

用户表和角色表的关系为，一个用户有多个角色，一个角色被多个用户使用

多对多查询的需求：查询用户同时查询出该用户的所有角色

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219135644055.png)



#### 3.2 查询语句

```sql
select * from user u ,user_role ur, role r where u.id=ur.user_id and ur.role_id=r.id;
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219142248470.png)



#### 3.3 创建Role实体，修改User实体

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219141036282.png)



#### 3.4 添加UserMapper接口方法

```java
List<User> findAllUserAndRole();
```



#### 3.5 配置UserMapper.xml

```xml
<resultMap id="userRoleMap" type="user">
    <id column="id" property="id"></id>
    <result column="username" property="username"></result>
    <result column="password" property="password"></result>
    <collection property="roleList" ofType="role">
        <id column="role_id" property="id"></id>
        <id column="rolename" property="rolename"></id>
    </collection>
</resultMap>
<select id="findAllUserAndRole" resultMap="userRoleMap">
    select * from user u ,user_role ur, role r where u.id=ur.user_id and ur.role_id=r.id;
</select>
```



#### 3.6 测试

```java
 @Test
public void test4() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("Config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList= mapper.findAllUserAndRole();
    for (User user : userList) {
        System.out.println(user);
    }
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219142400440.png)

<hr>

注：column与数据库列名一致

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219145137533.png)

```sql
select * from user u ,user_role ur, role r where u.id=ur.user_id and ur.role_id=r.id;
```



- 若user表和role表同时有id列，则这里不能写id，查询出来的是user.id(也有可能是role.id，得看sql语句查询时把哪个表放前面)，只能写role_id
- 若user表有id列，role表有rid列，则这里可以写rid或role_id
- idea会有提示

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219143253473.png)

| `column` | 数据库中的列名，或者是列的别名。一般情况下，这和传递给 `resultSet.getString(columnName)` 方法的参数一样。 |
| -------- | ------------------------------------------------------------ |

<hr>



## 九、注解开发

### 1. MyBatis常用注解

```
@Insert：实现新增
@Update：实现更新
@Delete：实现删除
@Select：实现查询
@Result：实现结果集封装
@Results：可以与@Result一起使用，封装多个结果集
@One：实现一对一结果集封装
@Many：实现一对多结果集封装
```



### 2. 增删改查

**UserMapper接口**

```java
public interface UserMapper {

    @Insert("insert into user values (#{id} ,#{username} ,#{password} )")
    void save(User user);

    @Update("update user set username=#{username} ,password=#{password} where id =#{id} ")
    void update(User u );

    @Delete("delete from user where id=#{id}")
    void delete(int id);

    @Select("select * from user where id=#{id} ")
    User findById(int id);

    @Select("select * from user")
    List<User> findAll();
}
```



**Config.xml**

修改MyBatis的核心配置文件，使用了注解替代的映射文件，所以只需要加载使用了注解的Mapper接口即可

```xml
<!--加载映射关系-->
<mappers>
    <!--法一-->
    <package name="com.lak.dao"></package>
    <!--法二-->
    <mapper class="com.lak.dao.UserMapper"></mapper>
</mappers>
```



**Test.java**

```java
public class TestDemo {
    private UserMapper userMapper;

    @Before
    public void before() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("Config.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        userMapper = sqlSession.getMapper(UserMapper.class);
    }

    @Test
    //增
    public void testSave() throws IOException {
        User user = new User();
        user.setUsername("李hh");
        user.setPassword("321");
        userMapper.save(user);
    }

    @Test
    //删
    public void testDelete() throws IOException {
        userMapper.delete(9);
    }

    //查
    @Test
    public void testSelect() throws IOException {
        User user = userMapper.findById(2);
        System.out.println(user);
        System.out.println("----------------");
        List<User> userList= userMapper.findAll();
        for (User user1 : userList) {
            System.out.println(user1);
        }
    }

    //改
    @Test
    public void testUpdate() throws IOException {
        User user = new User();
        user.setId(1);
        user.setUsername("李天");
        user.setPassword("321");
        userMapper.update(user);
    }
}
```

注：增删改需要提交事务生效

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219183539320.png)



### 3.  MyBatis注解实现复杂映射开发

实现复杂关系映射之前我们可以在映射文件中通过配置`<resultMap>`来实现，使用注解开发后，我们可以使用@Results注解，@Result注解，@One注解，@Many注解组合完成复杂关系的配置

| 注解             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| @Results         | 代替的是标签`<resultMap>`。该注解中可以使用单个@Result注解，也可以使用@Result集合。使用格式：@Results（{@Result（），@Result（）}）或@Results（@Result（）） |
| @Result          | 代替了`<id>`标签和`<result>`标签
@Result中属性介绍：
   column：数据库的列名
   property：需要装配的属性名
   one：需要使用的@One 注解（@Result（one=@One）（）））
   many：需要使用的@Many 注解（@Result（many=@many）（））） |
| @One （一对一）  | 代替了`<assocation> `标签，是多表查询的关键，在注解中用来指定子查询返回单一对象。
@One注解属性介绍：select: 指定用来多表查询的 sqlmapper
使用格式：@Result(column=" ",property="",one=@One(select="")) |
| @Many （多对一） | 代替了`<collection>`标签, 是是多表查询的关键，在注解中用来指定子查询返回对象集合。
使用格式：@Result(property="",column="",many=@Many(select="")) |



### 4. 一对一查询

一对一查询的需求：查询一个订单，与此同时查询出该订单所属的用户

```java
//1
public interface OrderMapper {
    @Select("select *,o.id oid from orders o,user u where o.uid=u.id")
    @Results({
            @Result(column = "oid",property = "id"),
            @Result(column = "ordertime",property = "ordertime"),
            @Result(column = "total",property = "total"),
            @Result(column = "uid",property = "user.id"),
            @Result(column = "username",property = "user.username"),
            @Result(column = "password",property = "user.password")
    })
    List<Order> findAll();
}

//2
public interface OrderMapper {
    @Select("select * from orders")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "ordertime",property = "ordertime"),
            @Result(column = "total",property = "total"),
            @Result(
                    property = "user", //要封装的属性名称
                    column = "uid", //根据哪个字段去查询user表的数据
                    javaType = User.class, //要封装的实体类型
                    //select属性 代表查询哪个接口的方法获得数据
                    one = @One(select = "com.lak.dao.UserMapper.findById")
            )
    })
    List<Order> findAll();
}

public interface UserMapper {
    @Select("select * from user where id=#{id} ")
    User findById(int id);
}
```

```java
 @Test
public void testFindAll() {
    List<Order> all = orderMapper.findAll();
    for (Order order : all) {
        System.out.println(order);
    }
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219202515279.png)

<hr>

映射文件存在时，里面的错误会影响注解方式的生效



### 5. 一对多查询

一对多查询的需求：查询一个用户，与此同时查询出该用户具有的订单

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219205059673.png)



### 6. 多对多查询

多对多查询的需求：查询用户同时查询出该用户的所有角色

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220219205131755.png)
