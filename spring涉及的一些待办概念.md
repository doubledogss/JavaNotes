---
title: spring涉及的一些待办概念
date: 2022-02-12 20:23:54
tags:
---

### Listener(监听器)的作用和内部机制

作用：监听某个事件的发生，状态的改变
内部机制：接口回调

##### 一、 Listener监听三个域对象创建与销毁

1. 监听ServletContext域对象的创建与销毁：实现ServletContextListener接口。
   ServletContext域对象的生命周期：
   **创建：启动服务器时创建
   销毁：关闭服务器或者从服务器移除项目**

**作用：** 利用`ServletContextListener`监听器在创建`ServletContext`域对象时完成一些想要初始化的工作或者执行自定义任务调度。





我们常见的ApplicationContext本质上来说是一种维护Bean的定义和对象之间协作关系的高级接口，spring的核心是容器，有且不止一个容器：
