# Spring MVC

Spring MVC是目前主流的实现 MVC 设计模式的框架，提供了前端路由映射、视图解析等功能，让 Web 开发变得更加简单，Spring MVC 是以 Spring IoC 容器为基础的，大大简化了它的配置，并且因为是 Spring 家族成员的一个组件，所以可以和 Spring Framework 无缝衔接，不存在整合的概念，使用起来非常方便，是 Java Web 开发者必须要掌握的框架。

## MVC设计模式

MVC模式的流程：Controller接收到客户端的请求，调用Model相关代码完成业务逻辑操作，获取业务数据并返回给Controller，Controller在结合View完成业务数据的视图层渲染，并将结果响应给客户端。

<img src="E:\我的坚果云\Java基础\images\MVC模式.jpg" alt="MVC模式图" style="zoom: 80%;" />

Spring MVC 对这套 MVC 流程进行了封装，帮助开发者屏蔽了底层代码，并开放出相关接口供开发者调用，让 MVC 开发变得更加简单快捷。

## Spring MVC的实现原理

### 核心组件

* **DispatcherServlet**：前端控制器，负责调度其他组件的执行，可降低不同组件之间的耦合性，是整个Spring MVC的核心模块。
* **Handler**：处理器，完成具体业务逻辑，相当于Servlet或Action。
* **HandlerMapping**：DispatcherServlet是通过HandlerMapping将请求映射到不同的Handler。
* **HandlerInterceptor**：处理器拦截器，是一个接口，如果我们需要做一些拦截处理，可以实现这个接口。
* **HandlerExecutionChain**：处理器执行链，包括两个部分，即Handler和HandlerInterceptor（系统会有一个默认的 HandlerInterceptor，如果需要额外拦截处理，可以添加拦截器设置）。
* **HandlerAdapter**：处理器适配器，Handler执行业务方法之前，需要进行一系列的操作包括表单数据的验证、数据类型的转换、将表单数据封装到POJO等，这一系列的操作，都是由HandlerAdapter来完成，DispatcherServlet通过HandlerAdapter执行不同的Handler。
* **ModelAndView**：视图解析器，DispatcherServlet通过它将将逻辑视图解析成物理视图，最终将渲染视图结果响应给客户端。

### 工作流程

1、客户端请求被DispatcherServlet（前端控制器）接收。

2、根据HandlerMapping映射到Handler。

3、生成Handler和HandlerInterceptor（如果有则生成）。

4、Handler 和 HandlerInterceptor 以 HandlerExecutionChain 的形式一并返回给DispatcherServlet。

5、DispatcherServlet 通过 HandlerAdapter 调用 Handler 的方法做业务逻辑处理。

6、返回一个ModelAndView 对象给DispatcherServlet。

7、DispatcherServlet 将获取的 ModelAndView 对象传给 ViewResolver 视图解析器，将逻辑视图解析成物理视图View。

8、ViewResolver 返回一个 View 给 DispatcherServlet。

9、DispatcherServlet 根据 View 进行视图渲染（将模型数据填充到视图中）。

10、DispatcherServlet 将渲染后的视图响应给客户端。

<img src="E:\我的坚果云\Java基础\images\SpringMVC工作流程.jpg" alt="Spring MVC工作流程" style="zoom: 50%;" />

## Spring MVC的使用

（1）环境搭建

创建 Maven 工程，创建 pom.xml 配置 Spring MVC 依赖。

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>4.3.1.RELEASE</version>
</dependency>
```

（2）在 web.xml 中配置 Spring MVC 的 DispatcherServlet。

```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
         <param-name>contextConfigLocation</param-name>
         <!-- 指定springmvc.xml的路径 -->
         <param-value>classpath:springmvc.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping> 
<!--乱码过滤器-->
<filter>  
    <filter-name>encodingFilter</filter-name>  
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
    <init-param>  
        <param-name>encoding</param-name>  
        <param-value>UTF-8</param-value>  
    </init-param>  
</filter>  
<filter-mapping>  
    <filter-name>encodingFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>
```

（3）创建 springmvc.xml 配置文件。

```xml
<!-- 配置自动扫描 -->
<context:component-scan base-package="com.southwind.handler"></context:component-scan>
<!-- 配置视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!-- 前缀 -->
    <property name="prefix" value="/"></property>
    <!-- 后缀 -->
    <property name="suffix" value=".jsp"></property>
</bean>
```

（4）创建 Handler 类。

```java
@Controller
public class HelloHandler {

    @RequestMapping("/index")
    public String index(){
        System.out.println("执行index业务代码");
        return "index";
    }

}
```

（5）启动 Tomcat，打开浏览器输入 URL 进行测试。

## Spring MVC的常用注解

#### @RequestMapping

Spring MVC通过@RequestMapping注解将URL请求与业务方法进行映射，在控制器的类定义处以及方法定义处都可添加@RequestMapping，在类定义处添加@RequestMapping注解，相当于多了一层访问路径。

```java
@Controller
@RequestMapping("/helloHandler")
public class HelloHandler {
     @RequestMapping(value="hello")
      public String hello(){
          System.out.println("hello world");
          return "index";
      }
}
```

**@RequestMapping常用参数**

（1）value：指定URL请求的实际地址，是@RequestMapping的默认值

（2）method：指定请求的method类型，包括GET、POST、PUT、DELETE等

```java
@RequestMapping(value="/postTest",method=RequestMethod.POST)
public String postTest(){
    System.out.println("postTest");
    return "index";
}
```

（3）params：指定request中必须包含的参数值，否则无法调用该方法。

```java
@RequestMapping(value="paramsTest",params={"name","id=10"})
public String paramsTest(){
    System.out.println("paramsTest");
    return "index";
}
```

URL 请求中必须包含 name 和 id 两个参数，并且 id 的值必须为 10，才能调用 paramsTest 方法。

#### 参数绑定

（1）在业务方法定义时声明参数列表；

（2）给参数列表添加 @RequestParam 注解。

```java
@RequestMapping(value="paramsBind")
public String paramsBind(@RequestParam("name") String name,@RequestParam("id") int id){
    System.out.println(name);
    int num = id+10;
    System.out.println(num);
    return "index";
}
```

将 URL 请求的参数 name 和 id 分别映射给形参 name 和 id，同时进行了数据类型的转换，URL 参数都是 String 
类型的，Spring MVC 可以自动根据形参的数据类型完成数据类型转换，比如将 id 转换为 int 类型，因此可以看到打印的 num 值为 20，完成了数学运算，说明已经进行了数据类型转换，具体的数据类型转换工作是由 HandlerAdapter 来完成的。

#### Spring MVC也支持RESTful风格的URL参数获取

```java
@RequestMapping(value="rest/{name}")
public String restTest(@PathVariable("name") String name){
    System.out.println(name);
    return "index";
}
```

将参数列表的注解改为 @PathVariable("name") 即可。

#### 映射Cookie

spring MVC通过映射可以直接在业务方法中获取Cookie值

```java
@RequestMapping("/cookieTest")
public String getCookie(@CookieValue(value="JSESSIONID") String sessionId){
    System.out.println(sessionId);
    return "index";
}
```

#### 使用POJO绑定参数

Spring MVC 会根据请求参数名和 POJO 属性名进行匹配，自动为该对象填充属性值，并且支持属性级联，具体操作如下所示。

（1）创建实体类 Address、User 并进行级联设置

```java
public class Address {
    private int id;
    private String name;
}

public class User {
    private int id;
    private String name;
    private Address address;
}
```

（2）创建 addUser.jsp

```html
<form action="addUser" method="post">
    编号：<input type="text" name="id"/><br/>
    姓名：<input type="text" name="name"/><br/>
    地址：<input type="text" name="address.name"/><br/>
    <input type="submit" value="提交"/>
</form>
```

（3）业务方法

```java
@RequestMapping("/addUser")
public String getPOJO(User user){
    System.out.println(user);
    return "index";
}
```

（4）运行localhost:8080/addUser.jsp

	#### JSP 页面的转发和重定向

重定向：地址发生改变

```java
@RequestMapping("redirectTest")
public String redirectTest(){
    return "redirect:/index.jsp";
}
```

转发：地址不改变

```java
@RequestMapping("forwardTest")
public String forwardTest(){
    return "forward:/index.jsp";
}
```



