---
title: 05-SpringMVC
date: 2022-02-12 09:22:53
tags: [Java]
---

# 第1章 SpringMVC入门

## 一、概述

>**SpringMVC** 是一种基于 Java 的实现 **MVC 设计模型**的请求驱动类型的轻量级 **Web 框架**，属于SpringFrameWork的后续产品，已经融合在 Spring Web Flow 中。
>
>SpringMVC 已经成为目前最主流的MVC框架之一，并且随着Spring3.0 的发布，全面超越Struts2，成为最优秀的 MVC 框架。它通过一套注解，让一个简单的 Java 类成为处理请求的控制器，而无须实现任何接口。同时它还支持 **RESTful** 编程风格的请求。



## 二、开发步骤

### 1. 导入Spring和SpringMVC相关坐标

1.1 导入Spring和SpringMVC的坐标

```xml
<!--Spring坐标--> 
<dependency> 
    <groupId>org.springframework</groupId> 
    <artifactId>spring-context</artifactId>
    <version>5.0.5.RELEASE</version>
</dependency>

<!--SpringMVC坐标--> 
<dependency> 
    <groupId>org.springframework</groupId> 
    <artifactId>spring-webmvc</artifactId> 
    <version>5.0.5.RELEASE</version>
</dependency>
```



1.2 导入Servlet和Jsp的坐标

```xml
<!--Servlet坐标--> 
<dependency> 
    <groupId>javax.servlet</groupId> 
    <artifactId>servlet-api</artifactId> 
    <version>2.5</version>
</dependency>

<!--Jsp坐标--> 
<dependency> 
    <groupId>javax.servlet.jsp</groupId> 
    <artifactId>jsp-api</artifactId> 
    <version>2.0</version>
</dependency>
```



### 2. 配置SpringMVC核心控制器DispathcerServlet

> SpringMVC是Spring中的模块,它实现了MVC设计模式的web框架,首先用户发出请求,请求到达SpringMVC的前端控制器(DispatcherServlet),前端控制器根据用户的url请求处理器映射器查找匹配该url的handler,并返回一个执行链,前端控制器再请求处理器适配器调用相应的handler进行处理并返回给前端控制器一个modelAndView,前端控制器再请求视图解析器对返回的逻辑视图进行解析,最后前端控制器将返回的视图进行渲染并把数据装入到request域,返回给用户.
>
> DispatcherServlet作为springMVC的前端控制器,负责接收用户的请求并根据用户的请求返回相应的视图给用户

在web.xml配置SpringMVC的核心控制器

```xml
<!--配置SpringMVC的前端控制器-->
<servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <!--
        load-on-startup元素标记容器是否在启动的时候就加载这个servlet(实例化并调用其init()方法)。
        1)它的值必须是一个整数，表示servlet应该被载入的顺序
        2)当值为0或者大于0时，表示容器在应用启动时就加载并初始化这个servlet；
        3)当值小于0或者没有指定时，则表示容器在该servlet被选择时才会去加载。
        4)正数的值越小，该servlet的优先级越高，应用启动时就越先加载。
        5)当值相同时，容器就会自己选择顺序来加载。
        所以，取值1，2，3，4，5代表的是优先级，而非启动延迟时间。
    -->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```



### 3. 创建Controller类和视图页面

3.1 创建Controller和业务方法

```java
public class UserController {
     public String save() {
        System.out.println("Controller save running");
        return "success.jsp";  //要跳转的视图
    }
}
```

3.2 创建视图页面success.jsp

```jsp
<html>
<head>
    <title>Title</title>
</head>
<body>
	<h1>Success!</h1>
</body>
</html>
```



### 4. 使用注解配置Controller类中业务方法的映射地址

```java
@Controller
public class UserController {
    //SpringMVC中使用@RequestMapping来映射请求，也就是通过它来指定控制器可以处理哪些URL请求
    @RequestMapping("/quick")
    public String save() {
        System.out.println("Controller save running");
        return "success.jsp";  
    }
}
```



### 5. 配置SpringMVC核心文件 spring-mvc.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--Controller的组件扫描-->
    <context:component-scan base-package="com.lak.controller"/>
</beans>
```



### 6. 客户端发起请求测试

访问测试地址 `http://localhost:8080/quick `

控制台打印

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213125926639.png)

页面显示

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213125956003.png" style="zoom:50%;" />

<hr>

 **SpringMVC流程图示**

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213130055333.png)





# 第2章 SpringMVC组件解析

## 一、SpringMVC的执行流程

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213130357734.png)



① 用户发送请求至前端控制器DispatcherServlet。 

② DispatcherServlet收到请求调用HandlerMapping处理器映射器。

③ 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。 

④ DispatcherServlet调用HandlerAdapter处理器适配器。

⑤ HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。 

⑥ Controller执行完成返回ModelAndView。 

⑦ HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。 

⑧ DispatcherServlet将ModelAndView传给ViewReslover视图解析器。

⑨ ViewReslover解析后返回具体View。 

⑩ DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。DispatcherServlet响应用户。



## 二、SpringMVC组件解析

### 1. 组件解析

**1.1 前端控制器：DispatcherServlet**

用户请求到达前端控制器，它就相当于 MVC 模式中的C，DispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，DispatcherServlet 的存在降低了组件之间的耦合性。



**2. 处理器映射器：HandlerMapping**

HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。



**3. 处理器适配器：HandlerAdapter**

通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。



**4. 处理器：Handler**

它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由Handler 对具体的用户请求进行处理。



**5. 视图解析器：View Resolver**

View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名，即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。



**6. 视图：View**

SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。最常用的视图就是 jsp。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。



### 2. 注解解析

**2.1 @RequestMapping**

作用：用于建立请求 URL 和处理请求方法之间的对应关系

位置：

- 类上，请求URL 的第一级访问目录。此处不写的话，就相当于应用的根目录。

- 方法上，请求 URL 的第二级访问目录，与类上的使用@RequestMapping标注的一级目录一起组成访问虚拟路径。

属性：

- **value**：用于指定请求的URL。它和path属性的作用是一样的

- **method**：用于指定请求的方式

- **params**：用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的key和value必须和配置的一模一样
  - **params = {"accountName"}**，表示请求参数必须有accountName
  - **params = {"moeny!=100"}**，表示请求参数中money不能是100



**2.2 mvc命名空间引入**

`spring-mvc.xml`

命名空间：`xmlns:context="http://www.springframework.org/schema/context"`

​					`xmlns:mvc="http://www.springframework.org/schema/mvc"`

约束地址：`http://www.springframework.org/schema/context`

​					`http://www.springframework.org/schema/context/spring-context.xsd`

​					`http://www.springframework.org/schema/mvc`

​					`http://www.springframework.org/schema/mvc/spring-mvc.xsd`



**2.3 组件扫描**

`spring-mvc.xml`

SpringMVC基于Spring容器，所以在进行SpringMVC操作时，需要将Controller存储到Spring容器中，如果使用@Controller注解标注的话，就需要使用<context:component-scan base- package=“com.lak.controller"/>进行组件扫描。

```xml
<!--Controller的组件扫描-->
<context:component-scan base-package="com.lak.controller">
    <!--只扫描controller包下的annotation注解-->
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    <!--不扫描controller包下的annotation注解-->
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```



### 3. XML配置解析

**视图解析器**

SpringMVC有默认组件配置，默认组件都是**DispatcherServlet.properties**配置文件中配置的，该配置文件地址

org/springframework/web/servlet/DispatcherServlet.properties，该文件中配置了默认的视图解析器，如下：

```xml
org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver
```



翻看该解析器源码，可以看到该解析器的默认设置，如下：

```
REDIRECT_URL_PREFIX = "redirect:" --重定向前缀
FORWARD_URL_PREFIX = "forward:" --转发前缀（默认值）
prefix = ""; --视图名称前缀
suffix = ""; --视图名称后缀
```



我们可以通过属性注入的方式修改视图的的前后缀

```xml
<!-配置内部资源视图解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"> 
	<property name="prefix" value="/WEB-INF/views/"></property> 
	<property name="suffix" value=".jsp"></property>
</bean>
```



# 第3章 SpringMVC的请求和响应

## 一、数据响应方式

### 1. 页面跳转

#### 1.1 直接返回字符串

直接返回字符串：此种方式会将返回的字符串与视图解析器的前后缀拼接后跳转。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213143712384.png)

返回带有前缀的字符串：

- 转发：forward:/WEB-INF/views/index.jsp

- 重定向：redirect:/index.jsp



#### 1.2 通过ModelAndView对象返回

```java
@RequestMapping("/quick2")
public ModelAndView save2(){
    /*
    	Model:模型 封装数据
    	View：视图 展示数据
    */
    ModelAndView modelAndView = new ModelAndView();
    //设置模型数据,取：success.jsp--><h1>Success!${username}</h1>
    modelAndView.addObject("username","itcast");
    //设置视图名称
    modelAndView.setViewName("success");
    return modelAndView;
}

@RequestMapping("/quick3")
public ModelAndView save3(ModelAndView modelAndView){
    modelAndView.setViewName("forward:/WEB-INF/views/success.jsp");
    return modelAndView;
}

@RequestMapping("/quick4")
public String save4(Model model){
    model.addAttribute("username","itcast");
    return "success";
}
```



#### 1.3 向request域存储数据

① 通过SpringMVC框架注入的request对象setAttribute()方法设置

```java
@RequestMapping("/quick5")
public String save5(HttpServletRequest request){
	request.setAttribute("name","zhangsan");
	return "success"; 
}
```

② 通过ModelAndView的addObject()方法设置



### 2. 回写数据

#### 2.1 直接返回字符串

​	Web基础阶段，客户端访问服务器端，如果想直接回写字符串作为响应体返回的话，只需要使用response.getWriter().print(“hello world”) 即可，那么在Controller中想直接回写字符串该怎样呢？



① 通过SpringMVC框架注入的response对象，使用response.getWriter().print(“hello world”) 回写数据，此时不需要视图跳转，业务方法返回值为void。

```java
@RequestMapping("/quick6")
    public void save6(HttpServletResponse response) throws IOException {
		response.getWriter().print("hello world");
}
```



② 将需要回写的字符串直接返回，但此时需要通过**@ResponseBody**注解告知SpringMVC框架，方法返回的字符串不是跳转是直接在http响应体中返回。

```java
@RequestMapping("/quick7")
@ResponseBody
public String save7() throws IOException {
	return "hello springMVC!!!"; 
}
```

<hr>

在异步项目中，客户端与服务器端往往要进行json格式字符串交互，此时我们可以手动拼接json字符串返回。

```java
@RequestMapping("/quick8")
@ResponseBody
public String save8() throws IOException {
	return "{\"name\":\"zhangsan\",\"age\":18}"; 
}
```



上述方式手动拼接json格式字符串的方式很麻烦，开发中往往要将复杂的java对象转换成json格式的字符串，我们可以使用web阶段学习过的json转换工具jackson进行转换，导入jackson坐标。

```xml
<!--jackson--> 
<dependency> 
    <groupId>com.fasterxml.jackson.core</groupId> 
    <artifactId>jackson-core</artifactId> 
    <version>2.9.0</version>
</dependency>

<dependency> 
    <groupId>com.fasterxml.jackson.core</groupId> 
    <artifactId>jackson-databind</artifactId> 
    <version>2.9.0</version>
</dependency> 

<dependency> 
    <groupId>com.fasterxml.jackson.core</groupId> 
    <artifactId>jackson-annotations</artifactId> 
    <version>2.9.0</version>
</dependency>
```

通过jackson转换json格式字符串，回写字符串。

```java
@RequestMapping("/quick9")
@ResponseBody
public String save9() throws IOException {
    User user = new User();
    user.setUsername("zhangsan");  //User需要生成get方法
    user.setAge(18);
    ObjectMapper objectMapper = new ObjectMapper();
    String json = objectMapper.writeValueAsString(user);
    return json;
}
```



#### 2.2 返回对象或集合

通过SpringMVC帮助我们对对象或集合进行json字符串的转换并回写，为处理器适配器配置消息转换参数，指定使用jackson进行对象或集合的转换，因此需要在spring-mvc.xml中进行如下配置：

```xml
<!--配置处理器映射器-->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"> 		
    <property name="messageConverters"> 
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">				</bean>
		</list>
	</property>
</bean>
```



```java
@RequestMapping("/quick10")
@ResponseBody
public User save10() throws IOException {
    User user = new User();
    user.setUsername("zhangsan");
    user.setAge(18);
    return user;
}
```



在方法上添加**@ResponseBody**就可以返回json格式的字符串，但是这样配置比较麻烦，配置的代码比较多，因此，我们可以使用mvc的注解驱动代替上述配置。

```xml
<!--mvc的注解驱动--> 
<mvc:annotation-driven/>
```

在 SpringMVC 的各个组件中，**处理器映射器**、**处理器适配器**、**视图解析器**称为 SpringMVC 的三大组件。

使用`<mvc:annotation-driven>`自动加载 RequestMappingHandlerMapping（处理映射器）和RequestMappingHandlerAdapter（处理适配器 ），可用在Spring-xml.xml配置文件中使用`<mvc:annotation-driven>`替代注解处理器和适配器的配置。同时使用`<mvc:annotation-driven>`默认底层就会集成jackson进行对象或集合的json格式字符串的转换。



## 二、获得请求数据

> 客户端请求参数的格式是：name=value&name=value… …

​	服务器端要获得请求的参数，有时还需要进行数据的封装，SpringMVC可以接收如下类型的参数：

- 基本类型参数

- POJO类型参数

- 数组类型参数

- 集合类型参数

### 1. 获得基本类型参数

Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配。

`http://localhost:8080/quick11?username=zhangsan&age=12`

```java
@RequestMapping("/quick11")
@ResponseBody
public void save11(String username,int age) throws IOException {
    System.out.println(username);
    System.out.println(age);
}
```



### 2.获得POJO类型参数

Controller中的业务方法的POJO参数的<font color='red'>属性名</font>与<font color='red'>请求参数的name</font>一致，参数值会自动映射匹配。

`http://localhost:8080/quick12?username=zhangsan&age=12`

```java
@RequestMapping("/quick12")
@ResponseBody
public void save12(User user) throws IOException {
	System.out.println(user);
}
```



### 3.获得数组类型参数

Controller中的业务方法<font color='red'>数组名称</font>与请求参数的name一致，参数值会自动映射匹配。
`http://localhost:8080/quick13?strs=111&strs=222&strs=333`

```java
@RequestMapping("/quick13")
@ResponseBody
public void save13(String[] strs) throws IOException {
	System.out.println(Arrays.asList(strs));
}

//控制台--> [111,222,333]
```



### 4. 获得集合类型参数

获得集合参数时，要将集合参数包装到一个POJO中才可以。

form.jsp

```jsp
<%@page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<body>
<form action="${pageContext.request.contextPath}/quick14" method="post">
    <input type="text" name="userList[0].name"><br>
    <input type="text" name="userList[0].age"><br>
    <input type="text" name="userList[1].name"><br>
    <input type="text" name="userList[1].age"><br>
    <input type="submit" value="提交"><br>
</form>
</body>
</html>
```



```java
public class VO {
    private List<User> userList;
    getter/setter...
    toString...
}
```



```java
@RequestMapping("/quick14")
@ResponseBody
public void save14(Vo vo) throws IOException {
	System.out.println(vo.getUserList());
}
```



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213153806378.png)

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213154114904.png)

<hr>

当使用ajax提交时，可以指定contentType为json形式，那么在方法参数位置使用@RequestBody可以直接接收集合数据而无需使用POJO进行包装。

ajax.jsp

```jsp
<script src="${pageContext.request.contextPath}/js/jquery-3.3.1.js"></script>
<script>	
    var userList = new Array();
    userList.push({username:"zhangsan",age:18});
    userList.push({username:"lisi",age:28});

    $.ajax({
        type:"POST",
        url:"${pageContext.request.contextPath}/quick15",
        data:JSON.stringify(userList),
        contentType:"application/json;charset=utf-8"
    });
</script>
```

```java
@RequestMapping("/quick15")
@ResponseBody
public void save15(@RequestBody List<User> userList) throws IOException {
    System.out.println(userList);
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213155719427.png)



```xml
<!--开放资源的访问，一般是静态文件-->
<mvc:resources mapping="/js/**" location="/js/"/>
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213160329251.png)



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213160453159.png)



> 静态资源访问的开启：spring-mvc.xml中

```xml
<!--开放资源的访问-->
<mvc:resources mapping="/js/**" location="/js/"/>
<mvc:resources mapping="/img/**" location="/img/"/>

<!--效果等同于上面-->
<mvc:default-servlet-handler/>
```



### 5. 请求数据乱码问题

> 当post请求时，数据会出现乱码，我们可以设置一个过滤器来进行编码的过滤。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213161411332.png)



```xml
<!--配置全局过滤的filter-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213161940936.png)



### 6. 参数绑定注解@RequestParam

> 当请求的参数名称与Controller的业务方法参数名称不一致时，就需要通过@RequestParam注解显示的绑定。

```jsp
<form action="${pageContext.request.contextPath}/quick16" method="post"> 
    <input type="text" name="name"><br> 
    <input type="submit" value="提交"><br>
</form>
```

```java
@RequestMapping("/quick16")
@ResponseBody
public void save16(@RequestParam("name") String username) throws IOException {
	System.out.println(username);
}
```



注解@RequestParam还有如下参数可以使用：

- value：请求参数名称
- required：此在指定的请求参数是否必须包括，默认是true，提交时如果没有此参数则报错
- defaultValue：当没有指定请求参数时，则使用指定的默认值赋值

```java
@RequestMapping("/quick17")
@ResponseBody
public void save17(@RequestParam(value="name",required = false,defaultValue = "itcast") String username) throws IOException {
	System.out.println(username);
}
```



### 7. 获得Restful风格的参数

> Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。主要用于客户端和服务器交互类的软件，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。
> Restful风格的请求是使用“url+请求方式”表示一次请求目的的，HTTP 协议里面四个表示操作方式的动词如下：

- GET：用于获取资源

- POST：用于新建资源

- PUT：用于更新资源

- DELETE：用于删除资源

```bash
# 例如：
/user/1 GET ： 得到 id = 1 的 user
/user/1 DELETE： 删除 id = 1 的 user
/user/1 PUT： 更新 id = 1 的 user
/user POST： 新增 user
```



> 上述url地址/user/1中的1就是要获得的请求参数，在SpringMVC中可以使用占位符进行参数绑定。地址/user/1可以写成/user/{id}，占位符{id}对应的就是1的值。在业务方法中我们可以使用@PathVariable注解进行占位符的匹配获取工作。



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213182419923.png)



### 8. 自定义类型转换器

> SpringMVC 默认已经提供了一些常用的类型转换器，例如客户端提交的字符串转换成int型进行参数设置。
>
> 但是不是所有的数据类型都提供了转换器，没有提供的就需要自定义转换器，例如：日期类型的数据就需要自定义转换器。



自定义类型转换器的开发步骤：

① 定义转换器类实现Converter接口


```java
public class DataConverter implements Converter<String, Date> {

    @Override
    public Date convert(String dataStr) {
        // 将日期字符串转换成日期对象 返回
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        Date data = null;
        try {
            data = format.parse(dataStr);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return data;
    }
}
```



② 在配置文件中声明转换器

```xml
<!--声明转换器-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
      <property name="converters">
            <list>
                <bean class="com.itheima.converter.DataConverter"></bean>
            </list>
      </property>
</bean>
```



③ 在`<annotation-driven>`中引用转换器

```xml
<mvc:annotation-driven conversion-service="conversionService"/>
```



```java
@RequestMapping("/quick18")
@ResponseBody
public void save18(Date data) throws IOException {
    System.out.println(data);
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213184901591.png)



### 9. 获得Servlet相关API

> SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入，常用的对象如下：
>
> - HttpServletRequest
>
> - HttpServletResponse
>
> - HttpSession



> 框架负责创建的思想！！！

```java
/**
 * SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入
 * @param request
 * @param response
 * @param session
 * @throws IOException
 */
@RequestMapping("/quick19")
@ResponseBody
public void save19(HttpServletRequest request, HttpServletResponse response,
                   HttpSession session) throws IOException {
    System.out.println(request);
    System.out.println(response);
    System.out.println(session);
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213185706350.png)



### 10. 获得请求头

#### 10.1 @RequestHeader

> 相当于web阶段学习的request.getHeader(name)，@RequestHeader注解的属性如下：
>
> - **value**：请求头的名称
>
> - **required**：是否必须携带此请求头



```java
/**
 * @RequestHeader 获得请求头
 * @param User_Agent
 * @throws IOException
 */
@RequestMapping("/quick20")
@ResponseBody
public void save20(@RequestHeader(value = "User-Agent")String User_Agent) throws IOException {
    System.out.println(User_Agent);
}
```

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213190755497.png)



#### 10.2 @CookieValue

> 使用@CookieValue可以获得指定Cookie的值，@CookieValue注解的属性如下：
>
> - **value**：指定cookie的名称
>
> - **required**：是否必须携带此cookie

```java
@RequestMapping("/quick21")
@ResponseBody
public void save21(@CookieValue(value = "JSESSIONID",required = false) String jsessionid){
	System.out.println(jsessionid);
}
```



### 11. 文件上传

#### 11.1 文件上传原理

文件上传客户端三要素

- 表单项type=“file”
- 表单的提交方式是post
- 表单的enctype属性是多部分表单形式，及enctype=“multipart/form-data”

```jsp
<form action="${pageContext.request.contextPath}/quick20" method="post" enctype="multipart/form-data">
	名称：<input type="text" name="name"><br>
	文件：<input type="file" name="file"><br> 
    <input type="submit" value="提交"><br>
</form>
```

<hr>

文件上传原理

- form表单修改为多部分表单时，request.getParameter()将失效。

- enctype=“application/x-www-form-urlencoded”时，form表单的正文内容格式是：

​			key=value&key=value&key=value

- 当form表单的enctype取值为Mutilpart/form-data时，请求正文内容就变成多部分形式

![image-20220213202957043](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213202957043.png)

<hr>

步骤

① 导入fileupload和io坐标
② 配置文件上传解析器
③ 编写文件上传代码

<hr>

#### 11.2 单文件上传实现

① 导入fileupload和io坐标

```xml
<!-- 文件上传部分依赖 -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.4</version>
</dependency>
```



② 配置文件上传解析器

```xml
<!--配置文件上传解析器-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="UTF-8"/>
    <property name="maxUploadSize" value="5000000"/>
</bean>
```



③ 编写文件上传代码

```java
/**
 * 文件上传
 * @param username
 * @param uploadFile
 * @throws IOException
 */
@RequestMapping("/quick22")
@ResponseBody
public void save22(String username, MultipartFile uploadFile) throws IOException {
    System.out.println(username);
    // 获得上传文件的名称
    String filename = uploadFile.getOriginalFilename();
    uploadFile.transferTo(new File("/Users/cat/upload/" + filename));
}
```



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213195753750.png)



#### 11.3 多文件上传实现

> 多文件上传，只需要将页面修改为多个文件上传项，将方法参数MultipartFile类型修改为MultipartFile[]即可

```jsp
<form action="${pageContext.request.contextPath}/user/quick23" method="post" enctype="multipart/form-data">
    名称<input type="text" name="username"><br/>
    多文件上传-文件1<input type="file" name="uploadFiles"><br/>
    多文件上传-文件2<input type="file" name="uploadFiles"><br/>
    <input type="submit" value="提交">
</form>
```



```java
/**
 * 多文件上传
 * @param username
 * @param uploadFiles
 * @throws IOException
 */
@RequestMapping("/quick23")
@ResponseBody
public void save23(String username, MultipartFile[] uploadFiles) throws IOException {
    System.out.println(username);

    for (MultipartFile uploadFile : uploadFiles) {
        // 获得上传文件的名称
        String filename = uploadFile.getOriginalFilename();
        uploadFile.transferTo(new File("/Users/cat/upload/" + filename));
    }
}
```



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220213200911488.png)



# 第4章 AOP

## 一、简介

> **AOP** 为 **A**spect **O**riented **P**rogramming 的缩写，意思为面向切面编程，是通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。
>
> 动态代理：在不修改源码的情况下，对目标方法进行相应的增强（修改），作用是完成程序功能间的松耦合。
>
> AOP 是 OOP 的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。



## 1. 作用和优势

- 作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强

- 优势：减少重复代码，提高开发效率，并且便于维护



## 2. AOP的底层实现

实际上，AOP 的底层是通过 Spring 提供的的动态代理技术实现的。

在运行期间，Spring通过动态代理技术动态的生成代理对象，代理对象方法执行时进行增强功能的介入，在去调用目标对象的方法，从而完成功能的增强。



## 3. AOP的动态代理技术

常用的动态代理技术

- JDK 代理 : 基于接口的动态代理技术

- cglib 代理：基于父类的动态代理技术

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220321132524948.png)









## 二、基于XML的AOP开发





## 三、基于注解的AOP开发



