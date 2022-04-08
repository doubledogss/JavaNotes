title: 08-springboot开发实用篇
date: 2022-03-02 16:53:09
tags:

# SpringBoot开发实用篇

## 第1章 热部署

> 热部署简单说就是程序改了，不用重启服务器，服务器会自己把更新后的程序给重新加载一遍。



**非springboot项目热部署实现原理**

​		开发非springboot项目时，我们要制作一个web工程并通过tomcat启动，通常需要先安装tomcat服务器到磁盘中，开发的程序配置发布到安装的tomcat服务器上。如果想实现热部署的效果，这种情况其实有两种做法，一种是在tomcat服务器的配置文件中进行配置，这种做法与你使用什么IDE工具无关，不管你使用eclipse还是idea都行。还有一种做法是通过IDE工具进行配置，比如在idea工具中进行设置，这种形式需要依赖IDE工具，每款IDE工具不同，对应的配置也不太一样。但是核心思想是一样的，就是使用服务器去监控其中加载的应用，发现产生了变化就重新加载一次。



**springboot项目热部署实现原理**

​		基于springboot开发的web工程其实有一个显著的特征，就是tomcat服务器内置了，还记得内嵌服务器吗？服务器是以一个对象的形式在spring容器中运行的。本来我们期望于tomcat服务器加载程序后由tomcat服务器盯着程序，你变化后我就重新启动重新加载，但是现在tomcat和我们的程序是平级的了，都是spring容器中的组件，这下就麻烦了，缺乏了一个直接的管理权，那该怎么做呢？简单，再搞一个程序X在spring容器中盯着你原始开发的程序A不就行了吗？确实，搞一个盯着程序A的程序X就行了，如果你自己开发的程序A变化了，那么程序X就命令tomcat容器重新加载程序A就OK了。并且这样做有一个好处，spring容器中东西不用全部重新加载一遍，只需要重新加载你开发的程序那一部分就可以了，这下效率又高了，挺好。

​	下面就说说，怎么搞出来这么一个程序X，肯定不是我们自己手写了，springboot早就做好了，搞一个坐标导入进去就行了。

### 一、手动启动热部署





### 二、自动启动热部署





### 三、热部署范围配置





### 四、关闭热部署







## 第2章 配置高级



## 第3章 测试



## 第4章 数据层解决方案

### 一、SQL

现在数据层解决方案可以说是Mysql+Druid+MyBatisPlus。而三个技术分别对应了数据层操作的三个层面：

- 数据源技术：Druid
- 持久化技术：MyBatisPlus
- 数据库技术：MySQL

下面的研究就分为三个层面进行研究，对应上面列出的三个方面。



#### 1. 数据源技术

目前我们使用的数据源技术是Druid，运行时可以在日志中看到对应的数据源初始化信息，具体如下：

```CMD
INFO 28600 --- [           main] c.a.d.s.b.a.DruidDataSourceAutoConfigure : Init DruidDataSource
INFO 28600 --- [           main] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} inited
```

将Druid技术对应的starter去掉再次运行程序可以在日志中找到如下初始化信息：

```CMD
INFO 31820 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
INFO 31820 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
```

虽然没有DruidDataSource相关的信息，但日志中有HikariDataSource——springboot内嵌数据源。

<hr>

springboot提供了3款内嵌数据源技术，分别如下：

- HikariCP
  - 这是springboot官方推荐的数据源技术，作为默认内置数据源使用。不配置数据源就用这个。

- Tomcat提供DataSource
  - 如果不想用HikartCP，并且使用tomcat作为web服务器进行web程序的开发，使用这个。
  - 为什么是Tomcat，不是其他web服务器呢？因为web技术导入starter后，默认使用内嵌tomcat，既然是默认使用的技术，那就数据源也用它的。
  - 怎么才能不使用HikartCP用tomcat提供的默认数据源对象？把HikartCP技术的坐标排除掉。

- Commons DBCP
  - 这个使用的条件就更苛刻，既不使用HikartCP也不使用tomcat的DataSource时，默认用这个。


<hr>

之前配置druid时使用druid的starter对应的配置如下：

```YAML
spring:
  datasource:
    druid:	
   	  url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: 123456
```



换成是默认的数据源HikariCP后，直接吧druid删掉就行了，如下：

```YAML
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 123456
```



也可以写上是对hikari做的配置，但是url地址要单独配置，如下：

```YAML
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    hikari:
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: 123456
```

这就是配置hikari数据源的方式。

如果想对hikari做进一步的配置，可以继续配置其独立的属性。例如：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    hikari:
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: 123456
      maximum-pool-size: 50
```

如果不想使用hikari数据源，使用tomcat的数据源或者DBCP配置格式也是一样的。



#### 2. 持久化技术

说完数据源解决方案，再来说一下持久化解决方案。springboot充分发挥其最强辅助的特征，给开发者提供了一套现成的数据层技术，叫做JdbcTemplate。其实这个技术不能说是springboot提供的，因为不使用springboot技术，一样能使用它，谁提供的呢？spring技术提供的，所以在springboot技术范畴中，这个技术也是存在的，毕竟springboot技术是加速spring程序开发而创建的。

这个技术其实就是回归到jdbc最原始的编程形式来进行数据层的开发，下面直接上操作步骤：

**步骤①**：导入jdbc对应的坐标，记得是starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency
```

**步骤②**：自动装配JdbcTemplate对象

```java
@SpringBootTest
class Springboot15SqlApplicationTests {
    @Test
    void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
    }
}
```

**步骤③**：使用JdbcTemplate实现查询操作（非实体类封装数据的查询操作）

```java
@Test
void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
    String sql = "select * from tbl_book";
    List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
    System.out.println(maps);
}
```

**步骤④**：使用JdbcTemplate实现查询操作（实体类封装数据的查询操作）

```java
@Test
void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){

    String sql = "select * from tbl_book";
    RowMapper<Book> rm = new RowMapper<Book>() {
        @Override
        public Book mapRow(ResultSet rs, int rowNum) throws SQLException {
            Book temp = new Book();
            temp.setId(rs.getInt("id"));
            temp.setName(rs.getString("name"));
            temp.setType(rs.getString("type"));
            temp.setDescription(rs.getString("description"));
            return temp;
        }
    };
    List<Book> list = jdbcTemplate.query(sql, rm);
    System.out.println(list);
}
```

**步骤⑤**：使用JdbcTemplate实现增删改操作

```java
@Test
void testJdbcTemplateSave(@Autowired JdbcTemplate jdbcTemplate){
    String sql = "insert into tbl_book values(3,'springboot1','springboot2','springboot3')";
    jdbcTemplate.update(sql);
}
```

​		如果想对JdbcTemplate对象进行相关配置，可以在yml文件中进行设定，具体如下：

```yaml
spring:
  jdbc:
    template:
      query-timeout: -1   # 查询超时时间
      max-rows: 500       # 最大行数
      fetch-size: -1      # 缓存行数
```

**总结**

1. SpringBoot内置JdbcTemplate持久化解决方案
2. 使用JdbcTemplate需要导入spring-boot-starter-jdbc的坐标



#### 3. 数据库技术

​		截止到目前，springboot给开发者提供了内置的数据源解决方案和持久化解决方案，在数据层解决方案三件套中还剩下一个数据库。springboot也提供有内置的3个数据库，分别是：

- H2
- HSQL
- Derby

​		以上三款数据库除了可以独立安装之外，还可以像是tomcat服务器一样，采用内嵌的形式运行在spirngboot容器中。内嵌在容器中运行，必须是java对象，这三款数据库底层都是使用java语言开发的。

​		我们一直使用MySQL数据库就挺好的，为什么有需求用这个呢？原因就在于这三个数据库都可以采用内嵌容器的形式运行，在应用程序运行后，如果我们进行测试工作，此时测试的数据无需存储在磁盘上，但是又要测试使用，内嵌数据库就方便了，运行在内存中，该测试测试，该运行运行，等服务器关闭后，一切烟消云散，省得维护外部数据库。这也是内嵌数据库的最大优点，方便进行功能测试。

​		下面以H2数据库为例讲解如何使用这些内嵌数据库。

**步骤①**：导入H2数据库对应的坐标，一共2个

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

**步骤②**：将工程设置为web工程，启动工程时启动H2数据库

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**步骤③**：通过配置开启H2数据库控制台访问程序，也可以使用其他的数据库连接软件操作

```yaml
spring:
  h2:
    console:
      enabled: true
      path: /h2
```

​		web端访问路径/h2，访问密码123456，如果访问失败，先配置下列数据源，启动程序运行后再次访问/h2路径就可以正常访问了

```yaml
datasource:
  url: jdbc:h2:~/test
  hikari:
    driver-class-name: org.h2.Driver
    username: sa
    password: 123456
```

**步骤④**：使用JdbcTemplate或MyBatisPlus技术操作数据库



​		其实我们只是换了一个数据库而已，其他的东西都不受影响。一个重要提醒，别忘了，上线时，把内存级数据库关闭，采用MySQL数据库作为数据持久化方案，关闭方式就是设置enabled属性为false即可。

**总结**

1. H2内嵌式数据库启动方式，添加坐标，添加配置
2. H2数据库线上运行时请务必关闭



​		到这里SQL相关的数据层解决方案就讲完了，现在的可选技术就丰富的多了。

- 数据源技术：Druid、Hikari、tomcat DataSource、DBCP
- 持久化技术：MyBatisPlus、MyBatis、JdbcTemplate
- 数据库技术：MySQL、H2、HSQL、Derby

​		现在开发程序时就可以在以上技术中任选一种组织成一套数据库解决方案了。



### 二、NoSQL

> NoSQL就是非关系型数据库。本节讲解的内容就是springboot如何整合这些技术，在springboot官方文档中提供了10种相关技术的整合方案，我们将讲解国内市场上最流行的几款NoSQL数据库整合方案，分别是Redis、MongoDB、ES。
>
> 上述这些技术最佳使用方案都是在Linux服务器上部署，但下面的课程都是以Windows平台作为安装基础讲解。



#### 1. SpringBoot整合Redis

> Redis是一款采用key-value数据存储格式的内存级NoSQL数据库，重点关注数据存储格式，是key-value格式，并且数据以存储在内存中使用为主。其实Redis有它的数据持久化方案，分别是RDB和AOF，但是Redis自身并不是为了数据持久化而生的，主要是在内存中保存数据，加速数据访问的，所以说是一款内存级数据库。
>
> - 支持多种数据存储格式
> - 支持持久化
> - 支持集群



##### 1.1 安装

​		windows版安装包下载地址：https://github.com/tporadowski/redis/releases

​		下载的安装包有两种形式，一种是一键安装的msi文件，还有一种是解压缩就能使用的zip文件，哪种形式都行，这里就不介绍安装过程了，本课程采用的是msi一键安装的msi文件进行安装的。

​		msi，其实就是一个文件安装包，不仅安装软件，还帮你把安装软件时需要的功能关联在一起，打包操作。比如如安装序列、创建和设置安装路径、设置系统依赖项、默认设定安装选项和控制安装过程的属性。说简单点就是一站式服务。

​		安装完毕后会得到如下文件，其中有两个文件对应两个命令，是启动Redis的核心命令，需要再CMD命令行模式执行。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323105716572.png)

**启动服务器**

```CMD
redis-server.exe redis.windows.conf
```

​		初学者无需调整服务器对外服务端口，默认6379。

**启动客户端**

```CMD
redis-cli.exe
```

​		如果启动redis服务器失败，可以先启动客户端，然后执行shutdown操作后退出，此时redis服务器就可以正常执行了。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323110249290.png)



##### 1.2 基本操作

​		服务器启动后，使用客户端就可以连接服务器，类似于启动完MySQL数据库，然后启动SQL命令行操作数据库。		

​		放置一个字符串数据到redis中，先为数据定义一个名称，比如name,age等，然后使用命令set设置数据到redis服务器中即可

```CMD
set name itheima
set age 12
```

​		从redis中取出已经放入的数据，根据名称取，就可以得到对应数据。如果没有对应数据就会得到(nil)

```CMD
get name
get age
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323110528273.png)		



```cmd
clear #清屏
```



以上使用的数据存储是一个名称对应一个值，如果要维护的数据过多，可以使用别的数据存储结构。例如hash，它是一种一个名称下可以存储多个数据的存储模型，并且每个数据也可以有自己的二级存储名称。向hash结构中存储数据格式如下：

```
hset a a1 aa1		#对外key名称是a，在名称为a的存储模型中，a1这个key中保存了数据aa1
hset a a2 aa2
```

​		获取hash结构中的数据命令如下

```CMD
hget a a1			#得到aa1
hget a a2			#得到aa2
```

​		有关redis的基础操作就普及到这里，需要全面掌握redis技术，请参看相关教程学习。



##### 1.3 整合

​		在进行整合之前先梳理一下整合的思想，springboot整合任何技术其实就是在springboot中使用对应技术的API。如果两个技术没有交集，就不存在整合的概念了。所谓整合其实就是使用springboot技术去管理其他技术，几个问题是躲不掉的。

​		第一，需要先导入对应技术的坐标，而整合之后，这些坐标都有了一些变化

​		第二，任何技术通常都会有一些相关的设置信息，整合之后，这些信息如何写，写在哪是一个问题

​		第三，没有整合之前操作如果是模式A的话，整合之后如果没有给开发者带来一些便捷操作，那整合将毫无意义，所以整合后操作肯定要简化一些，那对应的操作方式自然也有所不同

​		按照上面的三个问题去思考springboot整合所有技术是一种通用思想，在整合的过程中会逐步摸索出整合的套路，而且适用性非常强，经过若干种技术的整合后基本上可以总结出一套固定思维。

​		下面就开始springboot整合redis，操作步骤如下：

**步骤①**：导入springboot整合redis的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

​		上述坐标可以在创建模块的时候通过勾选的形式进行选择，归属NoSQL分类中

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323155827731.png)

**步骤②**：进行基础配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
```

​		操作redis，最基本的信息就是操作哪一台redis服务器，所以服务器地址属于基础配置信息，不可缺少。但是即便你不配置，目前也是可以用的。因为以上两组信息都有默认配置，刚好就是上述配置值。

**步骤③**：使用springboot整合redis的专用客户端接口操作，此处使用的是RedisTemplate

```java
@SpringBootTest
class Springboot16RedisApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    void set() {
        ValueOperations ops = redisTemplate.opsForValue();
        ops.set("age",41);
    }
    @Test
    void get() {
        ValueOperations ops = redisTemplate.opsForValue();
        Object age = ops.get("name");
        System.out.println(age);
    }
    @Test
    void hset() {
        HashOperations ops = redisTemplate.opsForHash();
        ops.put("info","b","bb");
    }
    @Test
    void hget() {
        HashOperations ops = redisTemplate.opsForHash();
        Object val = ops.get("info", "b");
        System.out.println(val);
    }
}
```

​		在操作redis时，需要先确认操作何种数据，根据数据种类得到操作接口。例如使用opsForValue()获取string类型的数据操作接口，使用opsForHash()获取hash类型的数据操作接口，剩下的就是调用对应api操作了。各种类型的数据操作接口如下：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323160033481.png)

**总结**

1. springboot整合redis步骤
   1. 导入springboot整合redis的starter坐标
   2. 进行基础配置
   3. 使用springboot整合redis的专用客户端接口RedisTemplate操作

<hr>

**StringRedisTemplate**

​		由于redis内部不提供java对象的存储格式，因此当操作的数据以对象的形式存在时，会进行转码，转换成字符串格式后进行操作。为了方便开发者使用基于字符串为数据的操作，springboot整合redis时提供了专用的API接口StringRedisTemplate，你可以理解为这是RedisTemplate的一种指定数据泛型的操作API。

```JAVA
@SpringBootTest
public class StringRedisTemplateTest {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
    @Test
    void get(){
        ValueOperations<String, String> ops = stringRedisTemplate.opsForValue();
        String name = ops.get("name");
        System.out.println(name);
    }
}
```

<hr>

**redis客户端选择**

springboot整合redis技术提供了多种客户端兼容模式，默认提供的是lettucs客户端技术，也可以根据需要切换成指定客户端技术，例如jedis客户端技术，切换成jedis客户端技术操作步骤如下：

**步骤①**：导入jedis坐标

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

​		jedis坐标受springboot管理，无需提供版本号

**步骤②**：配置客户端技术类型，设置为jedis

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    client-type: jedis
```

**步骤③**：根据需要设置对应的配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    client-type: jedis
    lettuce:
      pool:
        max-active: 16
    jedis:
      pool:
        max-active: 16
```

**lettcus与jedis区别**

- jedis连接Redis服务器是直连模式，当多线程模式下使用jedis会存在线程安全问题，解决方案可以通过配置连接池使每个连接专用，这样整体性能就大受影响。
- lettcus基于Netty框架进行与Redis服务器连接，底层设计中采用StatefulRedisConnection。 StatefulRedisConnection自身是线程安全的，可以保障并发访问安全问题，所以一个连接可以被多线程复用。当然lettcus也支持多连接实例一起工作。

**总结**

1. springboot整合redis提供了StringRedisTemplate对象，以字符串的数据格式操作redis
2. 如果需要切换redis客户端实现技术，可以通过配置的形式进行



#### 2. SpringBoot整合MongoDB

​		使用Redis技术可以有效的提高数据访问速度，但是由于Redis的数据格式单一性，无法操作结构化数据，当操作对象型的数据时，Redis就显得捉襟见肘。在保障访问速度的情况下，如果想操作结构化数据，看来Redis无法满足要求了，此时需要使用全新的数据存储结束来解决此问题，本节讲解springboot如何整合MongoDB技术。

> MongoDB是一个开源、高性能、无模式的文档型数据库，它是NoSQL数据库产品中的一种，是最像关系型数据库的非关系型数据库。
>
> - 无模式简单说就是作为一款数据库，没有固定的数据存储结构，第一条数据可能有A、B、C一共3个字段，第二条数据可能有D、E、F也是3个字段，第三条数据可能是A、C、E3个字段，也就是说数据的结构不固定，这就是无模式。灵活，随时变更，不受约束。

基于上述特点，MongoDB的应用面也会产生一些变化。以下列出了一些可以使用MongoDB作为数据存储的场景，但是并不是必须使用MongoDB的场景：

- 淘宝用户数据
  - 存储位置：数据库
  - 特征：永久性存储，修改频度极低
- 游戏装备数据、游戏道具数据
  - 存储位置：数据库、Mongodb
  - 特征：永久性存储与临时存储相结合、修改频度较高
- 直播数据、打赏数据、粉丝数据
  - 存储位置：数据库、Mongodb
  - 特征：永久性存储与临时存储相结合，修改频度极高
- 物联网数据
  - 存储位置：Mongodb
  - 特征：临时存储，修改频度飞速



##### 2.1 安装

​		windows版安装包下载地址：https://www.mongodb.com/try/download

​		下载的安装包也有两种形式，一种是一键安装的msi文件，还有一种是解压缩就能使用的zip文件，哪种形式都行，本课程采用解压缩zip文件进行安装。

​		解压缩完毕后会得到如下文件，其中bin目录包含了所有mongodb的可执行命令

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323162248004.png)

​		mongodb在运行时需要指定一个数据存储的目录，所以创建一个数据存储目录，通常放置在安装目录中，此处创建data的目录用来存储数据，具体如下

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323162349654.png)

​		如果在安装的过程中出现了如下警告信息，就是告诉你，你当前的操作系统缺少了一些系统文件，这个不用担心。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323163021739.png)

​		根据下列方案即可解决，在浏览器中搜索提示缺少的名称对应的文件，并下载，将下载的文件拷贝到windows安装目录的system32目录下，然后在命令行中执行regsvr32命令注册此文件。根据下载的文件名不同，执行命令前更改对应名称。

```CMD
regsvr32 vcruntime140_1.dll
```

**启动服务器**

```CMD
mongod --dbpath=..\data\db
```

​		启动服务器时需要指定数据存储位置，通过参数--dbpath进行设置，可以根据需要自行设置数据存储路径。默认服务端口27017。

**启动客户端**

```CMD
mongo --host=127.0.0.1 --port=27017
```



##### 2.2 基本操作

​		MongoDB虽然是一款数据库，但是它的操作并不是使用SQL语句进行的。好在有一些类似于Navicat的数据库客户端软件，能够便捷的操作MongoDB，先安装一个客户端，再来操作MongoDB。

​		同类型的软件较多，本次安装的软件时Robo3t，Robot3t是一款绿色软件，无需安装，解压缩即可。解压缩完毕后进入安装目录双击robot3t.exe即可使用。

​		打开软件首先要连接MongoDB服务器，选择【File】菜单，选择【Connect...】

​		进入连接管理界面后，选择左上角的【Create】链接，创建新的连接设置，输入设置值即可连接（默认不修改即可连接本机27017端口）。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323163753073.png)

​		连接成功后在命令输入区域输入命令即可操作MongoDB。

​		创建数据库：在左侧菜单中使用右键创建，输入数据库名称即可

​		创建集合：在Collections上使用右键创建，输入集合名称即可，集合等同于数据库中的表的作用

​		新增文档：（文档是一种类似json格式的数据，初学者可以先把数据理解为就是json数据）	

```CMD
db.集合名称.insert/save/insertOne(文档)
```

​		删除文档：

```CMD
db.集合名称.remove(条件)
```

​		修改文档：

```cmd
db.集合名称.update(条件，{操作种类:{文档}})
```

​		查询文档：

```CMD
基础查询
查询全部：		   db.集合.find();
查第一条：		   db.集合.findOne()
查询指定数量文档：	db.集合.find().limit(10)					//查10条文档
跳过指定数量文档：	db.集合.find().skip(20)					//跳过20条文档
统计：			  	db.集合.count()
排序：				db.集合.sort({age:1})						//按age升序排序
投影：				db.集合名称.find(条件,{name:1,age:1})		 //仅保留name与age域

条件查询
基本格式：			db.集合.find({条件})
模糊查询：			db.集合.find({域名:/正则表达式/})		  //等同SQL中的like，比like强大，可以执行正则所有规则
条件比较运算：		   db.集合.find({域名:{$gt:值}})				//等同SQL中的数值比较操作，例如：name>18
包含查询：			db.集合.find({域名:{$in:[值1，值2]}})		//等同于SQL中的in
条件连接查询：		   db.集合.find({$and:[{条件1},{条件2}]})	   //等同于SQL中的and、or
```

​		有关MongoDB的基础操作就普及到这里，需要全面掌握MongoDB技术，请参看相关教程学习。



##### 2.3 整合

​		使用springboot整合MongDB该如何进行呢？其实springboot为什么使用的开发者这么多，就是因为他的套路几乎完全一样。导入坐标，做配置，使用API接口操作。整合Redis如此，整合MongoDB同样如此。

​		第一，先导入对应技术的整合starter坐标

​		第二，配置必要信息

​		第三，使用提供的API操作即可

​		下面就开始springboot整合MongoDB，操作步骤如下：

**步骤①**：导入springboot整合MongoDB的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

​		上述坐标也可以在创建模块的时候通过勾选的形式进行选择，同样归属NoSQL分类中。

**步骤②**：进行基础配置

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://localhost/lak
```

​		操作MongoDB需要的配置与操作redis一样，最基本的信息都是操作哪一台服务器，区别就是连接的服务器IP地址和端口不同，书写格式不同而已。

**步骤③**：使用springboot整合MongoDB的专用客户端接口MongoTemplate来进行操作

```java
@SpringBootTest
class Springboot17MongodbApplicationTests {
    @Autowired
    private MongoTemplate mongoTemplate;
    @Test
    void contextLoads() {
        Book book = new Book();
        book.setId(2);
        book.setName("springboot2");
        book.setType("springboot2");
        book.setDescription("springboot2");
        mongoTemplate.save(book);
    }
    @Test
    void find(){
        List<Book> all = mongoTemplate.findAll(Book.class);
        System.out.println(all);
    }
}
```



**总结**

1. springboot整合MongoDB步骤
   1. 导入springboot整合MongoDB的starter坐标
   2. 进行基础配置
   3. 使用springboot整合MongoDB的专用客户端接口MongoTemplate操作



#### 3. SpringBoot整合ES

​		Redis可以使用内存加载数据并实现数据快速访问，MongoDB可以在内存中存储类似对象的数据并实现数据的快速访问，在企业级开发中对于速度的追求是永无止境的。下面要讲的内容也是一款NoSQL解决方案，只不过他的作用不是为了直接加速数据的读写，而是加速数据的查询的，叫做ES技术。

> ES（Elasticsearch）是一个分布式全文搜索引擎，重点是全文搜索。
>
> 什么是全文搜索呢？比如用户要买一本书，以Java为关键字进行搜索，不管是书名中还是书的介绍中，甚至是书的作者名字，只要包含java就作为查询结果返回给用户查看，上述过程就使用了全文搜索技术。搜索的条件不再是仅用于对某一个字段进行比对，而是在一条数据中使用搜索条件去比对更多的字段，只要能匹配上就列入查询结果，这就是全文搜索的目的。而ES技术就是一种可以实现上述效果的技术。

​		要实现全文搜索的效果，不可能使用数据库中like操作去进行比对，这种效率太低了。ES设计了一种全新的思想，来实现全文搜索。具体操作过程如下：

1. 将被查询的字段的数据全部文本信息进行查分，分成若干个词

   - 例如“中华人民共和国”就会被拆分成三个词，分别是“中华”、“人民”、“共和国”，此过程有专业术语叫做分词。分词的策略不同，分出的效果不一样，不同的分词策略称为分词器。

2. 将分词得到的结果存储起来，对应每条数据的id

   - 例如id为1的数据中名称这一项的值是“中华人民共和国”，那么分词结束后，就会出现“中华”对应id为1，“人民”对应id为1，“共和国”对应id为1

   - 例如id为2的数据中名称这一项的值是“人民代表大会“，那么分词结束后，就会出现“人民”对应id为2，“代表”对应id为2，“大会”对应id为2

   - 此时就会出现如下对应结果，按照上述形式可以对所有文档进行分词。需要注意分词的过程不是仅对一个字段进行，而是对每一个参与查询的字段都执行，最终结果汇总到一个表格中

     | 分词结果关键字——>倒排索引 | 对应id |
     | ------------------------- | ------ |
     | 中华                      | 1      |
     | 人民                      | 1,2    |
     | 共和国                    | 1      |
     | 代表                      | 2      |
     | 大会                      | 2      |

3. 当进行查询时，如果输入“人民”作为查询条件，可以通过上述表格数据进行比对，得到id值1,2，然后根据id值就可以得到查询的结果数据了。

​		上述过程中分词结果关键字内容每一个都不相同，作用有点类似于数据库中的索引，是用来加速数据查询的。但是数据库中的索引是对某一个字段进行添加索引，而这里的分词结果关键字不是一个完整的字段值，只是一个字段中的其中的一部分内容。并且索引使用时是根据索引内容查找整条数据，全文搜索中的分词结果关键字查询后得到的并不是整条的数据，而是数据的id，要想获得具体数据还要再次查询，因此这里为这种分词结果关键字起了一个全新的名称，叫做**倒排索引**。

​		通过上述内容的学习，发现使用ES其实准备工作还是挺多的，必须先建立文档的倒排索引，然后才能继续使用。



##### 3.1 安装

​		windows版安装包下载地址：[https://](https://www.elastic.co/cn/downloads/elasticsearch)[www.elastic.co/cn/downloads/elasticsearch](https://www.elastic.co/cn/downloads/elasticsearch)

​		下载的安装包是解压缩就能使用的zip文件，解压缩完毕后会得到如下文件

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323175603236.png)

- bin目录：包含所有的可执行命令
- config目录：包含ES服务器使用的配置文件
- jdk目录：此目录中包含了一个完整的jdk工具包，版本17，当ES升级时，使用最新版本的jdk确保不会出现版本支持性不足的问题
- lib目录：包含ES运行的依赖jar文件
- logs目录：包含ES运行后产生的所有日志文件
- modules目录：包含ES软件中所有的功能模块，也是一个一个的jar包。和jar目录不同，jar目录是ES运行期间依赖的jar包，modules是ES软件自己的功能jar包
- plugins目录：包含ES软件安装的插件，默认为空

**启动服务器**

```CMD
elasticsearch.bat
```

​		双击elasticsearch.bat文件即可启动ES服务器，默认服务端口9200。通过浏览器访问http://localhost:9200看到如下信息视为ES服务器正常启动

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323181126993.png)



##### 3.2 基本操作  TODO

​		ES中保存有我们要查询的数据，只不过格式和数据库存储数据格式不同而已。在ES中我们要先创建倒排索引，这个索引的功能又点类似于数据库的表，然后将数据添加到倒排索引中，添加的数据称为文档。所以要进行ES的操作要先创建索引，再添加文档，这样才能进行后续的查询操作。

​		要操作ES可以通过Rest风格的请求来进行，也就是说发送一个请求就可以执行一个操作。比如新建索引，删除索引这些操作都可以使用发送请求的形式来进行。

- 创建索引，books是索引名称，下同

  ```CMD
  PUT请求		http://localhost:9200/books
  ```

  发送请求后，看到如下信息即索引创建成功

  ```json
  {
      "acknowledged": true,
      "shards_acknowledged": true,
      "index": "books"
  }
  ```

  重复创建已经存在的索引会出现错误信息，reason属性中描述错误原因

  ```json
  {
      "error": {
          "root_cause": [
              {
                  "type": "resource_already_exists_exception",
                  "reason": "index [books/VgC_XMVAQmedaiBNSgO2-w] already exists",
                  "index_uuid": "VgC_XMVAQmedaiBNSgO2-w",
                  "index": "books"
              }
          ],
          "type": "resource_already_exists_exception",
          "reason": "index [books/VgC_XMVAQmedaiBNSgO2-w] already exists",	# books索引已经存在
          "index_uuid": "VgC_XMVAQmedaiBNSgO2-w",
          "index": "book"
      },
      "status": 400
  }
  ```

- 查询索引

  ```CMD
  GET请求		http://localhost:9200/books
  ```

  查询索引得到索引相关信息，如下

  ```json
  {
      "book": {
          "aliases": {},
          "mappings": {},
          "settings": {
              "index": {
                  "routing": {
                      "allocation": {
                          "include": {
                              "_tier_preference": "data_content"
                          }
                      }
                  },
                  "number_of_shards": "1",
                  "provided_name": "books",
                  "creation_date": "1645768584849",
                  "number_of_replicas": "1",
                  "uuid": "VgC_XMVAQmedaiBNSgO2-w",
                  "version": {
                      "created": "7160299"
                  }
              }
          }
      }
  }
  ```

  如果查询了不存在的索引，会返回错误信息，例如查询名称为book的索引后信息如下

  ```json
  {
      "error": {
          "root_cause": [
              {
                  "type": "index_not_found_exception",
                  "reason": "no such index [book]",
                  "resource.type": "index_or_alias",
                  "resource.id": "book",
                  "index_uuid": "_na_",
                  "index": "book"
              }
          ],
          "type": "index_not_found_exception",
          "reason": "no such index [book]",		# 没有book索引
          "resource.type": "index_or_alias",
          "resource.id": "book",
          "index_uuid": "_na_",
          "index": "book"
      },
      "status": 404
  }
  ```

- 删除索引

  ```CMD
  DELETE请求	http://localhost:9200/books
  ```

  删除所有后，给出删除结果

  ```json
  {
      "acknowledged": true
  }
  ```

  如果重复删除，会给出错误信息，同样在reason属性中描述具体的错误原因

  ```JSON
  {
      "error": {
          "root_cause": [
              {
                  "type": "index_not_found_exception",
                  "reason": "no such index [books]",
                  "resource.type": "index_or_alias",
                  "resource.id": "book",
                  "index_uuid": "_na_",
                  "index": "book"
              }
          ],
          "type": "index_not_found_exception",
          "reason": "no such index [books]",		# 没有books索引
          "resource.type": "index_or_alias",
          "resource.id": "book",
          "index_uuid": "_na_",
          "index": "book"
      },
      "status": 404
  }
  ```

- 创建索引并指定分词器

  ​		前面创建的索引是未指定分词器的，可以在创建索引时添加请求参数，设置分词器。目前国内较为流行的分词器是IK分词器，使用前先在下对应的分词器，然后使用。IK分词器下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases

  ​		分词器下载后解压到ES安装目录的plugins目录中即可，安装分词器后需要重新启动ES服务器。使用IK分词器创建索引格式：

  ```json
  PUT请求		http://localhost:9200/books
  
  请求参数如下（注意是json格式的参数）
  {
      "mappings":{							#定义mappings属性，替换创建索引时对应的mappings属性		
          "properties":{						#定义索引中包含的属性设置
              "id":{							#设置索引中包含id属性
                  "type":"keyword"			#当前属性可以被直接搜索
              },
              "name":{						#设置索引中包含name属性
                  "type":"text",              #当前属性是文本信息，参与分词  
                  "analyzer":"ik_max_word",   #使用IK分词器进行分词             
                  "copy_to":"all"				#分词结果拷贝到all属性中
              },
              "type":{
                  "type":"keyword"
              },
              "description":{
                  "type":"text",	                
                  "analyzer":"ik_max_word",                
                  "copy_to":"all"
              },
              "all":{							#定义属性，用来描述多个字段的分词结果集合，当前属性可以参与查询
                  "type":"text",	                
                  "analyzer":"ik_max_word"
              }
          }
      }
  }
  ```

  ​		创建完毕后返回结果和不使用分词器创建索引的结果是一样的，此时可以通过查看索引信息观察到添加的请求参数mappings已经进入到了索引属性中

  ```json
  {
      "books": {
          "aliases": {},
          "mappings": {						#mappings属性已经被替换
              "properties": {
                  "all": {
                      "type": "text",
                      "analyzer": "ik_max_word"
                  },
                  "description": {
                      "type": "text",
                      "copy_to": [
                          "all"
                      ],
                      "analyzer": "ik_max_word"
                  },
                  "id": {
                      "type": "keyword"
                  },
                  "name": {
                      "type": "text",
                      "copy_to": [
                          "all"
                      ],
                      "analyzer": "ik_max_word"
                  },
                  "type": {
                      "type": "keyword"
                  }
              }
          },
          "settings": {
              "index": {
                  "routing": {
                      "allocation": {
                          "include": {
                              "_tier_preference": "data_content"
                          }
                      }
                  },
                  "number_of_shards": "1",
                  "provided_name": "books",
                  "creation_date": "1645769809521",
                  "number_of_replicas": "1",
                  "uuid": "DohYKvr_SZO4KRGmbZYmTQ",
                  "version": {
                      "created": "7160299"
                  }
              }
          }
      }
  }
  ```

目前我们已经有了索引了，但是索引中还没有数据，所以要先添加数据，ES中称数据为文档，下面进行文档操作。

- 添加文档，有三种方式

  ```json
  POST请求	http://localhost:9200/books/_doc		#使用系统生成id
  POST请求	http://localhost:9200/books/_create/1	#使用指定id
  POST请求	http://localhost:9200/books/_doc/1		#使用指定id，不存在创建，存在更新（版本递增）
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }  
  ```

- 查询文档

  ```json
  GET请求	http://localhost:9200/books/_doc/1		 #查询单个文档 		
  GET请求	http://localhost:9200/books/_search		 #查询全部文档
  ```

- 条件查询

  ```json
  GET请求	http://localhost:9200/books/_search?q=name:springboot	# q=查询属性名:查询属性值
  ```

- 删除文档

  ```json
  DELETE请求	http://localhost:9200/books/_doc/1
  ```

- 修改文档（全量更新）

  ```json
  PUT请求	http://localhost:9200/books/_doc/1
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }
  ```

- 修改文档（部分更新）

  ```json
  POST请求	http://localhost:9200/books/_update/1
  
  文档通过请求参数传递，数据格式json
  {			
      "doc":{						#部分更新并不是对原始文档进行更新，而是对原始文档对象中的doc属性中的指定属性更新
          "name":"springboot"		#仅更新提供的属性值，未提供的属性值不参与更新操作
      }
  }
  ```

##### 3.3 整合

​		使用springboot整合ES该如何进行呢？老规矩，导入坐标，做配置，使用API接口操作。整合Redis如此，整合MongoDB如此，整合ES依然如此。太没有新意了，其实不是没有新意，这就是springboot的强大之处，所有东西都做成相同规则，对开发者来说非常友好。

​		下面就开始springboot整合ES，操作步骤如下：

**步骤①**：导入springboot整合ES的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

**步骤②**：进行基础配置

```yaml
spring:
  elasticsearch:
    rest:
      uris: http://localhost:9200
```

​		配置ES服务器地址，端口9200

**步骤③**：使用springboot整合ES的专用客户端接口ElasticsearchRestTemplate来进行操作

```java
@SpringBootTest
class Springboot18EsApplicationTests {
    @Autowired
    private ElasticsearchRestTemplate template;
}
```

​		上述操作形式是ES早期的操作方式，使用的客户端被称为Low Level Client，这种客户端操作方式性能方面略显不足，于是ES开发了全新的客户端操作方式，称为High Level Client。高级别客户端与ES版本同步更新，但是springboot最初整合ES的时候使用的是低级别客户端，所以企业开发需要更换成高级别的客户端模式。

​		下面使用高级别客户端方式进行springboot整合ES，操作步骤如下：

**步骤①**：导入springboot整合ES高级别客户端的坐标，此种形式目前没有对应的starter

```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
</dependency>
```

**步骤②**：使用编程的形式设置连接的ES服务器，并获取客户端对象

```java
@SpringBootTest
class Springboot18EsApplicationTests {
    private RestHighLevelClient client;
      @Test
      void testCreateClient() throws IOException {
          HttpHost host = HttpHost.create("http://localhost:9200");
          RestClientBuilder builder = RestClient.builder(host);
          client = new RestHighLevelClient(builder);
  
          client.close();
      }
}
```

​		配置ES服务器地址与端口9200，记得客户端使用完毕需要手工关闭。由于当前客户端是手工维护的，因此不能通过自动装配的形式加载对象。

**步骤③**：使用客户端对象操作ES，例如创建索引

```java
@SpringBootTest
class Springboot18EsApplicationTests {
    private RestHighLevelClient client;
      @Test
      void testCreateIndex() throws IOException {
          HttpHost host = HttpHost.create("http://localhost:9200");
          RestClientBuilder builder = RestClient.builder(host);
          client = new RestHighLevelClient(builder);
          
          CreateIndexRequest request = new CreateIndexRequest("books");
          client.indices().create(request, RequestOptions.DEFAULT); 
          
          client.close();
      }
}
```

​		高级别客户端操作是通过发送请求的方式完成所有操作的，ES针对各种不同的操作，设定了各式各样的请求对象，上例中创建索引的对象是CreateIndexRequest，其他操作也会有自己专用的Request对象。

​		当前操作我们发现，无论进行ES何种操作，第一步永远是获取RestHighLevelClient对象，最后一步永远是关闭该对象的连接。在测试中可以使用测试类的特性去帮助开发者一次性的完成上述操作，但是在业务书写时，还需要自行管理。将上述代码格式转换成使用测试类的初始化方法和销毁方法进行客户端对象的维护。

```JAVA
@SpringBootTest
class Springboot18EsApplicationTests {
    @BeforeEach		//在测试类中每个操作运行前运行的方法
    void setUp() {
        HttpHost host = HttpHost.create("http://localhost:9200");
        RestClientBuilder builder = RestClient.builder(host);
        client = new RestHighLevelClient(builder);
    }

    @AfterEach		//在测试类中每个操作运行后运行的方法
    void tearDown() throws IOException {
        client.close();
    }

    private RestHighLevelClient client;

    @Test
    void testCreateIndex() throws IOException {
        CreateIndexRequest request = new CreateIndexRequest("books");
        client.indices().create(request, RequestOptions.DEFAULT);
    }
}
```

​		现在的书写简化了很多，也更合理。下面使用上述模式将所有的ES操作执行一遍，测试结果

**创建索引（IK分词器）**：

```java
@Test
void testCreateIndexByIK() throws IOException {
    CreateIndexRequest request = new CreateIndexRequest("books");
    String json = "{\n" +
            "    \"mappings\":{\n" +
            "        \"properties\":{\n" +
            "            \"id\":{\n" +
            "                \"type\":\"keyword\"\n" +
            "            },\n" +
            "            \"name\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\",\n" +
            "                \"copy_to\":\"all\"\n" +
            "            },\n" +
            "            \"type\":{\n" +
            "                \"type\":\"keyword\"\n" +
            "            },\n" +
            "            \"description\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\",\n" +
            "                \"copy_to\":\"all\"\n" +
            "            },\n" +
            "            \"all\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\"\n" +
            "            }\n" +
            "        }\n" +
            "    }\n" +
            "}";
    //设置请求中的参数
    request.source(json, XContentType.JSON);
    client.indices().create(request, RequestOptions.DEFAULT);
}
```

​		IK分词器是通过请求参数的形式进行设置的，设置请求参数使用request对象中的source方法进行设置，至于参数是什么，取决于你的操作种类。当请求中需要参数时，均可使用当前形式进行参数设置。	

**添加文档**：

```java
@Test
//添加文档
void testCreateDoc() throws IOException {
    Book book = bookDao.selectById(1);
    IndexRequest request = new IndexRequest("books").id(book.getId().toString());
    String json = JSON.toJSONString(book);
    request.source(json,XContentType.JSON);
    client.index(request,RequestOptions.DEFAULT);
}
```

​		添加文档使用的请求对象是IndexRequest，与创建索引使用的请求对象不同。	

**批量添加文档**：

```java
@Test
//批量添加文档
void testCreateDocAll() throws IOException {
    List<Book> bookList = bookDao.selectList(null);
    BulkRequest bulk = new BulkRequest();
    for (Book book : bookList) {
        IndexRequest request = new IndexRequest("books").id(book.getId().toString());
        String json = JSON.toJSONString(book);
        request.source(json,XContentType.JSON);
        bulk.add(request);
    }
    client.bulk(bulk,RequestOptions.DEFAULT);
}
```

​		批量做时，先创建一个BulkRequest的对象，可以将该对象理解为是一个保存request对象的容器，将所有的请求都初始化好后，添加到BulkRequest对象中，再使用BulkRequest对象的bulk方法，一次性执行完毕。

**按id查询文档**：

```java
@Test
//按id查询
void testGet() throws IOException {
    GetRequest request = new GetRequest("books","1");
    GetResponse response = client.get(request, RequestOptions.DEFAULT);
    String json = response.getSourceAsString();
    System.out.println(json);
}
```

​		根据id查询文档使用的请求对象是GetRequest。

**按条件查询文档**：

```java
@Test
//按条件查询
void testSearch() throws IOException {
    SearchRequest request = new SearchRequest("books");

    SearchSourceBuilder builder = new SearchSourceBuilder();
    builder.query(QueryBuilders.termQuery("all","spring"));
    request.source(builder);

    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    for (SearchHit hit : hits) {
        String source = hit.getSourceAsString();
        //System.out.println(source);
        Book book = JSON.parseObject(source, Book.class);
        System.out.println(book);
    }
}
```

​		按条件查询文档使用的请求对象是SearchRequest，查询时调用SearchRequest对象的termQuery方法，需要给出查询属性名，此处支持使用合并字段，也就是前面定义索引属性时添加的all属性。

​		springboot整合ES的操作到这里就说完了，与前期进行springboot整合redis和mongodb的差别还是蛮大的，主要原始就是我们没有使用springboot整合ES的客户端对象。至于操作，由于ES操作种类过多，所以显得操作略微有点复杂。有关springboot整合ES就先学习到这里吧。

**总结**

1. springboot整合ES步骤
   1. 导入springboot整合ES的High Level Client坐标
   2. 手工管理客户端对象，包括初始化和关闭操作
   3. 使用High Level Client根据操作的种类不同，选择不同的Request对象完成对应操作





## 第5章 整合第三方技术

通过第四章的学习，我们领略到了springboot在整合第三方技术时强大的一致性，在第五章中我们要使用springboot继续整合各种各样的第三方技术，通过本章的学习，可以将之前学习的springboot整合第三方技术的思想贯彻到底，还是那三板斧。导坐标、做配置、调API。

​		springboot能够整合的技术实在是太多了，可以说是万物皆可整。本章将从企业级开发中常用的一些技术作为出发点，对各种各样的技术进行整合。



### 一、缓存

​		企业级应用主要作用是信息处理，当需要读取数据时，由于受限于数据库的访问效率，导致整体系统性能偏低。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323194644509.png)

​													应用程序直接与数据库打交道，访问效率低

​		为了改善上述现象，开发者通常会在应用程序与数据库之间建立一种临时的数据存储机制，该区域中的数据在内存中保存，读写速度较快，可以有效解决数据库访问效率低下的问题。这一块临时存储数据的区域就是缓存。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323194706938.png)

​							使用缓存后，应用程序与缓存打交道，缓存与数据库打交道，数据访问效率提高

​		缓存是什么？

- 缓存是一种介于数据永久存储介质与应用程序之间的数据临时存储介质。

- 使用缓存可以有效的减少低速数据读取过程的次数（例如磁盘IO），提高系统性能。

- 缓存不仅可以用于提高永久性存储介质的数据读取效率，还可以提供临时的数据存储空间。

  

  而springboot提供了对市面上几乎所有的缓存技术进行整合的方案。



#### 1. SpringBoot内置缓存解决方案

​		springboot技术提供有内置的缓存解决方案，可以帮助开发者快速开启缓存技术，并使用缓存技术进行数据的快速操作，例如读取缓存数据和写入数据到缓存。

​		缓存使用：启用缓存；设置进入缓存的数据；设置读取缓存的数据。

**步骤①**：导入springboot提供的缓存技术对应的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**步骤②**：启用缓存，在引导类上方标注注解@EnableCaching配置springboot程序中可以使用缓存

```java
@SpringBootApplication
//开启缓存功能
@EnableCaching
public class Springboot19CacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot19CacheApplication.class, args);
    }
}
```

**步骤③**：设置操作的数据是否使用缓存

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;

    @Cacheable(value="cacheSpace",key="#id")
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }
}
```

​		在业务方法上面使用注解@Cacheable声明当前方法的返回值放入缓存中，其中要指定缓存的存储位置，以及缓存中保存当前方法返回值对应的名称。上例中value属性描述缓存的存储位置，可以理解为是一个存储空间名，key属性描述了缓存中保存数据的名称，使用#id读取形参中的id值作为缓存名称。

​		使用@Cacheable注解后，执行当前操作，如果发现对应名称在缓存中没有数据，就正常读取数据，然后放入缓存；如果对应名称在缓存中有数据，就终止当前业务方法执行，直接返回缓存中的数据。



#### 2. 手机验证码案例

​		为了便于下面演示各种各样的缓存技术，我们创建一个手机验证码的案例环境，模拟使用缓存保存手机验证码的过程。

​		**需求：**

- 输入手机号获取验证码，组织文档以短信形式发送给用户（页面模拟）
- 输入手机号和验证码验证结果

​		**需求分析：**

- 提供controller，传入手机号，业务层通过手机号计算出独有的6位验证码数据，存入缓存后返回此数据。
- 提供controller，传入手机号与验证码，业务层提供手机号从缓存中读取验证码与输入验证码进行比对，返回比对结果。

​		为了描述上述操作，我们制作两个表现层接口，一个用来模拟发送短信的过程，其实就是根据用户提供的手机号生成一个验证码，然后放入缓存，另一个用来模拟验证码校验的过程，其实就是使用传入的手机号和验证码进行匹配，并返回最终匹配结果。下面直接制作本案例的模拟代码，先以上例中springboot提供的内置缓存技术来完成当前案例的制作。

**步骤①**：导入springboot提供的缓存技术对应的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**步骤②**：启用缓存，在引导类上方标注注解@EnableCaching配置springboot程序中可以使用缓存

```java
@SpringBootApplication
//开启缓存功能
@EnableCaching
public class Springboot19CacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot19CacheApplication.class, args);
    }
}
```

**步骤③**：定义验证码对应的实体类，封装手机号与验证码两个属性

```java
@Data
public class SMSCode {
    private String tele;
    private String code;
}
```

**步骤④**：定义验证码功能的业务层接口与实现类

```java
public interface SMSCodeService {
    public String sendCodeToSMS(String tele);
    public boolean checkCode(SMSCode smsCode);
}

@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;

    @CachePut(value = "smsCode", key = "#tele")
    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        //取出内存中的验证码与传递过来的验证码比对，如果相同，返回true
        String code = smsCode.getCode();
        String cacheCode = codeUtils.get(smsCode.getTele());
        return code.equals(cacheCode); //equalszuo
    }
}
```

​		获取验证码后，当验证码失效时必须重新获取验证码，因此在获取验证码的功能上不能使用@Cacheable注解，@Cacheable注解是缓存中没有值则放入值，缓存中有值则取值。此处的功能仅仅是生成验证码并放入缓存，并不具有从缓存中取值的功能，因此不能使用@Cacheable注解，应该使用仅具有向缓存中保存数据的功能，使用@CachePut注解即可。

​		对于校验验证码的功能建议放入工具类中进行。

**步骤⑤**：定义验证码的生成策略与根据手机号读取验证码的功能

```java
@Component
public class CodeUtils {
    private String [] patch = {"000000","00000","0000","000","00","0",""};

    public String generator(String tele){
        int hash = tele.hashCode();
        int encryption = 20206666;
        long result = hash ^ encryption;
        long nowTime = System.currentTimeMillis();
        result = result ^ nowTime;
        long code = result % 1000000;
        code = code < 0 ? -code : code;
        String codeStr = code + "";
        int len = codeStr.length();
        return patch[len] + codeStr;
    }

    @Cacheable(value = "smsCode",key="#tele")
    public String get(String tele){
        return null;
    }
}
```

**步骤⑥**：定义验证码功能的web层接口，一个方法用于提供手机号获取验证码，一个方法用于提供手机号和验证码进行校验

```java
@RestController
@RequestMapping("/sms")
public class SMSCodeController {
    @Autowired
    private SMSCodeService smsCodeService;
    
    @GetMapping
    public String getCode(String tele){
        String code = smsCodeService.sendCodeToSMS(tele);
        return code;
    }
    
    @PostMapping
    public boolean checkCode(SMSCode smsCode){
        return smsCodeService.checkCode(smsCode);
    }
}
```



#### 3. SpringBoot整合Ehcache缓存

​		手机验证码的案例已经完成了，下面就开始springboot整合各种各样的缓存技术，第一个整合Ehcache技术。Ehcache是一种缓存技术，使用springboot整合Ehcache其实就是变更一下缓存技术的实现方式，话不多说，直接开整

**步骤①**：导入Ehcache的坐标

```xml
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

​		此处为什么不是导入Ehcache的starter，而是导入技术坐标呢？其实springboot整合缓存技术做的是通用格式，不管你整合哪种缓存技术，只是实现变化了，操作方式一样。这也体现出springboot技术的优点，统一同类技术的整合方式。

**步骤②**：配置缓存技术实现使用Ehcache

```yaml
spring:
  cache:
    type: ehcache
    ehcache:
      config: ehcache.xml
```

​		配置缓存的类型type为ehcache，此处需要说明一下，当前springboot可以整合的缓存技术中包含有ehcach，所以可以这样书写。其实这个type不可以随便写的，不是随便写一个名称就可以整合的。

​		由于ehcache的配置有独立的配置文件格式，因此还需要指定ehcache的配置文件，以便于读取相应配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <diskStore path="D:\ehcache" />

    <!--默认缓存策略 -->
    <!-- external：是否永久存在，设置为true则不会被清除，此时与timeout冲突，通常设置为false-->
    <!-- diskPersistent：是否启用磁盘持久化-->
    <!-- maxElementsInMemory：最大缓存数量-->
    <!-- overflowToDisk：超过最大缓存数量是否持久化到磁盘-->
    <!-- timeToIdleSeconds：最大不活动间隔，设置过长缓存容易溢出，设置过短无效果，可用于记录时效性数据，例如验证码-->
    <!-- timeToLiveSeconds：最大存活时间-->
    <!-- memoryStoreEvictionPolicy：缓存清除策略-->
    <defaultCache
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="60"
        timeToLiveSeconds="60"
        memoryStoreEvictionPolicy="LRU" />

    <cache
        name="smsCode"
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="10"
        timeToLiveSeconds="10"
        memoryStoreEvictionPolicy="LRU" />
</ehcache>
```

​		注意前面的案例中，设置了数据保存的位置是smsCode

```java
@CachePut(value = "smsCode", key = "#tele")
public String sendCodeToSMS(String tele) {
    String code = codeUtils.generator(tele);
    return code;
}	
```

​		这个设定需要保障ehcache中有一个缓存空间名称叫做smsCode的配置，前后要统一。在企业开发过程中，通过设置不同名称的cache来设定不同的缓存策略，应用于不同的缓存数据。

​		到这里springboot整合Ehcache就做完了，可以发现一点，原始代码没有任何修改，仅仅是加了一组配置就可以变更缓存供应商了，这也是springboot提供了统一的缓存操作接口的优势，变更实现并不影响原始代码的书写。

**总结**

1. springboot使用Ehcache作为缓存实现需要导入Ehcache的坐标
2. 修改设置，配置缓存供应商为ehcache，并提供对应的缓存配置文件

​		

#### 4. SpringBoot整合Redis缓存

​		上节使用Ehcache替换了springboot内置的缓存技术，其实springboot支持的缓存技术还很多，下面使用redis技术作为缓存解决方案来实现手机验证码案例。

​		比对使用Ehcache的过程，加坐标，改缓存实现类型为ehcache，做Ehcache的配置。如果还成redis做缓存呢？一模一样，加坐标，改缓存实现类型为redis，做redis的配置。差别之处只有一点，redis的配置可以在yml文件中直接进行配置，无需制作独立的配置文件。

**步骤①**：导入redis的坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**步骤②**：配置缓存技术实现使用redis

```yaml
spring:
  redis:
    host: localhost
    port: 6379
  cache:
    type: redis
```

​		如果需要对redis作为缓存进行配置，注意不是对原始的redis进行配置，而是配置redis作为缓存使用相关的配置，隶属于spring.cache.redis节点下，注意不要写错位置了。

```yaml
spring:
  redis:
    host: localhost
    port: 6379
  cache:
    type: redis
    redis:
      use-key-prefix: false
      key-prefix: sms_
      cache-null-values: false
      time-to-live: 10s
```

**总结**

1. springboot使用redis作为缓存实现需要导入redis的坐标
2. 修改设置，配置缓存供应商为redis，并提供对应的缓存配置



#### 5. SpringBoot整合Memcached缓存

​		目前我们已经掌握了3种缓存解决方案的配置形式，分别是springboot内置缓存，ehcache和redis，本节研究一下国内比较流行的一款缓存memcached。

​		按照之前的套路，其实变更缓存并不繁琐，但是springboot并没有支持使用memcached作为其缓存解决方案，也就是说在type属性中没有memcached的配置选项，这里就需要更变一下处理方式了。在整合之前先安装memcached。

**安装**

​		windows版安装包下载地址：https://www.runoob.com/memcached/window-install-memcached.html

​		下载的安装包是解压缩就能使用的zip文件，解压缩完毕后会得到如下文件

![image-20220226174957040](E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220226174957040.png)

​		可执行文件只有一个memcached.exe，使用该文件可以将memcached作为系统服务启动，执行此文件时会出现报错信息，如下：

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220226175141986.png" alt="image-20220226175141986" style="zoom:80%;" />

​		此处出现问题的原因是注册系统服务时需要使用管理员权限，当前账号权限不足导致安装服务失败，切换管理员账号权限启动命令行

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220226175302903.png" alt="image-20220226175302903" style="zoom:80%;" />

​		然后再次执行安装服务的命令即可，如下：

```CMD
memcached.exe -d install
```

​		服务安装完毕后可以使用命令启动和停止服务，如下：

```cmd
memcached.exe -d start		# 启动服务
memcached.exe -d stop		# 停止服务
```

​		也可以在任务管理器中进行服务状态的切换

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220226175441675.png" alt="image-20220226175441675" style="zoom:67%;" />

**变更缓存为Memcached**

​		由于memcached未被springboot收录为缓存解决方案，因此使用memcached需要通过手工硬编码的方式来使用，于是前面的套路都不适用了，需要自己写了。

​		memcached目前提供有三种客户端技术，分别是Memcached Client for Java、SpyMemcached和Xmemcached，其中性能指标各方面最好的客户端是Xmemcached，本次整合就使用这个作为客户端实现技术了。下面开始使用Xmemcached

**步骤①**：导入xmemcached的坐标

```xml
<dependency>
    <groupId>com.googlecode.xmemcached</groupId>
    <artifactId>xmemcached</artifactId>
    <version>2.4.7</version>
</dependency>
```

**步骤②**：配置memcached，制作memcached的配置类

```java
@Configuration
public class XMemcachedConfig {
    @Bean
    public MemcachedClient getMemcachedClient() throws IOException {
        MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder("localhost:11211");
        MemcachedClient memcachedClient = memcachedClientBuilder.build();
        return memcachedClient;
    }
}
```

​		memcached默认对外服务端口11211。

**步骤③**：使用xmemcached客户端操作缓存，注入MemcachedClient对象

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;
    @Autowired
    private MemcachedClient memcachedClient;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        try {
            memcachedClient.set(tele,10,code);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = null;
        try {
            code = memcachedClient.get(smsCode.getTele()).toString();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return smsCode.getCode().equals(code);
    }
}
```

​		设置值到缓存中使用set操作，取值使用get操作，其实更符合我们开发者的习惯。

​		上述代码中对于服务器的配置使用硬编码写死到了代码中，将此数据提取出来，做成独立的配置属性。

**定义配置属性**

​		以下过程采用前期学习的属性配置方式进行，当前操作有助于理解原理篇中的很多知识。

- 定义配置类，加载必要的配置属性，读取配置文件中memcached节点信息

  ```java
  @Component
  @ConfigurationProperties(prefix = "memcached")
  @Data
  public class XMemcachedProperties {
      private String servers;
      private int poolSize;
      private long opTimeout;
  }
  ```

- 定义memcached节点信息

  ```yaml
  memcached:
    servers: localhost:11211
    poolSize: 10
    opTimeout: 3000
  ```

- 在memcached配置类中加载信息

```java
@Configuration
public class XMemcachedConfig {
    @Autowired
    private XMemcachedProperties props;
    @Bean
    public MemcachedClient getMemcachedClient() throws IOException {
        MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder(props.getServers());
        memcachedClientBuilder.setConnectionPoolSize(props.getPoolSize());
        memcachedClientBuilder.setOpTimeout(props.getOpTimeout());
        MemcachedClient memcachedClient = memcachedClientBuilder.build();
        return memcachedClient;
    }
}
```

**总结**

1. memcached安装后需要启动对应服务才可以对外提供缓存功能，安装memcached服务需要基于windows系统管理员权限
2. 由于springboot没有提供对memcached的缓存整合方案，需要采用手工编码的形式创建xmemcached客户端操作缓存
3. 导入xmemcached坐标后，创建memcached配置类，注册MemcachedClient对应的bean，用于操作缓存
4. 初始化MemcachedClient对象所需要使用的属性可以通过自定义配置属性类的形式加载

**思考**

​		到这里已经完成了三种缓存的整合，其中redis和mongodb需要安装独立的服务器，连接时需要输入对应的服务器地址，这种是远程缓存，Ehcache是一个典型的内存级缓存，因为它什么也不用安装，启动后导入jar包就有缓存功能了。这个时候就要问了，能不能这两种缓存一起用呢？咱们下节再说。



#### 6. SpringBoot整合jetcache缓存

​		目前我们使用的缓存都是要么A要么B，能不能AB一起用呢？这一节就解决这个问题。springboot针对缓存的整合仅仅停留在用缓存上面，如果缓存自身不支持同时支持AB一起用，springboot也没办法，所以要想解决AB缓存一起用的问题，就必须找一款缓存能够支持AB两种缓存一起用，有这种缓存吗？还真有，阿里出品，jetcache。

​		jetcache严格意义上来说，并不是一个缓存解决方案，只能说他算是一个缓存框架，然后把别的缓存放到jetcache中管理，这样就可以支持AB缓存一起用了。并且jetcache参考了springboot整合缓存的思想，整体技术使用方式和springboot的缓存解决方案思想非常类似。下面咱们就先把jetcache用起来，然后再说它里面的一些小的功能。

​		做之前要先明确一下，jetcache并不是随便拿两个缓存都能拼到一起去的。目前jetcache支持的缓存方案本地缓存支持两种，远程缓存支持两种，分别如下：

- 本地缓存（Local）
  - LinkedHashMap
  - Caffeine
- 远程缓存（Remote）
  - Redis
  - Tair

​		其实也有人问我，为什么jetcache只支持2+2这么4款缓存呢？阿里研发这个技术其实主要是为了满足自身的使用需要。最初肯定只有1+1种，逐步变化成2+2种。下面就以LinkedHashMap+Redis的方案实现本地与远程缓存方案同时使用。

##### 6.1 纯远程方案

**步骤①**：导入springboot整合jetcache对应的坐标starter，当前坐标默认使用的远程方案是redis

```xml
<dependency>
    <groupId>com.alicp.jetcache</groupId>
    <artifactId>jetcache-starter-redis</artifactId>
    <version>2.6.2</version>
</dependency>
```

**步骤②**：远程方案基本配置

```yaml
jetcache:
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

​		其中poolConfig是必配项，否则会报错

**步骤③**：启用缓存，在引导类上方标注注解@EnableCreateCacheAnnotation配置springboot程序中可以使用注解的形式创建缓存

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：创建缓存对象Cache，并使用注解@CreateCache标记当前缓存的信息，然后使用Cache对象的API操作缓存，put写缓存，get读缓存。

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;
    
    @CreateCache(name="jetCache_",expire = 10,timeUnit = TimeUnit.SECONDS)
    private Cache<String ,String> jetCache;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        jetCache.put(tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = jetCache.get(smsCode.getTele());
        return smsCode.getCode().equals(code);
    }
}
```

​		通过上述jetcache使用远程方案连接redis可以看出，jetcache操作缓存时的接口操作更符合开发者习惯，使用缓存就先获取缓存对象Cache，放数据进去就是put，取数据出来就是get，更加简单易懂。并且jetcache操作缓存时，可以为某个缓存对象设置过期时间，将同类型的数据放入缓存中，方便有效周期的管理。

​		上述方案中使用的是配置中定义的default缓存，其实这个default是个名字，可以随便写，也可以随便加。例如再添加一种缓存解决方案，参照如下配置进行：

```yaml
jetcache:
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
    sms:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

​		如果想使用名称是sms的缓存，需要再创建缓存时指定参数area，声明使用对应缓存即可

```JAVA
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;
    
    @CreateCache(area="sms",name="jetCache_",expire = 10,timeUnit = TimeUnit.SECONDS)
    private Cache<String ,String> jetCache;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        jetCache.put(tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = jetCache.get(smsCode.getTele());
        return smsCode.getCode().equals(code);
    }
}
```

##### 6.2 纯本地方案

​		远程方案中，配置中使用remote表示远程，换成local就是本地，只不过类型不一样而已。

**步骤①**：导入springboot整合jetcache对应的坐标starter

```xml
<dependency>
    <groupId>com.alicp.jetcache</groupId>
    <artifactId>jetcache-starter-redis</artifactId>
    <version>2.6.2</version>
</dependency>
```

**步骤②**：本地缓存基本配置

```yaml
jetcache:
  local:
    default:
      type: linkedhashmap
      keyConvertor: fastjson
```

​		为了加速数据获取时key的匹配速度，jetcache要求指定key的类型转换器。简单说就是，如果你给了一个Object作为key的话，我先用key的类型转换器给转换成字符串，然后再保存。等到获取数据时，仍然是先使用给定的Object转换成字符串，然后根据字符串匹配。由于jetcache是阿里的技术，这里推荐key的类型转换器使用阿里的fastjson。

**步骤③**：启用缓存

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：创建缓存对象Cache时，标注当前使用本地缓存

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @CreateCache(name="jetCache_",expire = 1000,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.LOCAL)
    private Cache<String ,String> jetCache;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        jetCache.put(tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = jetCache.get(smsCode.getTele());
        return smsCode.getCode().equals(code);
    }
}
```

​		cacheType控制当前缓存使用本地缓存还是远程缓存，配置cacheType=CacheType.LOCAL即使用本地缓存。

##### 		6.3 本地+远程方案

​		本地和远程方法都有了，两种方案一起使用如何配置呢？其实就是将两种配置合并到一起就可以了。

```YAML
jetcache:
  local:
    default:
      type: linkedhashmap
      keyConvertor: fastjson
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
    sms:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

​		在创建缓存的时候，配置cacheType为BOTH即则本地缓存与远程缓存同时使用。

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @CreateCache(name="jetCache_",expire = 1000,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.BOTH)
    private Cache<String ,String> jetCache;
}
```

​		cacheType如果不进行配置，默认值是REMOTE，即仅使用远程缓存方案。关于jetcache的配置，参考以下信息

| 属性                                                      | 默认值 | 说明                                                         |
| --------------------------------------------------------- | ------ | ------------------------------------------------------------ |
| jetcache.statIntervalMinutes                              | 0      | 统计间隔，0表示不统计                                        |
| jetcache.hiddenPackages                                   | 无     | 自动生成name时，隐藏指定的包名前缀                           |
| jetcache.[local\|remote].${area}.type                     | 无     | 缓存类型，本地支持linkedhashmap、caffeine，远程支持redis、tair |
| jetcache.[local\|remote].${area}.keyConvertor             | 无     | key转换器，当前仅支持fastjson                                |
| jetcache.[local\|remote].${area}.valueEncoder             | java   | 仅remote类型的缓存需要指定，可选java和kryo                   |
| jetcache.[local\|remote].${area}.valueDecoder             | java   | 仅remote类型的缓存需要指定，可选java和kryo                   |
| jetcache.[local\|remote].${area}.limit                    | 100    | 仅local类型的缓存需要指定，缓存实例最大元素数                |
| jetcache.[local\|remote].${area}.expireAfterWriteInMillis | 无穷大 | 默认过期时间，毫秒单位                                       |
| jetcache.local.${area}.expireAfterAccessInMillis          | 0      | 仅local类型的缓存有效，毫秒单位，最大不活动间隔              |

​		以上方案仅支持手工控制缓存，但是springcache方案中的方法缓存特别好用，给一个方法添加一个注解，方法就会自动使用缓存。jetcache也提供了对应的功能，即方法缓存。

**方法缓存**

​		jetcache提供了方法缓存方案，只不过名称变更了而已。在对应的操作接口上方使用注解@Cached即可

**步骤①**：导入springboot整合jetcache对应的坐标starter

```xml
<dependency>
    <groupId>com.alicp.jetcache</groupId>
    <artifactId>jetcache-starter-redis</artifactId>
    <version>2.6.2</version>
</dependency>
```

**步骤②**：配置缓存

```yaml
jetcache:
  local:
    default:
      type: linkedhashmap
      keyConvertor: fastjson
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      keyConvertor: fastjson
      valueEncode: java
      valueDecode: java
      poolConfig:
        maxTotal: 50
    sms:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

​		由于redis缓存中不支持保存对象，因此需要对redis设置当Object类型数据进入到redis中时如何进行类型转换。需要配置keyConvertor表示key的类型转换方式，同时标注value的转换类型方式，值进入redis时是java类型，标注valueEncode为java，值从redis中读取时转换成java，标注valueDecode为java。

​		注意，为了实现Object类型的值进出redis，需要保障进出redis的Object类型的数据必须实现序列化接口。

```JAVA
@Data
public class Book implements Serializable {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

**步骤③**：启用缓存时开启方法缓存功能，并配置basePackages，说明在哪些包中开启方法缓存

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation
//开启方法注解缓存
@EnableMethodCache(basePackages = "com.itheima")
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：使用注解@Cached标注当前方法使用缓存

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
    
    @Override
    @Cached(name="book_",key="#id",expire = 3600,cacheType = CacheType.REMOTE)
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }
}
```

##### 6.4 远程方案的数据同步

​		由于远程方案中redis保存的数据可以被多个客户端共享，这就存在了数据同步问题。jetcache提供了3个注解解决此问题，分别在更新、删除操作时同步缓存数据，和读取缓存时定时刷新数据

**更新缓存**

```JAVA
@CacheUpdate(name="book_",key="#book.id",value="#book")
public boolean update(Book book) {
    return bookDao.updateById(book) > 0;
}
```

**删除缓存**

```JAVA
@CacheInvalidate(name="book_",key = "#id")
public boolean delete(Integer id) {
    return bookDao.deleteById(id) > 0;
}
```

**定时刷新缓存**

```JAVA
@Cached(name="book_",key="#id",expire = 3600,cacheType = CacheType.REMOTE)
@CacheRefresh(refresh = 5)
public Book getById(Integer id) {
    return bookDao.selectById(id);
}
```

##### 6.5 数据报表

​		jetcache还提供有简单的数据报表功能，帮助开发者快速查看缓存命中信息，只需要添加一个配置即可

```yaml
jetcache:
  statIntervalMinutes: 1
```

​		设置后，每1分钟在控制台输出缓存数据命中信息

```CMD
[DefaultExecutor] c.alicp.jetcache.support.StatInfoLogger  : jetcache stat from 2022-02-28 09:32:15,892 to 2022-02-28 09:33:00,003
cache    |    qps|   rate|   get|    hit|   fail|   expire|   avgLoadTime|   maxLoadTime
---------+-------+-------+------+-------+-------+---------+--------------+--------------
book_    |   0.66| 75.86%|    29|     22|      0|        0|          28.0|           188
---------+-------+-------+------+-------+-------+---------+--------------+--------------
```

**总结**

1. jetcache是一个类似于springcache的缓存解决方案，自身不具有缓存功能，它提供有本地缓存与远程缓存多级共同使用的缓存解决方案
2. jetcache提供的缓存解决方案受限于目前支持的方案，本地缓存支持两种，远程缓存支持两种
3. 注意数据进入远程缓存时的类型转换问题
4. jetcache提供方法缓存，并提供了对应的缓存更新与刷新功能
5. jetcache提供有简单的缓存信息命中报表方便开发者即时监控缓存数据命中情况

**思考**

​		jetcache解决了前期使用缓存方案单一的问题，但是仍然不能灵活的选择缓存进行搭配使用，是否存在一种技术可以灵活的搭配各种各样的缓存使用呢？有，咱们下一节再讲。

#### 7. SpringBoot整合j2cache缓存

​		jetcache可以在限定范围内构建多级缓存，但是灵活性不足，不能随意搭配缓存，本节介绍一种可以随意搭配缓存解决方案的缓存整合框架，j2cache。下面就来讲解如何使用这种缓存框架，以Ehcache与redis整合为例：

**步骤①**：导入j2cache、redis、ehcache坐标

```xml
<dependency>
    <groupId>net.oschina.j2cache</groupId>
    <artifactId>j2cache-core</artifactId>
    <version>2.8.4-release</version>
</dependency>
<dependency>
    <groupId>net.oschina.j2cache</groupId>
    <artifactId>j2cache-spring-boot2-starter</artifactId>
    <version>2.8.0-release</version>
</dependency>
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

​		j2cache的starter中默认包含了redis坐标，官方推荐使用redis作为二级缓存，因此此处无需导入redis坐标

**步骤②**：配置一级与二级缓存，并配置一二级缓存间数据传递方式，配置书写在名称为j2cache.properties的文件中。如果使用ehcache还需要单独添加ehcache的配置文件

```yaml
# 1级缓存
j2cache.L1.provider_class = ehcache
ehcache.configXml = ehcache.xml

# 2级缓存
j2cache.L2.provider_class = net.oschina.j2cache.cache.support.redis.SpringRedisProvider
j2cache.L2.config_section = redis
redis.hosts = localhost:6379

# 1级缓存中的数据如何到达二级缓存
j2cache.broadcast = net.oschina.j2cache.cache.support.redis.SpringRedisPubSubPolicy
```

​		此处配置不能乱配置，需要参照官方给出的配置说明进行。例如1级供应商选择ehcache，供应商名称仅仅是一个ehcache，但是2级供应商选择redis时要写专用的Spring整合Redis的供应商类名SpringRedisProvider，而且这个名称并不是所有的redis包中能提供的，也不是spring包中提供的。因此配置j2cache必须参照官方文档配置，而且还要去找专用的整合包，导入对应坐标才可以使用。

​		一级与二级缓存最重要的一个配置就是两者之间的数据沟通方式，此类配置也不是随意配置的，并且不同的缓存解决方案提供的数据沟通方式差异化很大，需要查询官方文档进行设置。

**步骤③**：使用缓存

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;

    @Autowired
    private CacheChannel cacheChannel;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        cacheChannel.set("sms",tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = cacheChannel.get("sms",smsCode.getTele()).asString();
        return smsCode.getCode().equals(code);
    }
}
```

​		j2cache的使用和jetcache比较类似，但是无需开启使用的开关，直接定义缓存对象即可使用，缓存对象名CacheChannel。

​		j2cache的使用不复杂，配置是j2cache的核心，毕竟是一个整合型的缓存框架。缓存相关的配置过多，可以查阅j2cache-core核心包中的j2cache.properties文件中的说明。如下：

```properties
#J2Cache configuration
#########################################
# Cache Broadcast Method
# values:
# jgroups -> use jgroups's multicast
# redis -> use redis publish/subscribe mechanism (using jedis)
# lettuce -> use redis publish/subscribe mechanism (using lettuce, Recommend)
# rabbitmq -> use RabbitMQ publisher/consumer mechanism
# rocketmq -> use RocketMQ publisher/consumer mechanism
# none -> don't notify the other nodes in cluster
# xx.xxxx.xxxx.Xxxxx your own cache broadcast policy classname that implement net.oschina.j2cache.cluster.ClusterPolicy
#########################################
j2cache.broadcast = redis

# jgroups properties
jgroups.channel.name = j2cache
jgroups.configXml = /network.xml

# RabbitMQ properties
rabbitmq.exchange = j2cache
rabbitmq.host = localhost
rabbitmq.port = 5672
rabbitmq.username = guest
rabbitmq.password = guest

# RocketMQ properties
rocketmq.name = j2cache
rocketmq.topic = j2cache
# use ; to split multi hosts
rocketmq.hosts = 127.0.0.1:9876

#########################################
# Level 1&2 provider
# values:
# none -> disable this level cache
# ehcache -> use ehcache2 as level 1 cache
# ehcache3 -> use ehcache3 as level 1 cache
# caffeine -> use caffeine as level 1 cache(only in memory)
# redis -> use redis as level 2 cache (using jedis)
# lettuce -> use redis as level 2 cache (using lettuce)
# readonly-redis -> use redis as level 2 cache ,but never write data to it. if use this provider, you must uncomment `j2cache.L2.config_section` to make the redis configurations available.
# memcached -> use memcached as level 2 cache (xmemcached),
# [classname] -> use custom provider
#########################################

j2cache.L1.provider_class = caffeine
j2cache.L2.provider_class = redis

# When L2 provider isn't `redis`, using `L2.config_section = redis` to read redis configurations
# j2cache.L2.config_section = redis

# Enable/Disable ttl in redis cache data (if disabled, the object in redis will never expire, default:true)
# NOTICE: redis hash mode (redis.storage = hash) do not support this feature)
j2cache.sync_ttl_to_redis = true

# Whether to cache null objects by default (default false)
j2cache.default_cache_null_object = true

#########################################
# Cache Serialization Provider
# values:
# fst -> using fast-serialization (recommend)
# kryo -> using kryo serialization
# json -> using fst's json serialization (testing)
# fastjson -> using fastjson serialization (embed non-static class not support)
# java -> java standard
# fse -> using fse serialization
# [classname implements Serializer]
#########################################

j2cache.serialization = json
#json.map.person = net.oschina.j2cache.demo.Person

#########################################
# Ehcache configuration
#########################################

# ehcache.configXml = /ehcache.xml

# ehcache3.configXml = /ehcache3.xml
# ehcache3.defaultHeapSize = 1000

#########################################
# Caffeine configuration
# caffeine.region.[name] = size, xxxx[s|m|h|d]
#
#########################################
caffeine.properties = /caffeine.properties

#########################################
# Redis connection configuration
#########################################

#########################################
# Redis Cluster Mode
#
# single -> single redis server
# sentinel -> master-slaves servers
# cluster -> cluster servers (数据库配置无效，使用 database = 0）
# sharded -> sharded servers  (密码、数据库必须在 hosts 中指定，且连接池配置无效 ; redis://user:password@127.0.0.1:6379/0）
#
#########################################

redis.mode = single

#redis storage mode (generic|hash)
redis.storage = generic

## redis pub/sub channel name
redis.channel = j2cache
## redis pub/sub server (using redis.hosts when empty)
redis.channel.host =

#cluster name just for sharded
redis.cluster_name = j2cache

## redis cache namespace optional, default[empty]
redis.namespace =

## redis command scan parameter count, default[1000]
#redis.scanCount = 1000

## connection
# Separate multiple redis nodes with commas, such as 192.168.0.10:6379,192.168.0.11:6379,192.168.0.12:6379

redis.hosts = 127.0.0.1:6379
redis.timeout = 2000
redis.password =
redis.database = 0
redis.ssl = false

## redis pool properties
redis.maxTotal = 100
redis.maxIdle = 10
redis.maxWaitMillis = 5000
redis.minEvictableIdleTimeMillis = 60000
redis.minIdle = 1
redis.numTestsPerEvictionRun = 10
redis.lifo = false
redis.softMinEvictableIdleTimeMillis = 10
redis.testOnBorrow = true
redis.testOnReturn = false
redis.testWhileIdle = true
redis.timeBetweenEvictionRunsMillis = 300000
redis.blockWhenExhausted = false
redis.jmxEnabled = false

#########################################
# Lettuce scheme
#
# redis -> single redis server
# rediss -> single redis server with ssl
# redis-sentinel -> redis sentinel
# redis-cluster -> cluster servers
#
#########################################

#########################################
# Lettuce Mode
#
# single -> single redis server
# sentinel -> master-slaves servers
# cluster -> cluster servers (数据库配置无效，使用 database = 0）
# sharded -> sharded servers  (密码、数据库必须在 hosts 中指定，且连接池配置无效 ; redis://user:password@127.0.0.1:6379/0）
#
#########################################

## redis command scan parameter count, default[1000]
#lettuce.scanCount = 1000
lettuce.mode = single
lettuce.namespace =
lettuce.storage = hash
lettuce.channel = j2cache
lettuce.scheme = redis
lettuce.hosts = 127.0.0.1:6379
lettuce.password =
lettuce.database = 0
lettuce.sentinelMasterId =
lettuce.maxTotal = 100
lettuce.maxIdle = 10
lettuce.minIdle = 10
# timeout in milliseconds
lettuce.timeout = 10000
# redis cluster topology refresh interval in milliseconds
lettuce.clusterTopologyRefresh = 3000

#########################################
# memcached server configurations
# refer to https://gitee.com/mirrors/XMemcached
#########################################

memcached.servers = 127.0.0.1:11211
memcached.username =
memcached.password =
memcached.connectionPoolSize = 10
memcached.connectTimeout = 1000
memcached.failureMode = false
memcached.healSessionInterval = 1000
memcached.maxQueuedNoReplyOperations = 100
memcached.opTimeout = 100
memcached.sanitizeKeys = false
```

**总结**

1. j2cache是一个缓存框架，自身不具有缓存功能，它提供多种缓存整合在一起使用的方案
2. j2cache需要通过复杂的配置设置各级缓存，以及缓存之间数据交换的方式
3. j2cache操作接口通过CacheChannel实现







## 第6章 监控

​	在说监控之前，需要回顾一下软件业的发展史。最早的软件完成一些非常简单的功能，代码不多，错误也少。随着软件功能的逐步完善，软件的功能变得越来越复杂，功能不能得到有效的保障，这个阶段出现了针对软件功能的检测，也就是软件测试。伴随着计算机操作系统的逐步升级，软件的运行状态也变得开始让人捉摸不透，出现了不稳定的状况。伴随着计算机网络的发展，程序也从单机状态切换成基于计算机网络的程序，应用于网络的程序开始出现，由于网络的不稳定性，程序的运行状态让使用者更加堪忧。互联网的出现彻底打破了软件的思维模式，随之而来的互联网软件就更加凸显出应对各种各样复杂的网络情况之下的弱小。计算机软件的运行状况已经成为了软件运行的一个大话题，针对软件的运行状况就出现了全新的思维，建立起了初代的软件运行状态监控。

​		什么是监控？就是通过软件的方式展示另一个软件的运行情况，运行的情况则通过各种各样的指标数据反馈给监控人员。例如网络是否顺畅、服务器是否在运行、程序的功能是否能够整百分百运行成功，内存是否够用，等等等等。

​		本章要讲解的监控就是对软件的运行情况进行监督，但是springboot程序与非springboot程序的差异还是很大的，为了方便监控软件的开发，springboot提供了一套功能接口，为开发者加速开发过程。



### KF-6-1.监控的意义

​		对于现代的互联网程序来说，规模越来越大，功能越来越复杂，还要追求更好的客户体验，因此要监控的信息量也就比较大了。由于现在的互联网程序大部分都是基于微服务的程序，一个程序的运行需要若干个服务来保障，因此第一个要监控的指标就是服务是否正常运行，也就是**监控服务状态是否处理宕机状态**。一旦发现某个服务宕机了，必须马上给出对应的解决方案，避免整体应用功能受影响。其次，由于互联网程序服务的客户量是巨大的，当客户的请求在短时间内集中达到服务器后，就会出现各种程序运行指标的波动。比如内存占用严重，请求无法及时响应处理等，这就是第二个要监控的重要指标，**监控服务运行指标**。虽然软件是对外提供用户的访问需求，完成对应功能的，但是后台的运行是否平稳，是否出现了不影响客户使用的功能隐患，这些也是要密切监控的，此时就需要在不停机的情况下，监控系统运行情况，日志是一个不错的手段。如果在众多日志中找到开发者或运维人员所关注的日志信息，简单快速有效的过滤出要看的日志也是监控系统需要考虑的问题，这就是第三个要监控的指标，**监控程序运行日志**。虽然我们期望程序一直平稳运行，但是由于突发情况的出现，例如服务器被攻击、服务器内存溢出等情况造成了服务器宕机，此时当前服务不能满足使用需要，就要将其重启甚至关闭，如果快速控制服务器的启停也是程序运行过程中不可回避的问题，这就是第四个监控项，**管理服务状态**。以上这些仅仅是从大的方面来思考监控这个问题，还有很多的细节点，例如上线了一个新功能，定时提醒用户续费，这种功能不是上线后马上就运行的，但是当前功能是否真的启动，如果快速的查询到这个功能已经开启，这也是监控中要解决的问题，等等。看来监控真的是一项非常重要的工作。

​		通过上述描述，可以看出监控很重要。那具体的监控要如何开展呢？还要从实际的程序运行角度出发。比如现在有3个服务支撑着一个程序的运行，每个服务都有自己的运行状态。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301093704396.png" alt="image-20220301093704396" style="zoom:50%;" />

​		此时被监控的信息就要在三个不同的程序中去查询并展示，但是三个服务是服务于一个程序的运行的，如果不能合并到一个平台上展示，监控工作量巨大，而且信息对称性差，要不停的在三个监控端查看数据。如果将业务放大成30个，300个，3000个呢？看来必须有一个单独的平台，将多个被监控的服务对应的监控指标信息汇总在一起，这样更利于监控工作的开展。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301094001896.png" alt="image-20220301094001896" style="zoom:50%;" />

​		新的程序专门用来监控，新的问题就出现了，是被监控程序主动上报信息还是监控程序主动获取信息？如果监控程序不能主动获取信息，这就意味着监控程序有可能看到的是很久之前被监控程序上报的信息，万一被监控程序宕机了，监控程序就无法区分究竟是好久没法信息了，还是已经下线了。所以监控程序必须具有主动发起请求获取被监控服务信息的能力。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301094259844.png" alt="image-20220301094259844" style="zoom:50%;" />

​		如果监控程序要监控服务时，主动获取对方的信息。那监控程序如何知道哪些程序被自己监控呢？不可能在监控程序中设置我监控谁，这样互联网上的所有程序岂不是都可以被监控到，这样的话信息安全将无法得到保障。合理的做法只能是在被监控程序启动时上报监控程序，告诉监控程序你可以监控我了。看来需要在被监控程序端做主动上报的操作，这就要求被监控程序中配置对应的监控程序是谁。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301094547748.png" alt="image-20220301094547748" style="zoom:50%;" />

​		被监控程序可以提供各种各样的指标数据给监控程序看，但是每一个指标都代表着公司的机密信息，并不是所有的指标都可以给任何人看的，乃至运维人员，所以对被监控指标的是否开放出来给监控系统看，也需要做详细的设定。

​		以上描述的整个过程就是一个监控系统的基本流程。

**总结**

1. 监控是一个非常重要的工作，是保障程序正常运行的基础手段
2. 监控的过程通过一个监控程序进行，它汇总所有被监控的程序的信息集中统一展示
3. 被监控程序需要主动上报自己被监控，同时要设置哪些指标被监控

**思考**

​		下面就要开始做监控了，新的问题就来了，监控程序怎么做呢？难道要自己写吗？肯定是不现实的，如何进行监控，咱们下节再讲。



### KF-6-2.可视化监控平台

​		springboot抽取了大部分监控系统的常用指标，提出了监控的总思想。然后就有好心的同志根据监控的总思想，制作了一个通用性很强的监控系统，因为是基于springboot监控的核心思想制作的，所以这个程序被命名为**Spring Boot Admin**。

​		Spring Boot Admin，这是一个开源社区项目，用于管理和监控SpringBoot应用程序。这个项目中包含有客户端和服务端两部分，而监控平台指的就是服务端。我们做的程序如果需要被监控，将我们做的程序制作成客户端，然后配置服务端地址后，服务端就可以通过HTTP请求的方式从客户端获取对应的信息，并通过UI界面展示对应信息。

​		下面就来开发这套监控程序，先制作服务端，其实服务端可以理解为是一个web程序，收到一些信息后展示这些信息。

**服务端开发**

**步骤①**：导入springboot admin对应的starter，版本与当前使用的springboot版本保持一致，并将其配置成web工程

```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-server</artifactId>
    <version>2.5.4</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

​		上述过程可以通过创建项目时使用勾选的形式完成。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301102432817.png" alt="image-20220301102432817" style="zoom:50%;" />

**步骤②**：在引导类上添加注解@EnableAdminServer，声明当前应用启动后作为SpringBootAdmin的服务器使用

```java
@SpringBootApplication
@EnableAdminServer
public class Springboot25AdminServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot25AdminServerApplication.class, args);
    }
}
```

​		做到这里，这个服务器就开发好了，启动后就可以访问当前程序了，界面如下。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301103028468.png" alt="image-20220301103028468" style="zoom: 50%;" />

​		由于目前没有启动任何被监控的程序，所以里面什么信息都没有。下面制作一个被监控的客户端程序。

**客户端开发**

​		客户端程序开发其实和服务端开发思路基本相似，多了一些配置而已。

**步骤①**：导入springboot admin对应的starter，版本与当前使用的springboot版本保持一致，并将其配置成web工程

```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>2.5.4</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

​		上述过程也可以通过创建项目时使用勾选的形式完成，不过一定要小心，端口配置成不一样的，否则会冲突。

**步骤②**：设置当前客户端将信息上传到哪个服务器上，通过yml文件配置

```yaml
spring:
  boot:
    admin:
      client:
        url: http://localhost:8080
```

​		做到这里，这个客户端就可以启动了。启动后再次访问服务端程序，界面如下。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301103838079.png" alt="image-20220301103838079" style="zoom: 50%;" />

​		可以看到，当前监控了1个程序，点击进去查看详细信息。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301103936386.png" alt="image-20220301103936386" style="zoom: 50%;" />

​		由于当前没有设置开放哪些信息给监控服务器，所以目前看不到什么有效的信息。下面需要做两组配置就可以看到信息了。

1. 开放指定信息给服务器看

2. 允许服务器以HTTP请求的方式获取对应的信息

   配置如下：

```yaml
server:
  port: 80
spring:
  boot:
    admin:
      client:
        url: http://localhost:8080
management:
  endpoint:
    health:
      show-details: always
  endpoints:
    web:
      exposure:
        include: "*"
```

​		上述配置对于初学者来说比较容易混淆。简单解释一下，到下一节再做具体的讲解。springbootadmin的客户端默认开放了13组信息给服务器，但是这些信息除了一个之外，其他的信息都不让通过HTTP请求查看。所以你看到的信息基本上就没什么内容了，只能看到一个内容，就是下面的健康信息。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301104742563.png" alt="image-20220301104742563" style="zoom: 50%;" />

​		但是即便如此我们看到健康信息中也没什么内容，原因在于健康信息中有一些信息描述了你当前应用使用了什么技术等信息，如果无脑的对外暴露功能会有安全隐患。通过配置就可以开放所有的健康信息明细查看了。

```yaml
management:
  endpoint:
    health:
      show-details: always
```

​		健康明细信息如下：

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301105116554.png" alt="image-20220301105116554" style="zoom: 50%;" />

​		目前除了健康信息，其他信息都查阅不了。原因在于其他12种信息是默认不提供给服务器通过HTTP请求查阅的，所以需要开启查阅的内容项，使用*表示查阅全部。记得带引号。

```yaml
endpoints:
  web:
    exposure:
      include: "*"
```

​		配置后再刷新服务器页面，就可以看到所有的信息了。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301105554494.png" alt="image-20220301105554494" style="zoom: 50%;" />

​		以上界面中展示的信息量就非常大了，包含了13组信息，有性能指标监控，加载的bean列表，加载的系统属性，日志的显示控制等等。

**配置多个客户端**

​		可以通过配置客户端的方式在其他的springboot程序中添加客户端坐标，这样当前服务器就可以监控多个客户端程序了。每个客户端展示不同的监控信息。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301110352170.png" alt="image-20220301110352170" style="zoom: 50%;" />

​		进入监控面板，如果你加载的应用具有功能，在监控面板中可以看到3组信息展示的与之前加载的空工程不一样。

- 类加载面板中可以查阅到开发者自定义的类，如左图

​                        <img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301161246835.png" alt="image-20220301161246835" style="zoom:33%;" /><img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301161949431.png" alt="image-20220301161949431" style="zoom:33%;" />

- 映射中可以查阅到当前应用配置的所有请求

​                        <img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301161418791.png" alt="image-20220301161418791" style="zoom: 33%;" /><img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301162008737.png" alt="image-20220301162008737" style="zoom:33%;" />

- 性能指标中可以查阅当前应用独有的请求路径统计数据

​                        <img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301161906949.png" alt="image-20220301161906949" style="zoom: 33%;" /><img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301162040670.png" alt="image-20220301162040670" style="zoom: 33%;" />

**总结**

1. 开发监控服务端需要导入坐标，然后在引导类上添加注解@EnableAdminServer，并将其配置成web程序即可
2. 开发被监控的客户端需要导入坐标，然后配置服务端服务器地址，并做开放指标的设定即可
3. 在监控平台中可以查阅到各种各样被监控的指标，前提是客户端开放了被监控的指标

**思考**

​		之前说过，服务端要想监控客户端，需要主动的获取到对应信息并展示出来。但是目前我们并没有在客户端开发任何新的功能，但是服务端确可以获取监控信息，谁帮我们做的这些功能呢？咱们下一节再讲。



### KF-6-3.监控原理

​		通过查阅监控中的映射指标，可以看到当前系统中可以运行的所有请求路径，其中大部分路径以/actuator开头

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301170214076.png" alt="image-20220301170214076" style="zoom: 50%;" />

​		首先这些请求路径不是开发者自己编写的，其次这个路径代表什么含义呢？既然这个路径可以访问，就可以通过浏览器发送该请求看看究竟可以得到什么信息。

![image-20220301170723057](E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301170723057.png)

​		通过发送请求，可以得到一组json信息，如下

```json
{
    "_links": {
        "self": {
            "href": "http://localhost:81/actuator",
            "templated": false
        },
        "beans": {
            "href": "http://localhost:81/actuator/beans",
            "templated": false
        },
        "caches-cache": {
            "href": "http://localhost:81/actuator/caches/{cache}",
            "templated": true
        },
        "caches": {
            "href": "http://localhost:81/actuator/caches",
            "templated": false
        },
        "health": {
            "href": "http://localhost:81/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:81/actuator/health/{*path}",
            "templated": true
        },
        "info": {
            "href": "http://localhost:81/actuator/info",
            "templated": false
        },
        "conditions": {
            "href": "http://localhost:81/actuator/conditions",
            "templated": false
        },
        "shutdown": {
            "href": "http://localhost:81/actuator/shutdown",
            "templated": false
        },
        "configprops": {
            "href": "http://localhost:81/actuator/configprops",
            "templated": false
        },
        "configprops-prefix": {
            "href": "http://localhost:81/actuator/configprops/{prefix}",
            "templated": true
        },
        "env": {
            "href": "http://localhost:81/actuator/env",
            "templated": false
        },
        "env-toMatch": {
            "href": "http://localhost:81/actuator/env/{toMatch}",
            "templated": true
        },
        "loggers": {
            "href": "http://localhost:81/actuator/loggers",
            "templated": false
        },
        "loggers-name": {
            "href": "http://localhost:81/actuator/loggers/{name}",
            "templated": true
        },
        "heapdump": {
            "href": "http://localhost:81/actuator/heapdump",
            "templated": false
        },
        "threaddump": {
            "href": "http://localhost:81/actuator/threaddump",
            "templated": false
        },
        "metrics-requiredMetricName": {
            "href": "http://localhost:81/actuator/metrics/{requiredMetricName}",
            "templated": true
        },
        "metrics": {
            "href": "http://localhost:81/actuator/metrics",
            "templated": false
        },
        "scheduledtasks": {
            "href": "http://localhost:81/actuator/scheduledtasks",
            "templated": false
        },
        "mappings": {
            "href": "http://localhost:81/actuator/mappings",
            "templated": false
        }
    }
}
```

​		其中每一组数据都有一个请求路径，而在这里请求路径中有之前看到过的health，发送此请求又得到了一组信息

```JSON
{
    "status": "UP",
    "components": {
        "diskSpace": {
            "status": "UP",
            "details": {
                "total": 297042808832,
                "free": 72284409856,
                "threshold": 10485760,
                "exists": true
            }
        },
        "ping": {
            "status": "UP"
        }
    }
}
```

​		当前信息与监控面板中的数据存在着对应关系

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301171025615.png" alt="image-20220301171025615" style="zoom:50%;" />

​		原来监控中显示的信息实际上是通过发送请求后得到json数据，然后展示出来。按照上述操作，可以发送更多的以/actuator开头的链接地址，获取更多的数据，这些数据汇总到一起组成了监控平台显示的所有数据。

​		到这里我们得到了一个核心信息，监控平台中显示的信息实际上是通过对被监控的应用发送请求得到的。那这些请求谁开发的呢？打开被监控应用的pom文件，其中导入了springboot admin的对应的client，在这个资源中导入了一个名称叫做actuator的包。被监控的应用之所以可以对外提供上述请求路径，就是因为添加了这个包。

![image-20220301171437817](E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301171437817.png)

​		这个actuator是什么呢？这就是本节要讲的核心内容，监控的端点。

​		Actuator，可以称为端点，描述了一组监控信息，SpringBootAdmin提供了多个内置端点，通过访问端点就可以获取对应的监控信息，也可以根据需要自定义端点信息。通过发送请求路劲**/actuator**可以访问应用所有端点信息，如果端点中还有明细信息可以发送请求**/actuator/端点名称**来获取详细信息。以下列出了所有端点信息说明：

| ID               | 描述                                                         | 默认启用 |
| ---------------- | ------------------------------------------------------------ | -------- |
| auditevents      | 暴露当前应用程序的审计事件信息。                             | 是       |
| beans            | 显示应用程序中所有 Spring bean 的完整列表。                  | 是       |
| caches           | 暴露可用的缓存。                                             | 是       |
| conditions       | 显示在配置和自动配置类上评估的条件以及它们匹配或不匹配的原因。 | 是       |
| configprops      | 显示所有 @ConfigurationProperties 的校对清单。               | 是       |
| env              | 暴露 Spring ConfigurableEnvironment 中的属性。               | 是       |
| flyway           | 显示已应用的 Flyway 数据库迁移。                             | 是       |
| health           | 显示应用程序健康信息                                         | 是       |
| httptrace        | 显示 HTTP 追踪信息（默认情况下，最后 100 个  HTTP 请求/响应交换）。 | 是       |
| info             | 显示应用程序信息。                                           | 是       |
| integrationgraph | 显示 Spring Integration 图。                                 | 是       |
| loggers          | 显示和修改应用程序中日志记录器的配置。                       | 是       |
| liquibase        | 显示已应用的 Liquibase 数据库迁移。                          | 是       |
| metrics          | 显示当前应用程序的指标度量信息。                             | 是       |
| mappings         | 显示所有 @RequestMapping 路径的整理清单。                    | 是       |
| scheduledtasks   | 显示应用程序中的调度任务。                                   | 是       |
| sessions         | 允许从 Spring Session 支持的会话存储中检索和删除用户会话。当使用 Spring Session 的响应式 Web 应用程序支持时不可用。 | 是       |
| shutdown         | 正常关闭应用程序。                                           | 否       |
| threaddump       | 执行线程 dump。                                              | 是       |
| heapdump         | 返回一个 hprof 堆 dump 文件。                                | 是       |
| jolokia          | 通过 HTTP 暴露 JMX bean（当  Jolokia 在 classpath 上时，不适用于 WebFlux）。 | 是       |
| logfile          | 返回日志文件的内容（如果已设置 logging.file 或 logging.path 属性）。支持使用 HTTP Range 头来检索部分日志文件的内容。 | 是       |
| prometheus       | 以可以由 Prometheus 服务器抓取的格式暴露指标。               | 是       |

​		上述端点每一项代表被监控的指标，如果对外开放则监控平台可以查询到对应的端点信息，如果未开放则无法查询对应的端点信息。通过配置可以设置端点是否对外开放功能。使用enable属性控制端点是否对外开放。其中health端点为默认端点，不能关闭。

```yaml
management:
  endpoint:
    health:						# 端点名称
      show-details: always
    info:						# 端点名称
      enabled: true				# 是否开放
```

​		为了方便开发者快速配置端点，springboot admin设置了13个较为常用的端点作为默认开放的端点，如果需要控制默认开放的端点的开放状态，可以通过配置设置，如下：

```YAML
management:
  endpoints:
    enabled-by-default: true	# 是否开启默认端点，默认值true
```

​		上述端点开启后，就可以通过端点对应的路径查看对应的信息了。但是此时还不能通过HTTP请求查询此信息，还需要开启通过HTTP请求查询的端点名称，使用“*”可以简化配置成开放所有端点的WEB端HTTP请求权限。

```YAML
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

​		整体上来说，对于端点的配置有两组信息，一组是endpoints开头的，对所有端点进行配置，一组是endpoint开头的，对具体端点进行配置。

```YAML
management:
  endpoint:		# 具体端点的配置
    health:
      show-details: always
    info:
      enabled: true
  endpoints:	# 全部端点的配置
    web:
      exposure:
        include: "*"
    enabled-by-default: true
```

**总结**

1. 被监控客户端通过添加actuator的坐标可以对外提供被访问的端点功能

2. 端点功能的开放与关闭可以通过配置进行控制

3. web端默认无法获取所有端点信息，通过配置开放端点功能

   

### KF-6-4.自定义监控指标

​		端点描述了被监控的信息，除了系统默认的指标，还可以自行添加显示的指标，下面就通过3种不同的端点的指标自定义方式来学习端点信息的二次开发。

**INFO端点**

​		info端点描述了当前应用的基本信息，可以通过两种形式快速配置info端点的信息

- 配置形式

  在yml文件中通过设置info节点的信息就可以快速配置端点信息

  ```yaml
  info:
    appName: @project.artifactId@
    version: @project.version@
    company: 传智教育
    author: itheima
  ```

  配置完毕后，对应信息显示在监控平台上

  <img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301174133248.png" alt="image-20220301174133248" style="zoom:50%;" />

  也可以通过请求端点信息路径获取对应json信息

  <img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301174241310.png" alt="image-20220301174241310" style="zoom:50%;" />

- 编程形式

  通过配置的形式只能添加固定的数据，如果需要动态数据还可以通过配置bean的方式为info端点添加信息，此信息与配置信息共存

  ```JAVA
  @Component
  public class InfoConfig implements InfoContributor {
      @Override
      public void contribute(Info.Builder builder) {
          builder.withDetail("runTime",System.currentTimeMillis());		//添加单个信息
          Map infoMap = new HashMap();		
          infoMap.put("buildTime","2006");
          builder.withDetails(infoMap);									//添加一组信息
      }
  }
  ```

**Health端点**

​		health端点描述当前应用的运行健康指标，即应用的运行是否成功。通过编程的形式可以扩展指标信息。

```JAVA
@Component
public class HealthConfig extends AbstractHealthIndicator {
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        boolean condition = true;
        if(condition) {
            builder.status(Status.UP);					//设置运行状态为启动状态
            builder.withDetail("runTime", System.currentTimeMillis());
            Map infoMap = new HashMap();
            infoMap.put("buildTime", "2006");
            builder.withDetails(infoMap);
        }else{
            builder.status(Status.OUT_OF_SERVICE);		//设置运行状态为不在服务状态
            builder.withDetail("上线了吗？","你做梦");
        }
    }
}
```

​		当任意一个组件状态不为UP时，整体应用对外服务状态为非UP状态。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301174751845.png" alt="image-20220301174751845" style="zoom:50%;" />

**Metrics端点**

​		metrics端点描述了性能指标，除了系统自带的监控性能指标，还可以自定义性能指标。

```JAVA
@Service
public class BookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {
    @Autowired
    private BookDao bookDao;

    private Counter counter;

    public BookServiceImpl(MeterRegistry meterRegistry){
        counter = meterRegistry.counter("用户付费操作次数：");
    }

    @Override
    public boolean delete(Integer id) {
        //每次执行删除业务等同于执行了付费业务
        counter.increment();
        return bookDao.deleteById(id) > 0;
    }
}
```

​		在性能指标中就出现了自定义的性能指标监控项

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301175101812.png" alt="image-20220301175101812" style="zoom:50%;" />

**自定义端点**

​		可以根据业务需要自定义端点，方便业务监控

```JAVA
@Component
@Endpoint(id="pay",enableByDefault = true)
public class PayEndpoint {
    @ReadOperation
    public Object getPay(){
        Map payMap = new HashMap();
        payMap.put("level 1","300");
        payMap.put("level 2","291");
        payMap.put("level 3","666");
        return payMap;
    }
}
```

​		由于此端点数据spirng boot admin无法预知该如何展示，所以通过界面无法看到此数据，通过HTTP请求路径可以获取到当前端点的信息，但是需要先开启当前端点对外功能，或者设置当前端点为默认开发的端点。

<img src="E:/%25E5%25AD%25A6%25E4%25B9%25A0/Java-2022.1/05-SpringBoot/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20220301175355482.png" alt="image-20220301175355482" style="zoom:50%;" />

**总结**

1. 端点的指标可以自定义，但是每种不同的指标根据其功能不同，自定义方式不同
2. info端点通过配置和编程的方式都可以添加端点指标
3. health端点通过编程的方式添加端点指标，需要注意要为对应指标添加启动状态的逻辑设定
4. metrics指标通过在业务中添加监控操作设置指标
5. 可以自定义端点添加更多的指标





![image-20220323202937908](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220323202937908.png)
