title: 07-SpringBoot
date: 2022-02-15 14:28:44
tags: [Java]

# SpringBoot基础篇

## 第1章 SpringBoot入门

### 一、IDEA入门案例

#### 1. 使用SpringBoot快速构建SpringMVC程序

**步骤①**：创建空项目，新建新模块，选择Spring Initializr，并配置模块相关基础信息

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216102549960.png)



**步骤②**：选择当前模块需要使用的技术集

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216102611958.png" style="zoom: 50%;" />



**步骤③**：开发控制器类

```JAVA
//Rest模式
@RestController //=@Controller+@ResposeBody
@RequestMapping("/books")
public class BookController {
    @GetMapping
    public String getById(){
        System.out.println("springboot is running...");
        return "springboot is running...";
    }
}
/*
入门案例制作的SpringMVC的控制器基于Rest风格开发，
当然此处使用原始格式制作SpringMVC的程序也是没有问题的，
上例中的@RestController与@GetMapping注解是基于Restful开发的典型注解
*/
```

​	

**步骤④**：运行自动生成的Application类

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216132819653.png)

访问路径：	http://localhost:8080/books

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216133035706.png)



#### 2. 最简SpringBoot程序包含的基础文件

- pom.xml文件

​	这是maven的配置文件，描述了当前工程构建时相应的配置信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.3</version>
        <relativePath/>
    </parent>
    <groupId>com.lak</groupId>
    <artifactId>springboot_01_01_quickstart</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot_01_01_quickstart</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

配置中有两个信息需要关注，一个是parent，也就是当前工程继承了另外一个工程，还有依赖坐标



- Application类

```JAVA
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```



#### 3. SpringBoot优势

| **类配置文件**         | **Spring**   | **SpringBoot** |
| ---------------------- | ------------ | -------------- |
| pom文件中的坐标        | **手工添加** | **勾选添加**   |
| web3.0配置类           | **手工制作** | **无**         |
| Spring/SpringMVC配置类 | **手工制作** | **无**         |
| 控制器                 | **手工制作** | **手工制作**   |



#### 4. 总结

1. 开发SpringBoot程序可以根据向导进行联网快速制作
2. SpringBoot程序需要基于JDK8以上版本进行制作
3. SpringBoot程序中需要使用何种功能通过勾选选择技术，也可以手工添加对应的要使用的技术（后期讲解）
4. 运行SpringBoot程序通过运行Application程序入口进行



### 二、基于SpringBoot官网创建项目

1. 打开SpringBoot官网，选择Quickstart Your Project

​	`spring.io`

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216134141309.png" style="zoom:50%;" />



<hr>

2. 创建工程并保存项目

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216135713560.png" style="zoom:50%;" />

<hr>

3. 解压项目，通过IDE导入项目



### 三、基于阿里云创建项目

1. 创建工程时，切换选择starter服务路径，然后手工收入阿里云提供给我们的使用地址即可。

​		地址：http://start.aliyun.com或https://start.aliyun.com

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216140259101.png" style="zoom:50%;" />

​	

2. 阿里为了便于自己开发使用，因此在依赖坐标中添加了一些阿里相关的技术。阿里云地址默认创建的SpringBoot工程版本是<font color="#ff0000"><b>2.4.1</b></font>，所以如果想更换其他的版本，创建项目后手工修改即可，刷新一下，加载新版本信息。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216140354960.png" style="zoom:50%;" />

​	<font color="#ff0000"><b>注意</b></font>：阿里云提供的工程创建地址初始化完毕后和实用SpringBoot官网创建出来的工程略有区别。主要是在配置文件的形式上有区别。阿里云提供的坐标版本较低，如果需要使用高版本，进入工程后手工切换SpringBoot版本。阿里云提供的工程模板与Spring官网提供的工程模板略有不同



### 四、手工创建项目（手工导入坐标）

1. 创建普通Maven工程
2. 参照标准SpringBoot工程的pom文件，书写自己的pom文件。继承spring-boot-starter-parent
3. 添加依赖spring-boot-starter-web
4. 制作引导类Application，类上面的注解@SpringBootApplication
5. 创建一个Controller测试

<hr>

*Idea中隐藏指定文件或指定类型文件*

1. 【Files】→【Settings】
2. 【Editor】→【File Types】→【Ignored Files and Folders】
3. 输入要隐藏的名称，支持*号通配符
4. 回车确认添加



## 第2章 SpringBoot简介

> SpringBoot是由Pivotal团队提供的全新框架，其设计目的是用来<font color="#ff0000"><b>简化Spring应用的初始搭建以及开发过程</b></font>。



**Spring程序缺点:**

- 依赖设置繁琐

  - 写Spring程序，使用的技术都要自己一个一个的写

- 配置繁琐

  - 以前写配置类或者配置文件，然后用什么东西就要自己写加载bean这些东西

  

**SpringBoot程序的核心功能及优点：**

- 起步依赖（简化依赖配置）
  - 依赖配置的书写简化就是靠这个起步依赖达成的
- 自动配置（简化常用工程相关配置）
  - 配置过于繁琐，使用自动配置就可以做响应的简化，但是内部还是很复杂的，后面具体展开说
- 辅助功能（内置服务器，……）
  - 除了上面的功能，其实SpringBoot程序还有其他的一些优势，比如我们没有配置Tomcat服务器，但是能正常运行，这是SpringBoot程序的一个可以感知到的功能，也是SpringBoot的辅助功能之一。



### 一、parent

> - 相当于SpringBoot做了无数个技术版本搭配的列表，这个<font color='red'>技术版本搭配列表</font>的名字叫做parent
>
> - parent自身具有很多个版本，每个parent版本中包含有几百个其他技术的版本号，不同的parent间使用的各种技术的版本号有可能会发生变化。当开发者使用某些技术时，直接使用SpringBoot提供的parent就行了，由parent帮助开发者统一的进行各种技术的版本管理
>
> - parent<font color='red'>仅进行版本管理，不负责导入坐标</font>。整体上来说，使用parent可以帮助开发者进行版本的统一管理
>
> - parent定义出来以后，并不是直接使用的，仅仅给了开发者一个说明书，但是并<font color='red'>没有实际使用</font>



**总结**

1. 开发SpringBoot程序要继承spring-boot-starter-parent
2. spring-boot-starter-parent中定义了若干个依赖管理
3. 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突
4. 继承parent的形式也可以采用引入依赖的形式实现效果



### 二、starter

> - starter定义了使用某种技术时对于依赖的固定搭配格式，也是一种最佳解决方案，使用starter可以帮助开发者减少依赖配置。
>
> - 使用starter可以帮开发者快速配置依赖关系。以前写依赖3个坐标的，现在写导入一个就搞定了，就是加速依赖配置的。

<hr>

**starter与parent的区别**

​	<font color="#ff0000">starter</font>是一个坐标中定了若干个坐标，以前写多个的，现在写一个，<font color="#ff0000">是用来减少依赖配置的书写量的</font>。

​	<font color="#ff0000">parent</font>是定义了几百个依赖版本号，以前写依赖需要自己手工控制版本，现在由SpringBoot统一管理，这样就不存在版本冲突了，<font color='red'>是用来减少依赖冲突的</font>。

<hr>

**实际开发应用方式**

- 实际开发中如果需要用什么技术，先去找有没有这个技术对应的starter

  - 如果有对应的starter，直接写starter，而且无需指定版本，版本由parent提供
  - 如果没有对应的starter，手写坐标即可
- 实际开发中如果发现坐标出现了冲突现象，确认你要使用的可行的版本号，使用手工书写的方式添加对应依赖，覆盖SpringBoot提供给我们的配置管理

  - 方式一：直接写坐标
  - 方式二：覆盖`<properties>`中定义的版本号，哪个冲突了覆盖哪个

<hr>

<font color="#f0f"><b>温馨提示</b></font>

​	SpringBoot官方给出了好多个starter的定义，方便我们使用，而且名称都是如下格式

```JAVA
命名规则：spring-boot-starter-技术名称
```

​	所以以后见了spring-boot-starter-aaa这样的名字，这就是SpringBoot官方给出的starter定义。

<hr>

**总结**

1. 开发SpringBoot程序需要导入坐标时通常导入对应的starter
2. 每个不同的starter根据功能不同，通常包含多个依赖坐标
3. 使用starter可以实现快速配置的效果，达到简化配置的目的



### 三、引导类

目前程序运行的入口就是SpringBoot工程创建时自带的那个带有main方法的类，运行这个类就可以启动SpringBoot工程的运行

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

<hr>

​	SpringBoot本身是为了加速Spring程序的开发的，而Spring程序运行的基础是需要创建自己的Spring容器对象（IoC容器）并将所有的对象交给Spring的容器管理，也就是一个一个的Bean。当前这个类运行后就会产生一个Spring容器对象，并且可以将这个对象保存起来，通过容器对象直接操作Bean。

```JAVA
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        ConfigurableApplicationContext ctx = SpringApplication.run(Application.class, args);
        BookController bean = ctx.getBean(BookController.class);
        System.out.println("bean======>" + bean);
    }
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220216172922850.png)	

通过上述操作不难看出，其实SpringBoot程序启动还是创建了一个Spring容器对象。这个类在SpringBoot程序中是所有功能的入口，称这个类为引导类。

作为一个引导类最典型的特征就是当前类上方声明了一个注解<font color="#ff0000">@SpringBootApplication</font>

<HR>

**总结**

1. SpringBoot工程提供引导类用来启动程序
2. SpringBoot工程启动后创建并初始化Spring容器



### 四、内嵌tomcat

​	当前做的SpringBoot入门案例勾选了Spirng-web的功能，并且导入了对应的starter。

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```



#### 1. 内嵌Tomcat定义位置

​	导入的web相关的starter做的这件事。

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

​	打开查看web的starter

​	第三个依赖就是这个tomcat对应的，也是一个starter

```XML
<dependencies>
    
    <dependency>
        <groupId>jakarta.annotation</groupId>
        <artifactId>jakarta.annotation-api</artifactId>
        <version>1.3.5</version>
        <scope>compile</scope>
    </dependency>
    
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-core</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
        <exclusions>
            <exclusion>
                <artifactId>tomcat-annotations-api</artifactId>
                <groupId>org.apache.tomcat</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-el</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
    </dependency>
    
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-websocket</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
        <exclusions>
            <exclusion>
                <artifactId>tomcat-annotations-api</artifactId>
                <groupId>org.apache.tomcat</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    
</dependencies>
```

​	这里面有一个核心的坐标，tomcat-embed-core，叫做tomcat内嵌核心。就是这个东西把tomcat功能引入到了我们的程序中。谁把tomcat引入到程序中的？spring-boot-starter-web中的spring-boot-starter-tomcat做的。



#### 2. 内嵌Tomcat运行原理

​	tomcat服务器运行其实是以对象的形式在Spring容器中运行的。具体运行的就是上前面提到的那个tomcat内嵌核心

```XML
<dependencies>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-core</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

​	通过依赖排除可以去掉这个web服务器功能

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

​	上面对web-starter做了一个操作，使用maven的排除依赖去掉了使用tomcat的starter，容器中肯定没有这个对象了，重新启动程序可以观察到程序运行了，但是并没有像之前那样运行后会等着用户发请求，而是直接停掉了。



#### 3. 更换内嵌Tomcat

​	SpringBoot提供了3款内置的服务器

- tomcat(默认)：apache出品，粉丝多，应用面广，负载了若干较重的组件

- jetty：更轻量级，负载性能远不及tomcat

- undertow：负载性能勉强跑赢tomcat

  想用哪个，加个坐标。前提是把tomcat排除掉，因为tomcat是默认加载的。

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

​	现在就已经成功替换了web服务器，核心思想就是用什么加入对应坐标就可以了。如果有starter，优先使用starter。



**总结**

1. 内嵌Tomcat服务器是SpringBoot辅助功能之一
2. 内嵌Tomcat工作原理是将Tomcat服务器作为对象运行，并将该对象交给Spring容器管理
3. 变更内嵌服务器思想是去除现有服务器，添加全新的服务器

<hr>

### 五、REST风格简介

#### 1. REST简介

> REST(Representational State Transfer) 表现形式状态转换

传统风格资源描述形式

```
http://localhost/user/getById?id=1
http://localhost/user/saveUser
```



REST风格描述形式

```
http://localhost/user/1
http://localhost/user
```



优点：

- 隐藏资源的访问行为，无法通过地址得知对资源是何种操作
- 书写简化

 

按照REST风格访问资源时使用<font color='red'>行为动作</font>区分对资源进行了何种操作

- http://localhost/users         查询全部用户信息     GET（查询）
- http://localhost/users/1     查询指定用户信息     GET（查询）
- http://localhost/users        添加用户信息              POST（新增/保存）
- http://localhost/users         修改用户信息             PUT（修改/更新）
- http://localhost/users/1      删除用户信息            DELETE（删除）

根据REST风格对资源进行访问称为RESTful



注意：

- 上述行是约定方式，约定不是规范，可以打破，所以称REST风格，而不是REST规范
- 描述模块的名称通常使用复数，也就是加s的格式描述，表示此类资源，而非单个资源，例如：users、books、accounts…



#### 2. RESTful入门案例

2.1 设定http请求动作（动词）

```java
@RequestMapping(value="/users",method=RequestMethod.POST)
@ResponseBody
public String save(@RequestBody User user){
    System.out.println("user save..."+user);
    return "{'module':'user save'}";
}

@RequestMapping(value="/users",method=RequestMethod.PUT)
@ResponseBody
public String update(@RequestBody User user){
    System.out.println("user update..."+user);
    return "{'module':'user update'}";
}
```



2.2 设定请求参数（路径变量）

- 名称：@PathVariable

- 类型：形参注解
- 位置：SpringMVC控制器方法形参定义前面
- 作用：绑定路径参数与处理器方法形参间的关系，要求路径参数名与形参名一一对应
- 范例：

```java
@RequestMapping(value="/users/{id}",method=RequestMethod.DELETE)
@ResponseBody
public String delete(@PathVariable Integer id){
    System.out.println("user delete..."+id);
    return "{'module':'user delete'}";
}
```

<hr>

@RequestBody  

@RequestParam

@PathVariable

- 区别
  - @RequestParam用于接收url地址传参或表单传参
  - @RequestBody用于接收json数据
  - @PathVariable用于接收路径参数，使用{参数名称}描述路径参数
- 应用
  - 后期开发中，发送请求参数超过1个时，以json格式为主，@RequestBody应用较广
  - 如果发送非json格式数据，选用@RequestParam接收请求参数
  - 采用RESTful进行开发，当参数数量较少时，例如1个，可以采用@PathVariable接收请求路径变量，通常用于传递id值



#### 3. REST快速开发

- 名称：@RestController
- 类型：类注解
- 位置：基于SpringMVC的RESTful开发控制器类定义上方
- 作用：设置当前控制器为RESTful风格，等同于@Controller与@ResponseBody两个注解组合功能
  - @ResponseBody：将Java对象转为json格式的数据
  - @ResponseBody注解的作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据。使用此注解后不会再走视图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。

- 范例：

```java
@RestController
public class BookController{
}
```

<hr>

- 名称：@GetMapping  @PostMapping @PutMapping @DeleteMapping

- 类型：方法注解
- 位置：基于SpringMVC的RESTful开发控制器方法定义上方
- 作用：设置当前控制器方法请求访问路径与请求动作，每种对应一个请求动作
- 范例：

```java
@GetMapping("/{id}")
public String getById(@PathVariable Integer id){
    ...
}
```

- 属性
  - value（默认）：请求访问路径





## 第3章 SpringBoot基础配置

### 0. 复制工程

**原则**

- 保留工程基础结构

- 抹掉原始工程痕迹



**小结**

1. 在工作空间中复制对应工程，并修改工程名称

2. 删除与Idea相关配置文件，仅保留src目录与pom.xml文件

3. 修改pom.xml文件中的artifactId与新工程/模块名相同

4. 删除name标签（可选）

5. 保留备份工程供后期使用



### 1. 属性配置

> SpringBoot默认配置文件是application.properties

**修改服务器端口**

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220217143727388.png" style="zoom:50%;" />

```properties
server.port=80
```



**关闭运行日志图表（banner)**

```properties
spring.main.banner-mode=off
```



**设置运行日志的显示级别**

```properties
logging.level.root=debug
```

<hr>

​	打开SpringBoot的官网，找到SpringBoot官方文档，打开查看附录中的Application Properties就可以获取到对应的配置项，网址：https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties

​	在pom中注释掉导入的spring-boot-starter-web，然后刷新工程，会发现配置的提示消失了。设定使用了什么技术才能做什么配置。

<font color="#f0f"><b>温馨提示</b></font>

​	所有的starter中都会依赖下面这个starter，叫做spring-boot-starter。这个starter是所有的SpringBoot的starter的基础依赖，里面定义了SpringBoot相关的基础配置。

```JAVA
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.5.4</version>
    <scope>compile</scope>
</dependency>
```

**总结**

1. SpringBoot中导入对应starter后，提供对应配置属性
2. 书写SpringBoot配置采用关键字+提示形式书写



### 2. 配置文件分类

#### 2.1 分类

> SpringBoot除了支持properties格式的配置文件，还支持另外两种格式的配置文件。分别如下:
>
> - properties格式
> - yml格式
> - yaml格式

<hr>

- application.properties（properties格式）

```properties
server.port=80
```

- application.yml（yml格式）

```YML
server:
  port: 81
```

- application.yaml（yaml格式）

```yaml
server:
  port: 82
```

​	仔细看会发现yml格式和yaml格式除了文件名后缀不一样，格式完全一样，是这样的，yml和yaml文件格式就是一模一样的，只是文件后缀不同，所以可以合并成一种格式来看。

**总结**

1. SpringBoot提供了3种配置文件的格式
   - properties（传统格式/默认格式）
   - **yml**（主流格式）
   - yaml



#### 2.2 优先级

> application.properties  >  application.yml  >  application.yaml
>
> 每个配置文件中的项都会生效，只不过如果多个配置文件中有相同类型的配置会优先级高的文件覆盖优先级的文件中的配置。如果配置项不同的话，那所有的配置项都会生效。

<hr>

*自动提示功能消失解决方案*

指定SpringBoot配置文件

- Setting → Project Structure → Facets
- 选中对应项目/工程
- Customize Spring Boot
- 选择配置文件



### 3. yaml文件

> YAML（YAML Ain't Markup Language），一种数据序列化格式。具有容易阅读、容易与脚本语言交互、以数据为核心，重数据轻格式的特点。常见的文件扩展名有两种：
>
> - .yml格式（主流）
> - .yaml格式



**对于文件自身在书写时，具有严格的语法格式要求，具体如下：**

1. 大小写敏感
2. 属性层级关系使用多行描述，每行结尾使用冒号结束
3. 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
4. <font color='red'>属性值前面添加空格</font>（属性名与属性值之间使用冒号+空格作为分隔）
5. #号 表示注释

<hr>

​	常见的数据书写格式

```YAML
boolean: TRUE  						#TRUE,true,True,FALSE,false，False均可
float: 3.14    						#6.8523015e+5  #支持科学计数法
int: 123       						#0b1010_0111_0100_1010_1110    #支持二进制、八进制、十六进制
null: ~        						#使用~表示null
string: HelloWorld      			#字符串可以直接书写
string2: "Hello World"  			#可以使用双引号包裹特殊字符
date: 2018-02-17        			#日期必须使用yyyy-MM-dd格式
datetime: 2018-02-17T15:02:31+08:00  #时间和日期之间使用T连接，最后使用+代表时区
```

​	

此外，yaml格式中也可以表示数组，在属性名书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔

```YAML
subject:
	- Java
	- 前端
	- 大数据
enterprise:
	name: itcast
    age: 16
    subject:
    	- Java
        - 前端
        - 大数据
likes: [王者荣耀,刺激战场]			#数组书写缩略格式
users:							 #对象数组格式一
  - name: Tom
   	age: 4
  - name: Jerry
    age: 5
users:							 #对象数组格式二
  -  
    name: Tom
    age: 4
  -   
    name: Jerry
    age: 5			    
users2: [ { name:Tom , age:4 } , { name:Jerry , age:5 } ]	#对象数组缩略格式
```



### 4. yaml数据读取

#### 4.1 读取单一数据

​	yaml中保存的单个数据，可以使用Spring中的注解直接读取，使用@Value可以读取单个数据，属性名引用方式：<font color="#ff0000"><b>${一级属性名.二级属性名……}</b></font>

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220217151929533.png" style="zoom: 67%;" />

<hr>

在配置文件中可以使用属性名引用方式引用属性

```yml
baseDir: /usr/local/fire

center:
    dataDir: ${baseDir}/data
    tmpDir: ${baseDir}/tmp
    logDir: ${baseDir}/log
    msgDir: ${baseDir}/msgDir
```

属性值中如果出现转义字符，需要使用双引号包裹

```yml
lesson: "Spring\tboot\nlesson"
```



#### 4.2 读取全部数据

**4.2.1 封装全部数据到Environment对象**

​	SpringBoot提供了一个对象，能够把所有的数据都封装到这一个对象中，这个对象叫做Environment，使用自动装配注解@Autowired可以将所有的yaml数据封装到对象Environment中。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220217153604610.png)

​	**4.2.2 获取属性**

​		数据封装到了Environment对象中，获取属性时，通过Environment的接口操作进行，具体方法getProperties（String），参数填写属性名即可



#### 4.3 读取对象数据

​	SpringBoot也提供了可以将一组yaml对象数据封装一个Java对象的操作

**自定义对象封装指定数据**

> 1. 自定义类封装yaml文件中对应的数据
> 2. 定义封装类为Spring管控的bean——@Component
> 3. 使用@ConfigurationProperties注解绑定配置信息到封装类中。这个@ConfigurationProperties必须告诉他加载的数据前缀是什么，这样当前前缀下的所有属性就封装到这个对象中。数据属性名要与对象的变量名一一对应。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220217153756721.png)

<hr>

如果要定义一组数据自己使用，就可以先写一个对象，然后定义好属性，下面到配置中根据这个格式书写即可。

​	![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220217154045177.png)



## 第4章 基于SpringBoot实现SSMP整合

### 一、整合JUnit

```JAVA
@SpringBootTest
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

​	加载的配置类或者配置文件是哪一个？就是启动程序使用的引导类。如果想手工指定引导类有两种方式，第一种方式使用属性的形式进行，在注解@SpringBootTest中添加classes属性指定配置类

```JAVA
@SpringBootTest(classes = Springboot04JunitApplication.class)
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

​	第二种方式回归原始配置方式，仍然使用@ContextConfiguration注解进行，效果是一样的

```JAVA
@SpringBootTest
@ContextConfiguration(classes = Springboot04JunitApplication.class)
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

​	<font color="#f0f"><b>温馨提示</b></font>

​		使用SpringBoot整合JUnit需要保障导入test对应的starter，由于初始化项目时此项是默认导入的，所以此处没有提及。

**总结**

1. 导入测试对应的starter
2. 测试类使用<font color='red'>@SpringBootTest</font>修饰
3. 使用自动装配的形式添加要测试的对象
4. 测试类如果存在于引导类所在包或子包中无需指定引导类
5. 测试类如果不存在于引导类所在的包或子包中需要通过classes属性指定引导类



### 二、整合MyBatis

**步骤①**：创建模块时勾选要使用的技术，MyBatis，由于要操作数据库，还要勾选对应数据库

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220217163751696.png)

​	或者手工导入对应技术的starter，和对应数据库的坐标

```XML
<dependencies>
    <!--1.导入对应的starter-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.2.0</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```



**步骤②**：配置数据源相关信息

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

​	

下面就可以写一下MyBatis程序运行需要的Dao（或者Mapper）就可以运行了

**实体类**

```JAVA
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

**映射接口（Dao）**

```JAVA
@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
```

**测试类**

```JAVA
@SpringBootTest
class Springboot05MybatisApplicationTests {
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        System.out.println(bookDao.getById(1));
    }
}
```

<hr>	

​	<font color="#ff0000"><b>注意</b></font>：当前使用的SpringBoot版本是2.5.4，对应的坐标设置中Mysql驱动使用的是8x版本。当SpringBoot2.4.3（不含）版本之前会出现一个小BUG，就是MySQL驱动升级到8以后要求强制配置时区，如果不设置会出问题。解决方案很简单，驱动url上面添加上对应设置就行了

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
```

​	这里设置的UTC是全球标准时间，你也可以理解为是英国时间，中国处在东八区，需要在这个基础上加上8小时，这样才能和中国地区的时间对应的，也可以修改配置不写UTC，写Asia/Shanghai也可以解决这个问题。

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root
```

​	如果不想每次都设置这个东西，也可以去修改mysql中的配置文件mysql.ini，在mysqld项中添加default-time-zone=+8:00也可以解决这个问题。

​	此外在运行程序时还会给出一个提示，说数据库驱动过时的警告，根据提示修改配置即可，弃用**com.mysql.jdbc.Driver**，换用<font color="#ff0000"><b>com.mysql.cj.jdbc.Driver</b></font>。前面的例子中已经更换了驱动了，在此说明一下。

```tex
Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
```

**总结**

1. 整合操作需要勾选MyBatis技术，也就是导入MyBatis对应的starter

2. 数据库连接相关信息转换成配置

3. 数据库SQL映射需要添加@Mapper被容器识别到

4. MySQL 8.X驱动强制要求设置时区

   - 修改url，添加serverTimezone设定
   - 修改MySQL数据库配置

5. 驱动类过时，提醒更换为com.mysql.cj.jdbc.Driver

   

### 三、整合MyBatis-Plus

做这个整合究竟哪些是核心？总结下来就两句话

- 导入对应技术的starter坐标
- 根据对应技术的要求做配置

    <hr>

接下来在MyBatis的基础上再升级一下，整合MyBaitsPlus（简称MP），国人开发的技术，符合中国人开发习惯。

**步骤①**：导入对应的starter

```XML
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
```

​	关于这个坐标，此处要说明一点，之前我们看的starter都是spring-boot-starter-？？？，也就是说都是下面的格式

```tex
Spring-boot-start-***
```

​	而这个坐标的名字书写比较特殊，是第三方技术名称在前，boot和starter在后。此处简单提一下命名规范，后期原理篇会再详细讲解

| starter所属 | 命名规则                                                    | 示例                                                  |
| ----------- | ----------------------------------------------------------- | ----------------------------------------------------- |
| 官方提供    | spring-boot-starter-技术名称                                | spring-boot-starter-web <br/>spring-boot-starter-test |
| 第三方提供  | 第三方技术名称-spring-boot-starter                          | druid-spring-boot-starter                             |
| 第三方提供  | 第三方技术名称-boot-starter（第三方技术名称过长，简化命名） | mybatis-plus-boot-starter                             |

<font color="#f0f"><b>温馨提示</b></font>

​	在创建项目时想通过勾选的形式找到这个名字，没有。截止目前，SpringBoot官网还未收录此坐标，而我们Idea创建模块时读取的是SpringBoot官网的Spring Initializr，所以也没有。如果换用阿里云的url创建项目可以找到对应的坐标

**步骤②**：配置数据源相关信息

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

​	

剩下的就是写MyBaitsPlus的程序了

**映射接口（Dao）**

```JAVA
@Mapper
public interface BookDao extends BaseMapper<Book> {
}
```

​	核心在于Dao接口继承了一个BaseMapper的接口，这个接口中帮助开发者预定了若干个常用的API接口，简化了通用API接口的开发工作。

​	下面就可以写一个测试类进行测试了，此处省略。

<font color="#f0f"><b>温馨提示</b></font>

​	目前数据库的表名定义规则是tbl_模块名称，为了能和实体类相对应，需要做一个配置，相关知识各位小伙伴可以到MyBatisPlus课程中去学习，此处仅给出解决方案。配置application.yml文件，添加如下配置即可，设置所有表名的通用前缀名

```yaml
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_		#设置所有表的通用前缀名称为tbl_
```

**总结**

1. 手工添加MyBatis-Plus对应的starter
2. 数据层接口使用BaseMapper简化开发
3. 需要使用的第三方技术无法通过勾选确定时，需要手工添加坐标



### 四、整合Druid

​	前面整合MyBatis和MP的时候，使用的数据源对象都是SpringBoot默认的数据源对象，下面我们手工控制一下，自己指定了一个数据源对象，Druid。

​	在没有指定数据源时，我们的配置如下：

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root
```

​	此时虽然没有指定数据源，但是SpringBoot帮我们选了一个它认为最好的数据源对象，这就是HiKari。通过启动日志可以查看到对应的身影。

```tex
2021-11-29 09:39:15.202  INFO 12260 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2021-11-29 09:39:15.208  WARN 12260 --- [           main] com.zaxxer.hikari.util.DriverDataSource  : Registered driver with driverClassName=com.mysql.jdbc.Driver was not found, trying direct instantiation.
2021-11-29 09:39:15.551  INFO 12260 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
```

​	上述信息中每一行都有HiKari的身影，如果需要更换数据源，其实只需要两步即可。

1. 导入对应的技术坐标
2. 配置使用指定的数据源类型

​    下面就切换一下数据源对象

**步骤①**：导入对应的坐标（注意，是坐标，此处不是starter）

```XML
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>
</dependencies>
```

**步骤②**：修改配置，在数据源配置中有一个type属性，专用于指定数据源类型

```YAML
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
```

​	这里其实要提出一个问题的，目前的数据源配置格式是一个通用格式，不管你换什么数据源都可以用这种形式进行配置。但是新的问题又来了，如果对数据源进行个性化的配置，例如配置数据源对应的连接数量，这个时候就有新的问题了。每个数据源技术对应的配置名称都一样吗？肯定不是啊，各个厂商不可能提前商量好都写一样的名字啊，怎么办？就要使用专用的配置格式了。这个时候上面这种通用格式就不能使用了，怎么办？还能怎么办？按照SpringBoot整合其他技术的通用规则来套啊，导入对应的starter，进行相应的配置即可。

**步骤①**：导入对应的starter

```XML
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
</dependencies>
```



**步骤②**：修改配置

```YAML
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

​	注意观察，配置项中，在datasource下面并不是直接配置url这些属性的，而是先配置了一个druid节点，然后再配置的url这些东西。言外之意，url这个属性时druid下面的属性，那你能想到吗？除了这4个常规配置外，还有druid专用的其他配置。通过提示功能可以打开druid相关的配置查阅

**总结**

1. 整合Druid需要导入Druid对应的starter
2. 根据Druid提供的配置方式进行配置
3. 整合第三方技术通用方式
   - 导入对应的starter
   - 根据提供的配置格式，配置非默认值对应的配置项



### 五、SSMP整合综合案例

整体案例中需要采用的技术如下

1. 实体类开发————使用Lombok快速制作实体类
2. Dao开发————整合MyBatisPlus，制作数据层测试
3. Service开发————基于MyBatisPlus进行增量开发，制作业务层测试类
4. Controller开发————基于Restful开发，使用PostMan测试接口功能
5. Controller开发————前后端开发协议制作
6. 页面开发————基于VUE+ElementUI制作，前后端联调，页面数据处理，页面消息处理
   - 列表
   - 新增
   - 修改
   - 删除
   - 分页
   - 查询
7. 项目异常处理
8. 按条件查询————页面功能调整、Controller修正功能、Service修正功能



#### 0. 模块创建

后台做单体服务器，前端不使用前后端分离的制作。

一个服务器即充当后台服务调用，又负责前端页面展示，降低学习的门槛。



​		下面创建一个新的模块，加载要使用的技术对应的starter，修改配置文件格式为yml格式，并把web访问端口先设置成80。

**pom.xml**

```XML
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.6</version>
</dependency>
```

**application.yml**

```yaml
server:
  port: 80
```



#### 1. 实体类开发

本案例对应的模块表结构如下：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220321185212054.png)

根据上述表结构，制作对应的实体类



**实体类**

​		Lombok，一个Java类库，提供了一组注解，简化POJO实体类开发，SpringBoot目前默认集成了lombok技术，并提供了对应的版本控制，所以只需要提供对应的坐标即可，在pom.xml中添加lombok的坐标。

```XML
<dependencies>
    <!--lombok-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
```

​		使用lombok可以通过一个注解@Data完成一个实体类对应的getter，setter，toString，equals，hashCode等操作的快速添加

```JAVA
import lombok.Data;
@Data
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```



#### 2. 数据层开发——基础CRUD

​		数据层开发本次使用MyBatisPlus，数据源使用Druid。

##### 2.1 步骤

**步骤①**：导入MyBatisPlus与Druid对应的starter，当然mysql的驱动不能少

```xml
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.3</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

**步骤②**：配置数据库连接相关的数据源配置

```YAML
server:
  port: 80

spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC&characterEncoding=UTF-8
      username: root
      password: 123456
```

**步骤③**：使用MyBatisPlus的标准通用接口BaseMapper加速开发，别忘了@Mapper和泛型的指定

```JAVA
@Mapper
public interface BookDao extends BaseMapper<Book> {
}
```

**步骤④**：制作测试类测试结果

```JAVA
@SpringBootTest
public class BookDaoTestCase {

    @Autowired
    private BookDao bookDao;

    @Test
    void testGetById(){
        System.out.println(bookDao.selectById(1));
    }

    @Test
    void testSave(){
        Book book = new Book();
        book.setType("测试数据123");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookDao.insert(book);
    }

    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(17);
        book.setType("测试数据abcdefg");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookDao.updateById(book);
    }

    @Test
    void testDelete(){
        bookDao.deleteById(16);
    }

    @Test
    void testGetAll(){
        bookDao.selectList(null);
    }
}
```

<font color="#f0f"><b>温馨提示</b></font>

​		MyBatisPlus技术默认的主键生成策略为雪花算法，生成的主键ID长度较大，和目前的数据库设定规则不相符，需要配置一下使MyBatisPlus使用数据库的主键生成策略，方式嘛还是老一套，做配置。在application.yml中添加对应配置即可，具体如下

```yaml
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_		#设置表名通用前缀
      id-type: auto				#设置主键id字段的生成策略为参照数据库设定的策略，当前数据库设置id生成策略为自增
```



##### 2.2 查看MyBatisPlus运行日志

SpringBoot整合MyBatisPlus的时候，通过配置的形式就可以查阅执行期SQL语句，配置如下

```YAML
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_
      id-type: auto
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

​		运行结果就显示了运行期执行SQL的情况。



**总结**

1. 手工导入starter坐标（2个），mysql驱动（1个）

2. 配置数据源与MyBatisPlus对应的配置

3. 开发Dao接口（继承BaseMapper）

4. 制作测试类测试Dao功能是否有效

5. 使用配置方式开启日志，设置日志输出方式为标准输出即可查阅SQL执行日志



#### 3. 数据层开发——分页功能制作

前面仅仅是使用了MyBatisPlus提供的基础CRUD功能，实际上MyBatisPlus提供了几乎所有的基础操作。                                                                                                                                                                                                                MyBatisPlus提供的分页操作API如下：

```JAVA
@Test
void testGetPage(){
    IPage page = new Page(2,5);  //第2页的5条数据
    bookDao.selectPage(page, null);
    
    System.out.println(page.getCurrent());
    System.out.println(page.getSize());
    System.out.println(page.getTotal());
    System.out.println(page.getPages());
    System.out.println(page.getRecords());
}
```

​		其中selectPage方法需要传入一个封装分页数据的对象，可以通过new的形式创建这个对象，当然这个对象也是MyBatisPlus提供的，别选错包了。创建此对象时需要指定两个分页的基本数据

- 当前显示第几页
- 每页显示几条数据


​		可以通过创建Page对象时利用构造方法初始化这两个数据。

```JAVA
IPage page = new Page(2,5);
```

​		将该对象传入到查询方法selectPage后，可以得到查询结果，但是我们会发现当前操作查询结果返回值仍然是一个IPage对象，这又是怎么回事？

```JAVA
IPage page = bookDao.selectPage(page, null);
```

​		原来这个IPage对象中封装了若干个数据，而查询的结果作为IPage对象封装的一个数据存在的，可以理解为查询结果得到后，又塞到了这个IPage对象中，其实还是为了高度的封装，一个IPage描述了分页所有的信息。下面5个操作就是IPage对象中封装的所有信息。

```JAVA
@Test
void testGetPage(){
    IPage page = new Page(2,5);
    bookDao.selectPage(page, null);
    System.out.println(page.getCurrent());		//当前页码值
    System.out.println(page.getSize());			//每页显示数
    System.out.println(page.getTotal());		//数据总量
    System.out.println(page.getPages());		//总页数
    System.out.println(page.getRecords());		//详细数据
}
```

​		实际上这个分页功能当前是无效的。为什么呢？这要源于MyBatisPlus的内部机制。

​		对于MySQL的分页操作使用limit关键字进行，而并不是所有的数据库都使用limit关键字实现的，这个时候MyBatisPlus为了制作的兼容性强，将分页操作设置为基础查询操作的升级版。

​		基础操作中有查询全部的功能，而在这个基础上只需要升级一下就可以得到分页操作。MyBatisPlus将分页操作做成了一个开关，用分页功能就把开关开启，不用就不需要开启这个开关。而现在没有开启这个开关，所以分页操作是没有的。这个开关是通过MyBatisPlus的拦截器的形式存在的。具体设置方式如下：

**定义MyBatisPlus拦截器并将其设置为Spring管控的bean**

```JAVA
@Configuration
public class MPConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }
}
```

​		上述代码第一行是创建MyBatisPlus的拦截器栈，这个时候拦截器栈中没有具体的拦截器，第二行是初始化了分页拦截器，并添加到拦截器栈中。如果后期开发其他功能，需要添加全新的拦截器，按照第二行的格式继续add进去新的拦截器就可以了。

**总结**

1. 使用IPage封装分页数据
2. 分页操作依赖MyBatisPlus分页拦截器实现功能
3. 借助MyBatisPlus日志查阅执行SQL语句



#### 4. 数据层开发——条件查询功能制作

​		除了分页功能，MyBatisPlus还提供有强大的条件查询功能。以往写条件查询要自己动态拼写复杂的SQL语句，现在MyBatisPlus将这些操作都制作成API接口，调用方法就可以实现各种条件的拼装。

​		下面的操作是执行一个模糊匹配对应的操作，由like条件书写变为了like方法的调用。

```JAVA
@Test
void testGetBy(){
    QueryWrapper<Book> qw = new QueryWrapper<>();
    qw.like("name","Spring");
    bookDao.selectList(qw);
}
```

​		其中第一句QueryWrapper对象是一个用于封装查询条件的对象，该对象可以动态使用API调用的方法添加条件，最终转化成对应的SQL语句。第二句就是一个条件了，需要什么条件，使用QueryWapper对象直接调用对应操作即可。比如做大于小于关系，就可以使用lt或gt方法，等于使用eq方法。

​		这组API使用还是比较简单的，但是关于属性字段名的书写存在着安全隐患，比如查询字段name，当前是以字符串的形态书写的，万一写错，编译器还没有办法发现，只能将问题抛到运行器通过异常堆栈告诉开发者，不太友好。

​		MyBatisPlus针对字段检查进行了功能升级，全面支持Lambda表达式，就有了下面这组API。由QueryWrapper对象升级为LambdaQueryWrapper对象，这下就避免了上述问题的出现。

```JAVA
@Test
void testGetBy2(){
    String name = "1";
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
    lqw.like(Book::getName,name);
    bookDao.selectList(lqw);
}
```

​		

为了便于开发者动态拼写SQL，防止将null数据作为条件使用，MyBatisPlus还提供了动态拼装SQL的快捷书写方式。

```JAVA
@Test
void testGetBy2(){
    String name = "1";
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
    //if(name != null) lqw.like(Book::getName,name);		//方式一：JAVA代码控制
    lqw.like(name != null,Book::getName,name);				//方式二：API接口提供控制开关
    bookDao.selectList(lqw);
}
```

​		

**总结**

1. 使用QueryWrapper对象封装查询条件

2. 推荐使用LambdaQueryWrapper对象

3. 所有查询操作封装成方法调用

4. 查询条件支持动态条件拼装




#### 5. 业务层开发

> 业务层开发，是<font color="#ff0000"><b>组织业务逻辑功能，并根据业务需求，对数据持久层发起调用</b></font>。目标是为了组织出符合需求的业务逻辑功能，至于调不调用数据层，有需求就调用，没有需求就不调用。

​		业务层的方法名定义一定要与业务有关，例如登录操作

```JAVA
login(String username,String password);
```

​		而数据层的方法名定义一定与业务无关，是一定，不是可能，也不是有可能，例如根据用户名密码查询

```JAVA
selectByUserNameAndPassword(String username,String password);
```

​		我们在开发的时候是可以根据完成的工作不同划分成不同职能的开发团队的。比如制作数据层，他就可以不知道业务是什么样子，拿到的需求文档要求可能是这样的

```tex
接口：传入用户名与密码字段，查询出对应结果，结果是单条数据
接口：传入ID字段，查询出对应结果，结果是单条数据
接口：传入离职字段，查询出对应结果，结果是多条数据
```

​		但是进行业务功能开发，拿到的需求文档要求差别就很大

```tex
接口：传入用户名与密码字段，对用户名字段做长度校验，4-15位，对密码字段做长度校验，8到24位，对密码字段做特殊字符校验，不允许存在空格，查询结果为对象。如果为null，返回BusinessException，封装消息码INFO_LOGON_USERNAME_PASSWORD_ERROR
```

​		

业务层接口定义如下：

```JAVA
public interface BookService {
    Boolean save(Book book);
    Boolean update(Book book);
    Boolean delete(Integer id);
    Book getById(Integer id);
    List<Book> getAll();
    IPage<Book> getPage(int currentPage,int pageSize);
}
```

业务层实现类如下，转调数据层即可：

```JAVA
@Service
public class BookServiceImpl implements BookService {

    @Autowired
    private BookDao bookDao;

    @Override
    public Boolean save(Book book) {
        return bookDao.insert(book) > 0;
    }

    @Override
    public Boolean update(Book book) {
        return bookDao.updateById(book) > 0;
    }

    @Override
    public Boolean delete(Integer id) {
        return bookDao.deleteById(id) > 0;
    }

    @Override
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }

    @Override
    public List<Book> getAll() {
        return bookDao.selectList(null);
    }

    @Override
    public IPage<Book> getPage(int currentPage, int pageSize) {
        IPage page = new Page(currentPage,pageSize);
        bookDao.selectPage(page,null);
        return page;
    }
}
```

​		别忘了对业务层接口进行测试，测试类如下：

```JAVA
@SpringBootTest
public class BookServiceTest {
    @Autowired
    private BookService bookService;

    @Test
    void testGetById(){
        System.out.println(bookService.getById(4));
    }
    
    @Test
    void testSave(){
        Book book = new Book();
        book.setType("测试数据123");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookService.save(book);
    }
    
    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(17);
        book.setType("-----------------");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookService.update(book);
    }
    @Test
    void testDelete(){
        bookService.delete(18);
    }

    @Test
    void testGetAll(){
        bookService.getAll();
    }

    @Test
    void testGetPage(){
        IPage<Book> page = bookService.getPage(2, 5);
        System.out.println(page.getCurrent());
        System.out.println(page.getSize());
        System.out.println(page.getTotal());
        System.out.println(page.getPages());
        System.out.println(page.getRecords());
    }
}
```

**总结**

1. Service接口名称定义成业务名称，并与Dao接口名称进行区分
2. 制作测试类测试Service功能是否有效

<hr>

**业务层快速开发**

​		其实MyBatisPlus技术不仅提供了数据层快速开发方案，业务层MyBatisPlus也给了一个通用接口，其实就是一个封装+继承的思想，代码给出，实际开发慎用。

​		业务层接口快速开发

```JAVA
public interface IBookService extends IService<Book> {
    //添加非通用操作API接口
}
```

​		业务层接口实现类快速开发，关注继承的类需要传入两个泛型，一个是数据层接口，另一个是实体类。

```JAVA
@Service
public class BookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {
    @Autowired
    private BookDao bookDao;
	//添加非通用操作API
}
```

​		如果感觉MyBatisPlus提供的功能不足以支撑你的使用需要（其实是一定不能支撑的，因为需求不可能是通用的），在原始接口基础上接着定义新的API接口就行了，但是不要和已有的API接口名冲突即可。



**总结**

1. 使用通用接口（ISerivce<T>）快速开发Service
2. 使用通用实现类（ServiceImpl<M,T>）快速开发ServiceImpl
3. 可以在通用接口基础上做功能重载或功能追加
4. 注意重载时不要覆盖原始操作，避免原始提供的功能丢失



#### 6. 表现层开发

表现层的开发使用基于Restful的表现层接口开发，功能测试通过Postman工具进行。

​		表现层接口如下:

```JAVA
@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private IBookService bookService;

    @GetMapping
    public List<Book> getAll(){
        return bookService.list();
    }

    @PostMapping
    public Boolean save(@RequestBody Book book){
        return bookService.save(book);
    }

    @PutMapping
    public Boolean update(@RequestBody Book book){
        return bookService.modify(book);
    }

    @DeleteMapping("{id}")
    public Boolean delete(@PathVariable Integer id){
        return bookService.delete(id);
    }

    @GetMapping("{id}")
    public Book getById(@PathVariable Integer id){
        return bookService.getById(id);
    }

    @GetMapping("{currentPage}/{pageSize}")
    public IPage<Book> getPage(@PathVariable int currentPage,@PathVariable int pageSize){
        return bookService.getPage(currentPage,pageSize);
    }
}
```

​		在使用Postman测试时关注提交类型，对应上即可，不然就会报405的错误码了。

**普通GET请求**

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220322102022480.png)



**PUT请求传递json数据，后台实用@RequestBody接收数据**

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220322102202637.png)



**GET请求传递路径变量，后台实用@PathVariable接收数据**

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220322102309014.png)



**总结**

1. 基于Restful制作表现层接口
   - 新增：POST
   - 删除：DELETE
   - 修改：PUT
   - 查询：GET
2. 接收参数
   - 实体数据：@RequestBody
   - 路径变量：@PathVariable



#### 7. 表现层消息一致性处理

​		目前我们通过Postman测试后业务层接口功能是通的，但是这样的结果给到前端开发者会出现一个小问题。不同的操作结果所展示的数据格式差异化严重。

​	**增删改操作结果**

```tex
true
```

​	**查询单个数据操作结果**

```json
{
    "id": 1,
    "type": "计算机理论",
    "name": "Spring实战 第5版",
    "description": "Spring入门经典教程"
}
```

​	**查询全部数据操作结果**

```json
[
    {
        "id": 1,
        "type": "计算机理论",
        "name": "Spring实战 第5版",
        "description": "Spring入门经典教程"
    },
    {
        "id": 2,
        "type": "计算机理论",
        "name": "Spring 5核心原理与30个类手写实战",
        "description": "十年沉淀之作"
    }
]
```

​		

每种不同操作返回的数据格式都不一样，而且还不知道以后还会有什么格式，这样的结果让前端人员看了是很容易崩溃的，必须将所有操作的操作结果数据格式统一起来，需要设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为**前后端数据协议**

```JAVA
@Data
public class R {
    private Boolean flag;
    private Object data;

    public R() {
    }

    public R(Boolean flag) {
        this.flag = flag;
    }

    public R(Boolean flag, Object data) {
        this.flag = flag;
        this.data = data;
    }
}
```

​		其中flag用于标识操作是否成功，data用于封装操作数据，现在的数据格式就变了

```JSON
{
    "flag": true,
    "data":{
        "id": 1,
        "type": "计算机理论",
        "name": "Spring实战 第5版",
        "description": "Spring入门经典教程"
    }
}
```

​		表现层开发格式也需要转换一下

```java
@RestController
@RequestMapping("/books")
public class BookController {
    @Autowired
    private BookService bookService;

    @GetMapping
    public R getAll() {
        return new R(true,bookService.list());
    }

    @PostMapping
    public R save(@RequestBody Book book) {
        return new R(bookService.save(book));
    }

    @PutMapping
    public R update(@RequestBody Book book){
        return new R(bookService.update(book));
    }

    @DeleteMapping("{id}")
    public R delete(@PathVariable Integer id){
        return new R(bookService.delete(id));
    }

    @GetMapping("{id}")
    public R getById(@PathVariable Integer id){
        return new R(true,bookService.getById(id));
    }

    @GetMapping("{currentPage}/{pageSize}")
    public R getPage(@PathVariable int currentPage, @PathVariable int pageSize){
        return new R(true,bookService.getPage(currentPage,pageSize));
    }
}
```

​		现在后端发送给前端的数据格式就统一了，免去了不少前端解析数据的烦恼。



**总结**

1. 设计统一的返回值结果类型便于前端开发读取数据

2. 返回值结果类型可以根据需求自行设定，没有固定格式

3. 返回值结果模型类用于后端与前端进行数据格式统一，也称为前后端数据协议

   

#### 8. 前后端连通性测试

后端的表现层接口开发完毕，就可以进行前端的开发了。

将前端人员开发的页面保存到lresources目录下的static目录中，建议执行maven的clean生命周期，避免缓存的问题出现。

​	![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220322113131505.png)



在进行具体的功能开发之前，先做联通性的测试，通过页面发送异步提交（axios），这一步调试通过后再进行进一步的功能开发。



```js
 created() {
     this.getAll();
 },
    
//列表
getAll() {
	axios.get("/books").then((res)=>{
		console.log(res.data);
	});
},
```

​		只要后台代码能够正常工作，前端能够在日志中接收到数据，就证明前后端是通的，也就可以进行下一步的功能开发了。

**总结**

1. 单体项目中页面放置在resources/static目录下
2. created钩子函数用于初始化页面时发起调用
3. 页面使用axios发送异步请求获取数据后确认前后端是否联通



#### 9. 页面基础功能开发

##### 	F-1.列表功能（非分页版）

​		列表功能主要操作就是加载完数据，将数据展示到页面上，此处要利用VUE的数据模型绑定，发送请求得到数据，然后页面上读取指定数据即可。

​		**页面数据模型定义**

```js
data:{
	dataList: [],		//当前页要展示的列表数据
	...
},
```

​		异步请求获取数据

```JS
//列表
getAll() {
    axios.get("/books").then((res)=>{
        this.dataList = res.data.data;
    });
},
```

​		这样在页面加载时就可以获取到数据，并且由VUE将数据展示到页面上了。

总结：

1. 将查询数据返回到页面，利用前端数据绑定进行数据展示



##### 	F-2.添加功能

​		添加功能用于收集数据的表单是通过一个弹窗展示的，因此在添加操作前首先要进行弹窗的展示，添加后隐藏弹窗即可。因为这个弹窗一直存在，因此当页面加载时首先设置这个弹窗为不可显示状态，需要展示，切换状态即可。

​		**默认状态**

```js
data:{
	dialogFormVisible: false,	//添加表单是否可见
	...
},
```

​		**切换为显示状态**

```JS
//弹出添加窗口
handleCreate() {
	this.dialogFormVisible = true;
},
```

​		由于每次添加数据都是使用同一个弹窗录入数据，所以每次操作的痕迹将在下一次操作时展示出来，需要在每次操作之前清理掉上次操作的痕迹。

​		**定义清理数据操作**

```js
//重置表单
resetForm() {
    this.formData = {};
},
```

​		**切换弹窗状态时清理数据**

```js
//弹出添加窗口
handleCreate() {
    this.dialogFormVisible = true;
    this.resetForm();
},
```

​		至此准备工作完成，下面就要调用后台完成添加操作了。

​		**添加操作**

```js
//添加
handleAdd () {
    //发送异步请求
    axios.post("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层，显示数据
        if(res.data.flag){
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else {
            this.$message.error("添加失败");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

1. 将要保存的数据传递到后台，通过post请求的第二个参数传递json数据到后台
2. 根据返回的操作结果决定下一步操作
   - 如何是true就关闭添加窗口，显示添加成功的消息
   - 如果是false保留添加窗口，显示添加失败的消息
3. 无论添加是否成功，页面均进行刷新，动态加载数据（对getAll操作发起调用）

​		**取消添加操作**

```JS
//取消
cancel(){
    this.dialogFormVisible = false;
    this.$message.info("操作取消");
},
```

**总结**

1. 请求方式使用POST调用后台对应操作
2. 添加操作结束后动态刷新页面加载数据
3. 根据操作结果不同，显示对应的提示信息
4. 弹出添加Div时清除表单数据



##### 	F-3.删除功能

​		模仿添加操作制作删除功能，差别之处在于删除操作仅传递一个待删除的数据id到后台即可。

​		**删除操作**

```JS
// 删除
handleDelete(row) {
    axios.delete("/books/"+row.id).then((res)=>{
        if(res.data.flag){
            this.$message.success("删除成功");
        }else{
            this.$message.error("删除失败");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

​		**删除操作提示信息**

```JS
// 删除
handleDelete(row) {
    //1.弹出提示框
    this.$confirm("此操作永久删除当前数据，是否继续？","提示",{
        type:'info'
    }).then(()=>{
        //2.做删除业务
        axios.delete("/books/"+row.id).then((res)=>{
       		if(res.data.flag){
            	this.$message.success("删除成功");
        	}else{
            	this.$message.error("删除失败");
        	}
        }).finally(()=>{
            this.getAll();
        });
    }).catch(()=>{
        //3.取消删除
        this.$message.info("取消删除操作");
    });
}，	
```

**总结**

1. 请求方式使用Delete调用后台对应操作
2. 删除操作需要传递当前行数据对应的id值到后台
3. 删除操作结束后动态刷新页面加载数据
4. 根据操作结果不同，显示对应的提示信息
5. 删除操作前弹出提示框避免误操作



##### 	F-4.修改功能

​		修改功能可以说是列表功能、删除功能与添加功能的合体。几个相似点如下：

1. 页面也需要有一个弹窗用来加载修改的数据，这一点与添加相同，都是要弹窗

2. 弹出窗口中要加载待修改的数据，而数据需要通过查询得到，这一点与查询全部相同，都是要查数据

3. 查询操作需要将要修改的数据id发送到后台，这一点与删除相同，都是传递id到后台

4. 查询得到数据后需要展示到弹窗中，这一点与查询全部相同，都是要通过数据模型绑定展示数据

5. 修改数据时需要将被修改的数据传递到后台，这一点与添加相同，都是要传递数据

   所以整体上来看，修改功能就是前面几个功能的大合体

   **查询并展示数据**

```JS
//弹出编辑窗口
handleUpdate(row) {
    axios.get("/books/"+row.id).then((res)=>{
        if(res.data.flag){
            //展示弹层，加载数据
            this.formData = res.data.data;
            this.dialogFormVisible4Edit = true;
        }else{
            this.$message.error("数据同步失败，自动刷新");
        }
    });
},
```

​		**修改操作**

```JS
//修改
handleEdit() {
    axios.put("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层并刷新页面
        if(res.data.flag){
            this.dialogFormVisible4Edit = false;
            this.$message.success("修改成功");
        }else {
            this.$message.error("修改失败，请重试");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

**总结**

1. 加载要修改数据通过传递当前行数据对应的id值到后台查询数据（同删除与查询全部）
2. 利用前端双向数据绑定将查询到的数据进行回显（同查询全部）
3. 请求方式使用PUT调用后台对应操作（同新增传递数据）
4. 修改操作结束后动态刷新页面加载数据（同新增）
5. 根据操作结果不同，显示对应的提示信息（同新增）

#### 	10. 业务消息一致性处理

​		目前的功能制作基本上达成了正常使用的情况，什么叫正常使用呢？也就是这个程序不出BUG，如果我们搞一个BUG出来，你会发现程序马上崩溃掉。比如后台手工抛出一个异常，看看前端接收到的数据什么样子。

```json
{
    "timestamp": "2021-09-15T03:27:31.038+00:00",
    "status": 500,
    "error": "Internal Server Error",
    "path": "/books"
}
```

​		面对这种情况，前端的同学又不会了，这又是什么格式？怎么和之前的格式不一样？

```json
{
    "flag": true,
    "data":{
        "id": 1,
        "type": "计算机理论",
        "name": "Spring实战 第5版",
        "description": "Spring入门经典教程"
    }
}
```

​		看来不仅要对正确的操作数据格式做处理，还要对错误的操作数据格式做同样的格式处理。

​		首先在当前的数据结果中添加消息字段，用来兼容后台出现的操作消息。

```JAVA
@Data
public class R{
    private Boolean flag;
    private Object data;
    private String msg;		//用于封装消息
}
```

​		后台代码也要根据情况做处理，当前是模拟的错误。

```JAVA
@PostMapping
public R save(@RequestBody Book book) throws IOException {
    Boolean flag = bookService.insert(book);
    return new R(flag , flag ? "添加成功^_^" : "添加失败-_-!");
}
```

​		然后在表现层做统一的异常处理，使用SpringMVC提供的异常处理器做统一的异常处理。

```JAVA
@RestControllerAdvice
public class ProjectExceptionAdvice {
    @ExceptionHandler(Exception.class)
    public R doOtherException(Exception ex){
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员,ex对象发送给开发人员
        ex.printStackTrace();
        return new R(false,null,"系统错误，请稍后再试！");
    }
}
```

​		页面上得到数据后，先判定是否有后台传递过来的消息，标志就是当前操作是否成功，如果返回操作结果false，就读取后台传递的消息。

```JS
//添加
handleAdd () {
	//发送ajax请求
    axios.post("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层，显示数据
        if(res.data.flag){
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else {
            this.$message.error(res.data.msg);			//消息来自于后台传递过来，而非固定内容
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

**总结**

1. 使用注解@RestControllerAdvice定义SpringMVC异常处理器用来处理异常的
2. 异常处理器必须被扫描加载，否则无法生效
3. 表现层返回结果的模型类中添加消息属性用来传递消息到页面

​	

#### 11. 页面功能开发

##### 	F-5.分页功能

​		分页功能的制作用于替换前面的查询全部，其中要使用到elementUI提供的分页组件。

```js
<!--分页组件-->
<div class="pagination-container">
    <el-pagination
		class="pagiantion"
		@current-change="handleCurrentChange"
		:current-page="pagination.currentPage"
		:page-size="pagination.pageSize"
		layout="total, prev, pager, next, jumper"
		:total="pagination.total">
    </el-pagination>
</div>
```

​		为了配合分页组件，封装分页对应的数据模型。

```js
data:{
	pagination: {	
		//分页相关模型数据
		currentPage: 1,	//当前页码
		pageSize:10,	//每页显示的记录数
		total:0,		//总记录数
	}
},
```

​		修改查询全部功能为分页查询，通过路径变量传递页码信息参数。

```JS
getAll() {
    axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize).then((res) => {
    });
},
```

​		后台提供对应的分页功能。

```JAVA
@GetMapping("/{currentPage}/{pageSize}")
public R getAll(@PathVariable Integer currentPage,@PathVariable Integer pageSize){
    IPage<Book> pageBook = bookService.getPage(currentPage, pageSize);
    return new R(null != pageBook ,pageBook);
}
```

​		页面根据分页操作结果读取对应数据，并进行数据模型绑定。

```JS
getAll() {
    axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize).then((res) => {
        this.pagination.total = res.data.data.total;
        this.pagination.currentPage = res.data.data.current;
        this.pagination.pagesize = res.data.data.size;
        this.dataList = res.data.data.records;
    });
},
```

​		对切换页码操作设置调用当前分页操作。

```JS
//切换页码
handleCurrentChange(currentPage) {
    this.pagination.currentPage = currentPage;
    this.getAll();
},
```

**总结**

1. 使用el分页组件
2. 定义分页组件绑定的数据模型
3. 异步调用获取分页数据
4. 分页数据页面回显



##### 	F-6.删除功能维护

​		由于使用了分页功能，当最后一页只有一条数据时，删除操作就会出现BUG，最后一页无数据但是独立展示，对分页查询功能进行后台功能维护，如果当前页码值大于最大页码值，重新执行查询。其实这个问题解决方案很多，这里给出比较简单的一种处理方案。

```JAVA
@GetMapping("{currentPage}/{pageSize}")
public R getPage(@PathVariable int currentPage,@PathVariable int pageSize){
    IPage<Book> page = bookService.getPage(currentPage, pageSize);
    //如果当前页码值大于了总页码值，那么重新执行查询操作，使用最大页码值作为当前页码值
    if( currentPage > page.getPages()){
        page = bookService.getPage((int)page.getPages(), pageSize);
    }
    return new R(true, page);
}
```



##### 	F-7.条件查询功能

​		最后一个功能来做条件查询，其实条件查询可以理解为分页查询的时候除了携带分页数据再多带几个数据的查询。这些多带的数据就是查询条件。比较一下不带条件的分页查询与带条件的分页查询差别之处，这个功能就好做了

- 页面封装的数据：带不带条件影响的仅仅是一次性传递到后台的数据总量，由传递2个分页相关数据转换成2个分页数据加若干个条件

- 后台查询功能：查询时由不带条件，转换成带条件，反正不带条件的时候查询条件对象使用的是null，现在换成具体条件，差别不大

- 查询结果：不管带不带条件，出来的数据只是有数量上的差别，其他都差别，这个可以忽略

  经过上述分析，看来需要在页面发送请求的格式方面做一定的修改，后台的调用数据层操作时发送修改，其他没有区别。

  页面发送请求时，两个分页数据仍然使用路径变量，其他条件采用动态拼装url参数的形式传递。

  **页面封装查询条件字段**

  ```vue
  pagination: {		
  //分页相关模型数据
  	currentPage: 1,		//当前页码
  	pageSize:10,		//每页显示的记录数
  	total:0,			//总记录数
  	name: "",
  	type: "",
  	description: ""
  },
  ```

  页面添加查询条件字段对应的数据模型绑定名称

  ```HTML
  <div class="filter-container">
      <el-input placeholder="图书类别" v-model="pagination.type" class="filter-item"/>
      <el-input placeholder="图书名称" v-model="pagination.name" class="filter-item"/>
      <el-input placeholder="图书描述" v-model="pagination.description" class="filter-item"/>
      <el-button @click="getAll()" class="dalfBut">查询</el-button>
      <el-button type="primary" class="butT" @click="handleCreate()">新建</el-button>
  </div>
  ```

  将查询条件组织成url参数，添加到请求url地址中，这里可以借助其他类库快速开发，当前使用手工形式拼接，降低学习要求

  ```JS
  getAll() {
      //1.获取查询条件,拼接查询条件
      param = "?name="+this.pagination.name;
      param += "&type="+this.pagination.type;
      param += "&description="+this.pagination.description;
      console.log("-----------------"+ param);
      axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize+param).then((res) => {
          this.dataList = res.data.data.records;
      });
  },
  ```

  后台代码中定义实体类封查询条件

  ```JAVA
  @GetMapping("{currentPage}/{pageSize}")
  public R getAll(@PathVariable int currentPage,@PathVariable int pageSize,Book book) {
      System.out.println("参数=====>"+book);
      IPage<Book> pageBook = bookService.getPage(currentPage,pageSize);
      return new R(null != pageBook ,pageBook);
  }
  ```

  对应业务层接口与实现类进行修正

  ```JAVA
  public interface IBookService extends IService<Book> {
      IPage<Book> getPage(Integer currentPage,Integer pageSize,Book queryBook);
  }
  ```

  ```JAVA
  @Service
  public class BookServiceImpl2 extends ServiceImpl<BookDao,Book> implements IBookService {
      public IPage<Book> getPage(Integer currentPage,Integer pageSize,Book queryBook){
          IPage page = new Page(currentPage,pageSize);
          LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
          lqw.like(Strings.isNotEmpty(queryBook.getName()),Book::getName,queryBook.getName());
          lqw.like(Strings.isNotEmpty(queryBook.getType()),Book::getType,queryBook.getType());
          lqw.like(Strings.isNotEmpty(queryBook.getDescription()),Book::getDescription,queryBook.getDescription());
          return bookDao.selectPage(page,lqw);
      }
  }
  ```

  页面回显数据

  ```js
  getAll() {
      //1.获取查询条件,拼接查询条件
      param = "?name="+this.pagination.name;
      param += "&type="+this.pagination.type;
      param += "&description="+this.pagination.description;
      console.log("-----------------"+ param);
      axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize+param).then((res) => {
          this.pagination.total = res.data.data.total;
          this.pagination.currentPage = res.data.data.current;
          this.pagination.pagesize = res.data.data.size;
          this.dataList = res.data.data.records;
      });
  },
  ```

**总结**

1. 定义查询条件数据模型（当前封装到分页数据模型中）
2. 异步调用分页功能并通过请求参数传递数据到后台





# SpringBoot运维实用篇

## 第1章 打包与运行

### 一、程序打包

> SpringBoot程序是基于Maven创建的，在Maven中提供有打包的指令，叫做package。本操作可以在Idea环境下执行。

```java
mvn package
```

打包后会产生一个与工程名类似的jar文件，其名称是由模块名+版本号+.jar组成的。



### 二、程序运行

程序包打好以后，就可以直接执行了。在程序包所在路径下，执行指令。

```JAVA
java -jar 工程包名.jar
```

执行程序打包指令后，程序正常运行，与在Idea下执行程序没有区别。



<font color="#ff0000">特别关注</font>

- 如果你的计算机中没有安装java的jdk环境，是无法正确执行上述操作的，因为程序执行使用的是java指令。
- 在使用向导创建SpringBoot工程时，pom.xml文件中会有如下配置，这一段配置千万不能删除，否则打包后无法正常执行程序。

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```



### 三、命令行启动常见问题及解决方案

在DOS环境下启动SpringBoot工程时，可能会遇到端口占用的问题。

```JAVA
# 查询端口
netstat -ano
# 查询指定端口
netstat -ano |findstr "端口号"
# 根据进程PID查询进程名称
tasklist |findstr "进程PID号"
# 根据PID杀死任务
taskkill /F /PID "进程PID号"
# 根据进程名称杀死任务
taskkill -f -t -im "进程名称"
```



## 第2章 配置高级

### 一、临时属性设置

#### 1. 带属性数启动SpringBoot

```JAVA
java –jar springboot.jar –-server.port=80
```

如果你发现要修改的属性不止一个，可以按照上述格式继续写，属性与属性之间使用空格分隔。

```JAVA
java –jar springboot.jar –-server.port=80 --logging.level.root=debug
```

这里的格式不是yaml中的书写格式，当属性存在多级名称时，中间使用点分隔，和properties文件中的属性格式完全相同。



#### 2. 属性加载优先级

[https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html)

在yaml中配置了user.name属性值，然后读取出来的时候居然不是自己的配置值，因为在系统属性中有一个属性叫做user.name，两个相互冲突了。而系统属性的加载优先顺序在上面这个列表中是5号，高于3号，所以SpringBoot最终会加载系统配置属性user.name。



#### 3. 开发环境中使用临时属性

打开SpringBoot引导类的运行界面，在里面找到配置项。其中Program arguments对应的位置就是添加临时属性的

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220302131909115.png)

运行main方法的时候，使用main方法的args参数，就是在上面这个位置添加的参数。



### 二、配置文件分类

SpringBoot提供了配置文件和临时属性的方式来对程序进行配置。

SpringBoot提供的4级配置文件：

- 类路径下配置文件（一直使用的是这个，也就是resources目录中的application.yml文件）
- 类路径下config目录下配置文件
- 程序包所在目录中配置文件
- 程序包所在目录中config目录下配置文件



上面4个文件的加载优先顺序为

1级：file ：config/application.yml **【最高】**

2级：file ：application.yml

3级：classpath：config/application.yml

4级：classpath：application.yml  **【最低】**

作用： 

- 1级与2级留做系统打包后设置通用属性，1级常用于运维经理进行线上整体项目部署方案调控

- 3级与4级用于系统开发阶段设置通用属性，3级常用于项目经理进行整体项目属性调控



**总结**

1. 配置文件分为4种

   - 项目类路径配置文件：服务于开发人员本机开发与测试
   - 项目类路径config目录中配置文件：服务于项目经理整体调控
   - 工程路径配置文件：服务于运维人员配置涉密线上环境
   - 工程路径config目录中配置文件：服务于运维经理整体调控

2. 多层级配置文件间的属性采用叠加并覆盖的形式作用于程序



### 三、自定义配置文件

​	之前咱们做配置使用的配置文件都是application.yml，其实这个文件也是可以改名字的，这样方便维护。比如我2020年4月1日搞活动，走了一组配置，2020年5月1日活动取消，恢复原始配置，这个时候只需要重新更换一下配置文件就可以了。但是你总不能在原始配置文件上修改吧，不然搞完活动以后，活动的配置就留不下来了，不利于维护。

​		自定义配置文件方式有如下两种：

**方式一：使用临时属性设置配置文件名，注意仅仅是名称，不要带扩展名**



**方式二：使用临时属性设置配置文件路径，这个是全路径名**



​		也可以设置加载多个配置文件



​		使用的属性一个是spring.config.name，另一个是spring.config.location，这个一定要区别清楚。

<font color="#f0f"><b>温馨提示</b></font>

​		我们现在研究的都是SpringBoot单体项目，就是单服务器版本。其实企业开发现在更多的是使用基于SpringCloud技术的多服务器项目。这种配置方式和我们现在学习的完全不一样，所有的服务器将不再设置自己的配置文件，而是通过配置中心获取配置，动态加载配置信息。为什么这样做？集中管理。这里不再说这些了，后面再讲这些东西。

**总结**

1. 配置文件可以修改名称，通过启动参数设定
2. 配置文件可以修改路径，通过启动参数设定
3. 微服务开发中配置文件通过配置中心进行设置



## 第3章 多环境开发





## 第4章 日志
