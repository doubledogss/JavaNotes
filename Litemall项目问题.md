---
title: Litemall项目问题
date: 2022-02-28 11:26:54
tags:
---

## 注解

@Resource

> @Resource和@Autowired注解都是用来实现依赖注入的。
>
> @AutoWried按by type自动注入，而@Resource默认按byName自动注入。

@Resource有两个重要属性，分别是name和type

@Resource依赖注入时查找bean的规则：(以用在field上为例)

1. 既不指定name属性，也不指定type属性，则自动按byName方式进行查找。如果没有找到符合的bean，则回退为一个原始类型进行查找，如果找到就注入。

2. 只是指定了@Resource注解的name，则按name后的名字去bean元素里查找有与之相等的name属性的bean。

3. 只指定@Resource注解的type属性，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常

4. 既指定了@Resource的name属性又指定了type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常

<hr>

@RequestParam

> @RequestParam：将请求参数绑定到控制器的方法参数上（是springmvc中接收普通参数的注解）

语法

```swift
语法：@RequestParam(value=”参数名”,required=”true/false”,defaultValue=””)
value：参数名
required：是否包含该参数，默认为true，表示该请求路径中必须包含该参数，如果不包含就报错。
defaultValue：默认参数值，如果设置了该值，required=true将失效，自动为false,如果没有传该参数，就使用默认值
```

<hr>

shiro注解权限控制-5个权限注解

- **RequiresAuthentication:**

  > 使用该注解标注的类，实例，方法在访问或调用时，当前Subject必须在当前session中已经过认证。

- **RequiresGuest:**

  > 使用该注解标注的类，实例，方法在访问或调用时，当前Subject可以是“gust”身份，不需要经过认证或者在原先的session中存在记录。

- **RequiresPermissions:**

  > 当前Subject需要拥有某些特定的权限时，才能执行被该注解标注的方法。如果当前Subject不具有这样的权限，则方法不会被执行。

- **RequiresRoles:**

  > 当前Subject必须拥有所有指定的角色时，才能访问被该注解标注的方法。如果当天Subject不同时拥有所有指定角色，则方法不会执行还会抛出AuthorizationException异常。

- **RequiresUser**

  > 当前Subject必须是应用的用户，才能访问或调用被该注解标注的类，实例，方法。



