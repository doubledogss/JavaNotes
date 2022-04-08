---
title: litemall
date: 2022-04-06 15:39:01
tags:
---

# litemall项目笔记

## 项目启动

端口设置 

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220406155045791.png)



![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220406155252717.png)



<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220406155417440.png" style="zoom: 50%;" />



启动前端 npm run dev

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220406155608200.png" style="zoom:50%;" />



启动后端：运行引导类



## 接口

### SpringBoot2.x整合mybatis-generator插件

1.模块litemall-db下

pom.xml—bulid—plugins 标签内导入插件依赖

2.添加配置文件，设置数据库连接信息、生成文件存放位置

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220406162242515.png" alt="image-20220406162242515" style="zoom:50%;" />



3.运行

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220406163136047.png" style="zoom:50%;" />



### 首页

`litemall-admin-api` —> controller

`litemall-db` —> service

逆向工程生成的 <u>mapper接口</u> 和 <u>实体类&Example类</u> 存放在`litemall-db` —> dao、 domain

<hr>

1.首页显示4种数量，在首页的 AdminDashboardController注入4个service，调用它们的count()方法获取总数。

2.以orderService为例：在service中注入对应的mapper，使用mapper去操作数据库



<hr>

### 用户管理

会员管理AdminUserController (收获地址 会员收藏 会员足迹 搜索历史 意见反馈)

**admin/user/list  查询满足条件的所有用户**

- @RequestParam 用于接收请求参数为表单类型的数据，通常用在方法的参数前面。

- Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配（注解用不用均可）。

- 当请求的参数名称与Controller的业务方法参数名称不一致时，就需要通过@RequestParam注解显示的绑定。

<hr>

### 商场管理

行政区域 AdminRegionController（商品类目）

**admin/region/list 查询所有 省——市——区**

所有省市区信息都存储在litemall_region表中，

- type：区分行政区域类型，1省， 2市，3区县

- pid：行政区域父ID，例如区县的pid指向市，市的pid指向省，省的pid则是0

1.定义RegionVo

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220406180152694.png)

其中children的类型为List<RegionVo>

2.取出表中所有数据——List<LitemallRegion> regionList

regionList按type属性分流——>省、市、区

```java
Map<Byte, List<LitemallRegion>> collect = regionList.stream().collect(Collectors.groupingBy(LitemallRegion::getType)); 

List<LitemallRegion> provinceList = collect.get(provinceType);  //所有省
List<LitemallRegion> cityList = collect.get(cityType); //所有市
List<LitemallRegion> areaList = collect.get(areaType); //所有区

//对cityList按pid属性（标记属于哪个省）再次分流
Map<Integer, List<LitemallRegion>> cityListMap = cityList.stream().collect(Collectors.groupingBy(LitemallRegion::getPid));

//对areaList按pid属性（标记属于哪个市）再次分流
 Map<Integer, List<LitemallRegion>> areaListMap = areaList.stream().collect(Collectors.groupingBy(LitemallRegion::getPid));

//三层for循环封装数据
```



<hr>
### 品牌制造商





### 订单管理







