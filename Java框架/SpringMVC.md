---
typora-copy-images-to: ..\..\..\Typora\upload
---

# SpringMVC的基本概念

## 三层架构模型

![image-20200514232047980](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514232047980.png)



## SpringMVC概述

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200506235621.png" style="zoom: 80%;" />

SpringMVC是一种基于Java的实现MVC设计模型的请求驱动类型的轻量级Web框架，属于 Spring FrameWork 的后续产品。它通过一套注解，让一个简单的Java类成为处理请求的控制器，而无须实现任何接口。同时它还支持RESTful编程风格的请求。





# SpringMVC的入门

## SpringMVC组件介绍

![image-20200515182321320](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515182321320.png)

* DispatcherServlet：前端控制器

  用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

* HandlerMapping：处理器映射器

  HandlerMapping负责根据用户请求找到Handler即处理器，SpringMVC提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

* Handler：处理器

  它就是我们开发中要编写的具体业务控制器。由DispatcherServlet把用户请求转发到Handler。由Handler对具体的用户请求进行处理。

* HandlAdapter：处理器适配器

  通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

* View Resolver：视图解析器

  View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。

* View：视图

  SpringMVC框架提供了很多的View视图类型的支持，包括：jstlView、freemarkerView、pdfView等。我们最常用的视图就是jsp。 一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。



##  SpringMVC的请求响应流程

![image-20200515184247465](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515184247465.png)





# 请求参数的绑定

## 支持封装的类型

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515191541611.png" alt="image-20200515194324587" style="zoom:80%;" />

## 将基本类型和String类型进行封装

```jsp
<a href="account/findAccount?accountId=10&accountName=zhangsan">查询账户</a>
```

```java
@RequestMapping("/findAccount") 
public String findAccount(Integer accountId,String accountName)
{...}
```

## 将实体类型进行封装

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515194324587.png" alt="image-20200515191541611" style="zoom:80%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515194347606.png" alt="image-20200515194347606" style="zoom:80%;" />

```java
//Account类
public class Account{
    private String username;
    private String password;
    private double money;
    private User user;
}
//User类
public class User{
    private String uname;
    private int age;
}
```

## 将集合类型进行封装

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515195632224.png" alt="image-20200515195712628" style="zoom:80%;" />

 实体类为：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515195712628.png" alt="image-20200515195632224" style="zoom:80%;" />

封装结果为：

![image-20200515195808631](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515195808631.png)

## 解决参数绑定过程中中文乱码的问题

在web.xml中配置过滤器

![image-20200515195200869](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515195200869.png)

## 自定义类型转换器

一般情况下，服务器从浏览器接收到的参数都是字符串类型，但是我们在接受参数时一般都不需要进行类型转换，这是因为spring自动帮助我们进行了类型转换。但是 有些数据格式spring无法转换，比如2020-5-15，spring在进行转换时就会报错（2020/5/15转换是没问题的）。所以这时我们需要自定义类型转换器。

首先了解一下spring提供的接口：（***<u>原码</u>***）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515201459480.png" alt="image-20200515201459480" style="zoom: 67%;" />

1. 我们可以在utils包下构建一个类来实现他的接口：例如

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515201654673.png" alt="image-20200515201654673" style="zoom:80%;" />

2. 在配置文件中配置自定义类型转换器

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515202050060.png" alt="image-20200515202050060" style="zoom: 67%;" />

3. 使转换器生效

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515202209764.png" alt="image-20200515202209764" style="zoom:80%;" />

## 获取Servlet原生API（request/response对象）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515212206653.png" alt="image-20200515212206653" style="zoom:80%;" />

想要获取什么 对象，直接加上参数就好了。



# 常用注解

## RequestMapping注解（可以写在类上/方法上）

作用：用于建立请求URL和处理请求方法之间的对应关系。

示例：

![image-20200515185123731](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515185123731.png)

![image-20200515185155360](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515185155360.png)

如果在类上也有注解：比如在类上的注解为@RequestMapping(path="/user")，子类上的注解为@RequestMapping(path="/testRequestMapping")，那么对应的链接应该为`user/testRequestMapping`

![image-20200515190914279](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515190914279.png)



## RequestParam

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515233339993.png" alt="image-20200515233339993" style="zoom: 80%;" />

示例：

![image-20200515233630946](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515233630946.png)



## RequestBody

![image-20200515233846629](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515233846629.png)

示例：

![image-20200515234043655](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515234043655.png)

![image-20200515234120172](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515234120172.png)

获取到的请求体内容：

![image-20200515234158333](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515234158333.png)



## PathVaribale

![image-20200515234616666](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515234616666.png)

示例：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515234747352.png" alt="image-20200515235052778" style="zoom:80%;" />

![image-20200515234845316](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515234845316.png)

> 拓展：REST风格URL
>
> ![image-20200515234747352](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515235052778.png)



## RequestHeader

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515235538276.png" alt="image-20200515235538276" style="zoom:67%;" />

![image-20200515235731598](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515235731598.png)



## CookieValue

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515235819878.png" alt="image-20200515235819878" style="zoom:80%;" />

![image-20200515235948344](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200515235948344.png)

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516000027592.png" alt="image-20200516000027592" style="zoom:80%;" />



## ModelAttribute

![image-20200516000612310](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516000612310.png)

有返回值的情况：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516001623867.png" alt="image-20200516001623867" style="zoom:80%;" />

无返回值的情况：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516002423960.png" alt="image-20200516002833164" style="zoom:80%;" />



## SessionAttribute

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516002833164.png" alt="image-20200516002423960" style="zoom:80%;" />

只能作用在类上。

![image-20200516004136635](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516004136635.png)

存取到session域：

![image-20200516004227963](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516004227963.png)

读取session域中的值：

![image-20200516004318730](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516004318730.png)

删除session域中的值：

![image-20200516004408575](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516004408575.png)





# 响应数据和结果视图

## 响应数据的几种情况

1. 返回字符串

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516165250865.png" alt="image-20200516165250865" style="zoom:80%;" />

2. 返回值void

   如果控制器的方法返回值编写成void，执行程序报404的异常，默认查找JSP页面没有找到。

   默认会跳转到@RequestMapping(value="/initUpdate")  initUpdate的页面。

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516165641141.png" alt="image-20200516165641141" style="zoom:80%;" />

3. 返回值是ModelAndView对象

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516170355568.png" alt="image-20200516170355568"  />

## 转发和重定向

转发：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516170849928.png" alt="image-20200516170849928" style="zoom:80%;" />

重定向：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516170946464.png" alt="image-20200516170946464" style="zoom:80%;" />

## 响应json数据

![image-20200516173317936](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516173317936.png)

![image-20200516173648252](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516173648252.png)

![image-20200516173845123](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516173845123.png)

![image-20200516174025661](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516174025661.png)

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516174218856.png" alt="image-20200516174218856" style="zoom:80%;" />

![image-20200516174335423](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516174335423.png)

![image-20200516174415991](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516174415991.png)

# SpringMVC实现文件上传







# SpringMVC中的异常处理

## springMVC中异常处理的流程

系统的dao、service、controller出现都通过throws Exception向上抛出，最后由springmvc前端控制器交由异常处理器进行异常处理。

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516184710338.png" alt="image-20200516184710338" style="zoom:80%;" />

## 自定义异常处理

![image-20200516224757760](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516224757760.png)

1. 自定义异常类

   ```java
   //继承Java的异常类
   public class CustomException extends Exception { 
       private String message; 
       public CustomException(String message) { 
           this.message = message; 
       } 
       public String getMessage() { 
           return message; 
       } 
   }
   ```

2. 自定义异常处理器

   ```java
   //需要实现spring的HandlerExceptionResolver接口，并重写方法
   public class CustomExceptionResolver implements HandlerExceptionResolver {
       @Override
       public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) { 
           ex.printStackTrace(); 
           CustomException customException = null; 
           //如果抛出的是系统自定义异常则直接转换 
           if(ex instanceof CustomException){ 
               customException = (CustomException)ex; 
           }else{ 
               //如果抛出的不是系统自定义异常则重新构造一个系统错误异常。 
               customException = new CustomException("系统错误，请与系统管理 员联系！"); 
           } 
           ModelAndView modelAndView = new ModelAndView(); 			                    		modelAndView.addObject("message", customException.getMessage()); 			    		modelAndView.setViewName("error"); return modelAndView; 
       } 
   }
   ```

3. 配置异常处理器

   ```xml
   <!-- 配置自定义异常处理器 --> 
   <bean id="handlerExceptionResolver" class="com.itheima.exception.CustomExceptionResolver"/>
   ```

   



# SpringMVC的拦截器

## 拦截器的作用

Spring MVC 的处理器拦截器类似于Servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理。 用户可以自己定义一些拦截器来实现特定的功能。 谈到拦截器，还要向大家提一个词——拦截器链（Interceptor Chain）。拦截器链就是将拦截器按一定的顺序联结成一条链。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。说到这里，可能大家脑海中有了一个疑问，这不是我们之前学的过滤器吗？是的它和过滤器是有几分相似，但是也有区别，接下来我们就来说说他们的区别： 

过滤器是servlet规范中的一部分，任何java web工程都可以使用。 

拦截器是SpringMVC框架自己的，只有使用了SpringMVC框架的工程才能用。 

过滤器在url-pattern中配置了/*之后，可以对所有要访问的资源拦截。

 拦截器它是只会拦截访问的控制器方法，如果访问的是jsp，html,css,image或者js是不会进行拦截的。 它也是AOP思想的具体应用。 我们要想自定义拦截器， 要求必须实现：**HandlerInterceptor接口**。

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516230302296.png" alt="image-20200516230302296" style="zoom:80%;" />

## HandlerInterceptor接口中的方法

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200516231700472.png" alt="image-20200516231700472" style="zoom:80%;" />

## 设计拦截器

1. 编写拦截器器（实现HandlerInterceptor接口）

2. 配置拦截器

   ```xml
   <!-- 配置拦截器 -->
   <mvc:interceptors>
   	<mvc:interceptor>
   		<!-- 哪些方法进行拦截 -->
   		<mvc:mapping path="/user/*"/>
   		<!-- 哪些方法不进行拦截
   		<mvc:exclude-mapping path=""/>
   		<!-- 注册拦截器对象 -->
   		<bean class="cn.itcast.demo1.MyInterceptor1"/>
   	</mvc:interceptor>
     
       <!-- 设置第二个拦截器 -->
       <mvc:interceptor>
   		<!-- 哪些方法进行拦截 -->
   		<mvc:mapping path="/**"/>
   		<!-- 注册拦截器对象 -->
   		<bean class="cn.itcast.demo1.MyInterceptor2"/>
   	</mvc:interceptor>
   </mvc:interceptors>
   ```





# SSM框架整合

