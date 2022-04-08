---
title: 03-Maven
date: 2022-02-09 17:36:27
tags: [Java]
---

# 第1章 Maven基础

## 一、简介

> Maven是专门用于管理和构建Java项目的工具，它的主要功能有：
>
> - 提供了一套标准化的项目结构
>
> - 提供了一套标准化的构建流程（编译，测试，打包，发布……）
>
> - 提供了一套依赖管理机制

**标准化的项目结构：**

​	项目结构我们都知道，每一个开发工具（IDE）都有自己不同的项目结构，它们互相之间不通用。在eclipse中创建的目录，无法在idea中进行使用，这就造成了很大的不方便。

​	而Maven提供了一套标准化的项目结构，所有的IDE使用Maven构建的项目完全一样，所以IDE创建的Maven项目可以通用。如下图右边就是Maven构建的项目结构。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726153815028.png" alt="image-20210726153815028" style="zoom:150%;" />



**标准化的构建流程：**

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726154144488.png" alt="image-20210726154144488" style="zoom:80%;" />

​	如上图所示我们开发了一套系统，代码需要进行编译、测试、打包、发布，这些操作如果需要反复进行就显得特别麻烦，而Maven提供了一套简单的命令来完成项目构建。

**依赖管理：**

​	依赖管理其实就是管理你项目所依赖的第三方资源（jar包、插件）。如之前我们项目中需要使用JDBC和Druid的话，就需要去网上下载对应的依赖包（当前之前是老师已经下载好提供给大家了），复制到项目中，还要将jar包加入工作环境这一系列的操作。如下图所示

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726154753631.png" alt="image-20210726154753631" style="zoom:80%;" />

​	而Maven使用标准的 ==坐标== 配置来管理各种依赖，只需要简单的配置就可以完成依赖管理。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726154922337.png" alt="image-20210726154922337" style="zoom:80%;" />

​	如上图右边所示就是mysql驱动包的坐标，在项目中只需要写这段配置，其他都不需要我们担心，Maven都帮我们进行操作了。

<hr>

### 1. Maven模型

* 项目对象模型POM (Project Object Model)
* 依赖管理模型(Dependency)
* 插件(Plugin)



<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726155759621.png" alt="image-20210726155759621" style="zoom:80%;" />

​	如上图所示就是Maven的模型，而我们先看紫色框框起来的部分，他就是用来完成 `标准化构建流程` 。如我们需要编译，Maven提供了一个编译插件供我们使用，我们需要打包，Maven就提供了一个打包插件提供我们使用等。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726160928515.png" alt="image-20210726160928515" style="zoom:80%;" />

​	上图中紫色框起来的部分，项目对象模型就是将我们自己抽象成一个对象模型，有自己专属的坐标，如下图所示是一个Maven项目：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726161340796.png" alt="image-20210726161340796" style="zoom:80%;" />

​	依赖管理模型则是使用坐标来描述当前项目依赖哪些第三方jar包，如下图所示

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726161616034.png)



### 2. 仓库

​	依赖jar包是存储在我们的本地仓库中，而项目运行时从本地仓库中拿需要的依赖jar包。

​	**仓库分类：**

* 本地仓库：自己计算机上的一个目录

* 中央仓库：由Maven团队维护的全球唯一的仓库

  * 地址： https://repo1.maven.org/maven2/

* 远程仓库(私服)：一般由公司团队搭建的私有仓库

  今天我们只学习远程仓库的使用，并不会搭建。

当项目中使用坐标引入对应依赖jar包后，首先会查找本地仓库中是否有对应的jar包：

* 如果有，则在项目直接引用;

* 如果没有，则去中央仓库中下载对应的jar包到本地仓库。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726162605394.png" alt="image-20210726162605394" style="zoom:70%;" />

如果还可以搭建远程仓库，将来jar包的查找顺序则变为：

> 本地仓库 --> 远程仓库--> 中央仓库

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726162815045.png" alt="image-20210726162815045" style="zoom:70%;" />



## 二、安装配置

* 解压 apache-maven-3.6.1.rar 既安装完成

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726163219682.png" alt="image-20210726163219682" style="zoom:90%;" />

  > 建议解压缩到没有中文、特殊字符的路径下。如课程中解压缩到 `D:\software` 下。

  解压缩后的目录结构如下：

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726163518885.png" alt="image-20210726163518885" style="zoom:80%;" />

  * bin目录 ： 存放的是可执行命令。mvn 命令重点关注。
  * conf目录 ：存放Maven的配置文件。`settings.xml` 配置文件后期需要修改。
  * lib目录 ：存放Maven依赖的jar包。Maven也是使用java开发的，所以它也依赖其他的jar包。

  

* 配置环境变量 MAVEN_HOME 为安装路径的bin目录

  `设置`   -->`系统`  -->  `关于`   -->  `高级系统设置`  -->  `高级`  -->  `环境变量`

  在系统变量处新建一个变量 `MAVEN_HOME`

  ![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220209121233619.png)

  在 `Path` 中进行配置

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726164146832.png" alt="image-20210726164146832" style="zoom:80%;" />

  打开命令提示符进行验证，出现如图所示表示安装成功

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726164306480.png" alt="image-20210726164306480" style="zoom:80%;" />



* 配置本地仓库

  修改 conf/settings.xml 中的 <localRepository> 为一个指定目录作为本地仓库，用来存储jar包。

  ![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220209124832682.png)

* 配置阿里云私服

  中央仓库在国外，所以下载jar包速度可能比较慢，而阿里公司提供了一个远程仓库，里面基本也都有开源项目的jar包。

  修改 conf/settings.xml 中的 <mirrors>标签，为其添加如下子标签：

  ```xml
  <!--配置具体的仓库的下载镜像-->
  <mirror> 
      <!--此镜像的唯一标识，用来区分不同的mirror元素-->
      <id>alimaven</id>  
      <!--镜像名称-->
      <name>aliyun maven</name>
      <!--镜像url-->
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <!--对哪种仓库进行镜像，简单说就是替代哪个仓库-->
      <mirrorOf>central</mirrorOf>          
  </mirror>
  ```

<hr>

全局setting与用户setting的区别

- 全局setting定义了当前计算器中Maven的公共配置
- 用户setting定义了当前用户的配置



## 三、基本使用

### 1. 常用命令

> * compile ：编译
>
> * clean：清理
>
> * test：测试
>
> * package：打包
>
> * install：安装

**命令演示：**

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726170404545.png" alt="image-20210726170404545" style="zoom:70%;" />

而我们使用上面命令需要在磁盘上进入到项目的 `pom.xml` 目录下，打开命令提示符（shift+右键）

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726170549907.png" alt="image-20210726170549907" style="zoom:70%;" />

**编译命令演示：**

```java
compile ：编译
```

执行上述命令可以看到：

* 从阿里云下载编译需要的插件的jar包，在本地仓库也能看到下载好的插件
* 在项目下会生成一个 `target` 目录

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726171047324.png" alt="image-20210726171047324" style="zoom:80%;" />

同时在项目下会出现一个 `target` 目录，编译后的字节码文件就放在该目录下

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726171346824.png" alt="image-20210726171346824" style="zoom:80%;" />

**清理命令演示：**

```
mvn clean
```

执行上述命令可以看到

* 从阿里云下载清理需要的插件jar包
* 删除项目下的 `target` 目录

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726171558786.png" alt="image-20210726171558786" style="zoom:80%;" />

**打包命令演示：**

```
mvn package
```

执行上述命令可以看到：

* 从阿里云下载打包需要的插件jar包
* 在项目的 `terget` 目录下有一个jar包（将当前项目打成的jar包）

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726171747125.png" alt="image-20210726171747125" style="zoom:80%;" />

**测试命令演示：**

```
mvn test  
```

该命令会执行所有的测试代码。执行上述命令效果如下

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726172343933.png" alt="image-20210726172343933" style="zoom:80%;" />

**安装命令演示：**

```
mvn install
```

该命令会将当前项目打成jar包，并安装到本地仓库。执行完上述命令后到本地仓库查看结果如下：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726172709112.png" alt="image-20210726172709112" style="zoom:80%;" />



### 2. 生命周期

> Maven 构建项目生命周期描述的是一次构建过程经历经历了多少个事件
>
> Maven 对项目构建的生命周期划分为3套：
>
> - clean ：清理工作。
> - default ：核心工作，例如编译，测试，打包，安装等。
> - site ： 产生报告，发布站点等。这套声明周期一般不会使用。

<hr>

同一套生命周期内，执行后边的命令，前面的所有命令会自动执行。例如默认（default）生命周期如下：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726173153576.png" alt="image-20210726173153576" style="zoom:80%;" />

- 当我们执行 `install`（安装）命令时，它会先执行 `compile`命令，再执行 `test ` 命令，再执行 `package` 命令，最后执行 `install` 命令。

- 当我们执行 `package` （打包）命令时，它会先执行 `compile` 命令，再执行 `test` 命令，最后执行 `package` 命令。

<hr>

#### 2.1 clean生命周期

- pre-clean     执行一些需要在clean之前完成的工作
- clean             移除所有上一次构建生成的文件
- post-clean    执行一些需要在clean之后立刻完成的工作



#### 2.2 default构建生命周期

默认的生命周期也有对应的很多命令，其他的一般都不会使用，我们只关注常用的：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726173619353.png" alt="image-20210726173619353" style="zoom:80%;" />



#### 2.3 site构建生命周期

- pre-site         执行一些需要在生成站点文档之前完成的工作
- site                 生成项目的站点文档
- post-site        执行一些需要在生成站点文档之后完成的工作,并且为部署做准备
- site-deploy    将生成的站点文档部署到特定的服务器上



### 3. 插件

> 插件与生命周期内的阶段绑定,在执行到对应生命周期时执行对应的插件功能
>
> 默认maven在各个生命周期上绑定有预设的功能
>
> 通过插件可以自定义其他功能

```xml
<build> 
    <plugins> 
        <plugin> 
            <groupId>org.apache.maven.plugins</groupId> 
            <artifactId>maven-source-plugin</artifactId>
            <version>2.2.1</version> 
            <executions> 
                <execution> 
                    <goals> 
                        <goal>jar</goal>
                    </goals> 
                    <phase>generate-test-resources</phase>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```



## 四、IDEA使用Maven

### 1.配置Maven环境

我们需要先在IDEA中配置Maven环境：

* 选择 IDEA中 File --> Settings

* 搜索 maven 

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726174229396.png" alt="image-20210726174229396" style="zoom:80%;" />

* 设置 IDEA 使用本地安装的 Maven，并修改配置文件路径

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726174248050.png" alt="image-20210726174248050" style="zoom:70%;" />

  

### 2. Maven坐标

**什么是坐标？**

* Maven 中的坐标是==资源的唯一标识==
* 使用坐标来定义项目或引入项目中需要的依赖

**Maven 坐标主要组成**

* groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）
* artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
* version：定义当前项目版本号

如下图就是使用坐标表示一个项目：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726174718176.png)

> ==注意：==
>
> * 上面所说的资源可以是插件、依赖、当前项目。
> * 我们的项目如果被其他的项目依赖时，也是需要坐标来引入的。



### 3. 创建Maven项目

* 创建模块，选择Maven，点击Next

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726175049876.png" alt="image-20210726175049876" style="zoom:90%;" />

* 填写模块名称，坐标信息，点击finish，创建完成

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726175109822.png" alt="image-20210726175109822" style="zoom:80%;" />

  创建好的项目目录结构如下：

  ![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726175244826.png)

* 编写 HelloWorld，并运行



### 4. 导入Maven项目

* 选择右侧Maven面板，点击 + 号

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726182702336.png" alt="image-20210726182702336" style="zoom:70%;" />

* 选中对应项目的pom.xml文件，双击即可

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726182648891.png" alt="image-20210726182648891" style="zoom:70%;" />

* 如果没有Maven面板，选择

  View --> Appearance --> Tool Window Bars

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726182634466.png" alt="image-20210726182634466" style="zoom:80%;" />



可以通过下图所示进行命令的操作：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726182902961.png" alt="image-20210726182902961" style="zoom:80%;" />

**配置 Maven-Helper 插件** 

* 选择 IDEA中 File --> Settings

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726192212026.png" alt="image-20210726192212026" style="zoom:80%;" />

* 选择 Plugins

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726192224914.png" alt="image-20210726192224914" style="zoom:80%;" />

* 搜索 Maven，选择第一个 Maven Helper，点击Install安装，弹出面板中点击Accept

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726192244567.png" alt="image-20210726192244567" style="zoom:80%;" />

* 重启 IDEA

安装完该插件后可以通过 选中项目右键进行相关命令操作，如下图所示：

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726192430371.png" alt="image-20210726192430371" style="zoom:80%;" />

<hr>

tomcat7运行插件

```xml
<build> 
    <plugins> 
        <plugin> 
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId> 
            <version>2.1</version> 
            <configuration> 
                <port>80</port> 
                <path>/</path>
            </configuration>
        </plugin>
    </plugins>
</build>
```



## 五、依赖管理

### 1.  依赖配置

**使用坐标引入jar包的步骤：**

* 在项目的 pom.xml 中编写 <dependencies> 标签

* 在 <dependencies> 标签中 使用 <dependency> 引入坐标

* 定义坐标的 groupId，artifactId，version

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726193105765.png" alt="image-20210726193105765" style="zoom:70%;" />

* 点击刷新按钮，使坐标生效

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726193121384.png" alt="image-20210726193121384" style="zoom:60%;" />

>  注意：
>
>  * 具体的坐标我们可以到如下网站进行搜索
>  * https://mvnrepository.com/

**快捷方式导入jar包的坐标：**

每次需要引入jar包，都去对应的网站进行搜索是比较麻烦的，接下来给大家介绍一种快捷引入坐标的方式

* 在 pom.xml 中 按Fn+ alt + insert，选择 Dependency

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726193603724.png" alt="image-20210726193603724" style="zoom:60%;" />

* 在弹出的面板中搜索对应坐标，然后双击选中对应坐标

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726193625229.png" alt="image-20210726193625229" style="zoom:60%;" />

* 点击刷新按钮，使坐标生效

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726193121384.png" alt="image-20210726193121384" style="zoom:60%;" />

**自动导入设置：**

上面每次操作都需要点击刷新按钮，让引入的坐标生效。当然我们也可以通过设置让其自动完成

* 选择 IDEA中 File --> Settings

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726193854438.png" alt="image-20210726193854438" style="zoom:60%;" />

* 在弹出的面板中找到 Build Tools

  <img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726193909276.png" alt="image-20210726193909276" style="zoom:80%;" />

* 选择 Any changes，点击 ok 即可生效



### 2.  依赖传递

#### 2.1 传递性

- 直接依赖：在当前项目中通过依赖配置建立的依赖关系

- 间接依赖：被依赖的资源如果依赖其他资源，当前项目间接依赖其他资源



#### 2.2 依赖传递冲突

- 路径优先：当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高

- 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖顺序靠后的

- 特殊优先：当同级配置了相同资源的不同版本，后配置的覆盖先配置的



### 3. 可选依赖

> 可选依赖指对外隐藏当前所依赖的资源——不透明

```xml
<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId> 
    <version>4.12</version> 
    <optional>true</optional>
</dependency>
```



### 4. 排除依赖

>排除依赖指主动断开依赖的资源，被排除的资源无需指定版本――不需要

```xml
<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId> 
    <version>4.12</version> 
    <exclusions> 
        <exclusion> 
            <groupId>org.hamcrest</groupId> 
            <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```



### 5. 依赖范围

> 通过设置坐标的依赖范围(scope)，可以设置 对应jar包的作用范围。
>
> 作用范围：
>
> - 主程序范围有效（main文件夹范围内）
> - 测试程序范围有效（test文件夹范围内）
> - 是否参与打包（package指令范围内）

如下图所示给 `junit` 依赖通过 `scope` 标签指定依赖的作用范围。 那么这个依赖就只能作用在测试环境，其他环境下不能使用。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20210726194703845.png" alt="image-20210726194703845" style="zoom:70%;" />

 `scope` 取值：

| **依赖范围**        | 主代码 | 测试代码 | 打包 | 例子        |
| ------------------- | ------ | -------- | ---- | ----------- |
| **compile（默认）** | Y      | Y        | Y    | log4j       |
| **test**            |        | Y        |      | junit       |
| **provided**        | Y      | Y        |      | servlet-api |
| **runtime**         |        |          | Y    | jdbc驱动    |

<hr>

**依赖范围传递性**

带有依赖范围的资源在进行传递时.作用范围将收到影响

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220218091348257.png)





# 第2章 Maven高级

## 一、分模块开发与设计

### 1. 工程模块与模块划分

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220218093940806.png)



2.ssm_pojo拆分





















## 二、 聚合

> 聚合用于快速构建maven工程，一次性构建多个项目/模块。所有模块一起编译和安装。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220130202201089.png)



**制作方式**

1. 创建一个空模块，打包类型定义为`pom`。

> 聚合工程写pom，web工程为war，默认为jar。

```xml
<packaging>pom</packaging>
```

2. 导入（关联）其它模块。

```xml
<modules> 
  	<module>../ssm_controller</module> 
  	<module>../ssm_service</module> 
  	<module>../ssm_dao</module> 
  	<module>../ssm_pojo</module>
</modules>
```



## 三、继承（统一版本）

> 在子工程中沿用父工程的配置。

**具体操作：**

1. 在子工程中声明其父工程坐标与对应的位置。

2. 在子工程中定义依赖，无需声明依赖版本，版本参照父工程。



```xml
<!--声明此处进行依赖管理-->
<dependencyManagement>
    <!--具体的依赖-->
    <dependencies>
        <!--spring环境-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
<dependencyManagement>
```



```xml
<!--定义该工程的父工程-->
<parent>
    <groupId>com.itheima</groupId>
    <artifactId>ssm_maven</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--填写父工程的pom文件-->
    <relativePath>../pom.xml</relativePath>
</parent>

<!--坐标gv可省略-->
<!--<groupId>com.itheima</groupId>-->
<artifactId>ssm_controller</artifactId>
<!--<version>1.0-SNAPSHOT</version>-->
<packaging>jar</packaging>
```



#### 继承的资源

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20211203003854520.png)



#### 继承与聚合

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220203220831809.png)



## 四、属性（定义变量）

#### 版本统一的重要性

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207110405488.png)



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207110321250.png)



#### 属性类别

##### 自定义属性（用的最多，也就是定义变量）

见上图。

##### 内置属性

```xml
# 使用maven内置属性，快速配置
${basedir}
${version}
```



##### Setting属性

```xml
# 使用maven配置文件setting.xml中的标签属性，用于动态配置
${settings.localRepository}  
```



##### Java系统属性

```xml
# 读取Java系统属性
# 调用格式
${user.home}
# 系统属性查询方式
mvn help:system
```



##### 环境变量属性

```xml
# 使用maven配置文件setting.xml中的标签属性，用于动态配置
# 调用格式
${env.JAVA_HOME}
# 环境变量属性查询方式
mvn help:system
```



## 五、版本管理SNAPSHOT&&RELEASE

```xml
<version>2.2.5-SNAPSHOT</version>
```

#### 工程版本

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207111931028.png)

#### 工程版本号约定

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207112037429.png)



## 六、资源配置

#### 资源配置多文件维护

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207113832719.png)

#### 配置文件引用pom属性

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207114045019.png)



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207114405553.png)



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207114531133.png)



## 七、多环境开发配置profile



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207114936440.png)



#### 配置多环境

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207120751547.png)

#### 加载指定环境

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207120403277.png)



```sh
# 加载指定环境配置
mvn 指令 –P 环境定义id
mvn install –P pro_env
```



## 八、跳过测试（了解）

#### 应用场景

> 整体模块功能未开发
>
> 模块中某个功能未开发完毕
>
> 单个功能更新调试导致其他功能失败
>
> 快速打包



#### 3种方式

##### 命令

```sh
mvn 指令 –D skipTests
```

##### 界面

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207121255254.png)

##### 配置

```xml
<!--配置跳过测试-->
<plugin>
    <groupId>org.apache.maven</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.22.1</version>
    <configuration>
        <!--设置跳过测试-->
        <skipTests>true</skipTests>
        <includes>
            <include>
                <!--包含指定的用例-->
            </include>
        </includes>
        <excludes>
            <exclude>
                <!--排除指定的用例-->
            </exclude>
        </excludes>
    </configuration>
</plugin>
```



## 九、私服（重点）

> 想到第一次工作的时候，组长让我在私服上发布一下`4a-server-api`组件，我不会。。。

#### 分模块开发合作

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207154312947.png)



#### Nexus

> Nexus是Sonatype公司的一款maven私服产品
>
> 下载地址：https://help.sonatype.com/repomanager3/product-information/download

##### 安装

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207160645044.png)

##### 启动http://localhost:8081/

```sh
./nexus start
```



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207160408662.png)

> 去提示的目录复制你的密码进行登录，默认密码不是admin123，密码在文件admin.password中。



> 启动需要时间。

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207161839162.png" alt="" style="zoom:33%;" />

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207163400137.png)



##### 配置

![](https://notes2021.oss-cn-beijing.aliyuncs.com/2021/image-20220207162242048.png)



#### 私服资源获取

> 放资源的仓库，拿资源的仓库组，与外界中央仓库打交道的特殊仓库。



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207162841610.png)



#### 仓库分类

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207162948516.png)



#### 手工上传资源（仅用于第三方资源的上传）

> 创建仓库
>
> 加入仓库组`maven-public`

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207163756629.png)



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207164058897.png)



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207164614679.png)

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207164829490.png)



#### IDEA环境中资源的上传与下载

> 访问私服的用户名密码配在本地。上传位置配在工程中。下载地址（组地址）配在本地。

> 资源发布：
>
> - 设置私服访问权限
> - 设置资源上传路径（私服宿主仓库地址）
> - 设置资源下载路径（私服仓库组地址）



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207165443968.png)



##### 访问私服配置（本地仓库访问私服）

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207170338004.png)

##### 访问私服配置（项目工程访问私服）

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220207170833688.png)



```xml
<!--发布配置管理-->
<distributionManagement>
    <repository>
        <!--本地maven配置文件中找用户名密码-->
        <id>heima-release</id>
        <!--资源路径-->
        <url>http://localhost:8081/repository/heima-release/</url>
    </repository>
    <snapshotRepository>
        <id>heima-snapshots</id>
        <url>http://localhost:8081/repository/heima-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```


