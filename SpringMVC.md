# MVC

**Model（模型）**：数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作

**最典型的MVC就是JSP + servlet + javabean的模式。**

![img](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3S3dQT1BXcTAwcE1KaWFLODZsRjZCaklYVzdXbW05S1ZFVjFGWFVmSk1EMEt6dVlaN2ljNVVIZ2dzWkRBenlZeXJkNHBMdm5CSVZNNXpBLzY0MA)

前端 数据传输 实体类

实体类：用户名，，密码，生日，爱好，......

前端：用户名 密码

pojo:数据库实体类，即针对数据库而言的

vo:UserVo,视图层对象，对实体类细分，即用来接收前端数据的

dto:数据传输的对象



JSP：本质是一个Servelet

# Spring MVC

Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

Spring MVC框架像许多其他MVC框架一样, 以请求为驱动 , 围绕一个中心Servlet分派请求及提供其他功能，DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)。

DispatcherServlet

![image-20210520105514247](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210520105514247.png)

第一个SpringMVC程序

![image-20210520131615074](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210520131615074.png)

controller

```java
package com.leo.controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


//注意：这里我们先导入Controller接口
public class HelloController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中。Model
        mv.addObject("msg","HelloSpringMVC!");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello"); //由于我们已经在Springmvc-servelet里面配置了路径，所以他会自动拼接: /WEB-INF/jsp/hello.jsp
        return mv;
    }

}
```

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <!--注册bean并且配置路径配置路径-->
    <bean id="/hello" class="com.leo.controller.HelloController"/>
</beans>
```

hello.jsp

```java
<%--
  Created by IntelliJ IDEA.
  User: Lenovo
  Date: 2021/5/20
  Time: 12:58
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--/ 匹配所有的请求；（不包括.jsp）-->
    <!--/* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

注意：如果代码没问题但仍然报404，则检查下是否给服务器导入了依赖![image-20210520132027310](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210520132027310.png)

如果没有

首先在class同级目录下新建lib

![image-20210520132144777](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210520132144777.png)

然后

![image-20210520132238745](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210520132238745.png)



![image-20210520132304139](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210520132304139.png)

选择所有依赖配置到lib中再运行就没问题了

# SpringMVC原理

![image-20210521165205678](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521165205678.png)

首先由于在servlet-mapping中我们对DispatcherServlet匹配的是`/`，

![image-20210521165248186](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521165248186.png)

故我们访问的时候会首先访问DispatcherServlet

由于我们添加了

![image-20210521165224756](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521165224756.png)

故DispatcherServlet会到resources目录下寻找springMVC-servlet.xml作为它的配置文件

然后DispatcherServlet调用BeanNameUrlHandlerMapping找寻以`/`为开头的bean id

![image-20210521165946477](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521165946477.png)

从而找到Controller

![image-20210521170853566](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521170853566.png)

然后DispatcherServlet调用SimpleControllerHandlerAdapter进而调用Controller，通过断点测试我们发现，在执行return后

![image-20210521171130947](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521171130947.png)

会由Controller返回跳转至SimpleControllerHandlerAdapter

![image-20210521171245905](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521171245905.png)

然后SimpleControllerHandlerAdapter将mv传递给InternalResourceViewResolver对视图进行拼接，最终由DispatcherServlet根据视图的全路径调用视图

**提醒：上面是我结合课程，网上找的资料，以及自己打断点得出的，肯定不准确，以后分析源码的时候我再修改**

# 注解开发

配置web.xml,即注册DispatcherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:Springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--/ 匹配所有的请求；（不包括.jsp）-->
    <!--/* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

创建视图

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Lenovo
  Date: 2021/5/21
  Time: 21:23
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>
```

创建Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.leo.controller"/>
    <!--过滤静态资源,.css,.js,.html,.mp3,.mp4-->
    <mvc:default-servlet-handler/>
    <!--开启注解支持-->
    <mvc:annotation-driven/>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

注意，`<mvc:annotation-driven/>`起到了

```xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```

这两项的作用

```java
package com.leo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {
    @RequestMapping("/h1")
    public String Hello(Model model){
        model.addAttribute("msg","Hello,SpringMVCAnnotation");
        return "hello";
    }
}
```

`@RequestMapping("/h1")`

则起到了

```xml
<!--注册bean并且配置路径配置路径-->
<bean id="/hello" class="com.leo.controller.HelloController"/>
```

的作用

@Controller则是实现了Controller接口的作用，同时注册bean

![image-20210521220154028](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521220154028.png)

# @Controller

@Controller

代表这个类会被Spring接管

被这个注解的类中的所有方法，如果返回值是String，并且有具体页面可以跳转，那么就会被视图解析器解析

# @RequestMapping

如果需要多层url比如：/c1/h1,/c2/h1,c1/h2,c2/h2可以采用一下方法

![image-20210521230636420](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521230636420.png)

外层的为父类，内层为子类

当然如果项目非常大，则父类url会很难找，可以采用绝对url

即

![image-20210521230837489](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210521230837489.png)

# RestFul风格URL

## 传统的结构

```java
@Controller
public class RestFulController {
    @RequestMapping(value = "/test",method = RequestMethod.GET)
    //可以简化为
    //@RequestMapping("/test")
    public String test1(int a, int b, Model model){
        int res=a+b;
        model.addAttribute("msg","结果1为"+res);
        return "hello";
    }
}
```

![image-20210522002450735](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522002450735.png)

## RestFul风格

```java
@Controller
public class RestFulController {
    @RequestMapping(value = "/test/{a}/{b}",method = RequestMethod.GET)
    //@PathVariable 标记的变量名可以在@RequestMapping的{a}({b})中取到
    public String test1(@PathVariable int a,@PathVariable int b, Model model){
        int res=a+b;
        model.addAttribute("msg","结果1为"+res);
        return "hello";
    }
}
```

![image-20210522002853025](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522002853025.png)

method方法很多，总共有

![image-20210522003218614](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522003218614.png)

对于采用什么方式，除了`method =` 的方式外，我们更经常用

```java
@GetMapping
@PostMapping
```

如果有两个同路径但是method不同的Controller，会自动匹配对应method的方法

```java
@Controller
public class RestFulController {
    //@RequestMapping(value = "/test/{a}/{b}",method = RequestMethod.GET)
    @GetMapping("/test/{a}/{b}")
    public String test1(@PathVariable int a,@PathVariable int b, Model model){
        int res=a+b;
        model.addAttribute("msg","结果1为"+res);
        return "hello";
    }
    @PostMapping("/test/{a}/{b}")
    public String test2(@PathVariable int a,@PathVariable int b, Model model){
        int res=a+b;
        model.addAttribute("msg","结果2为"+res);
        return "hello";
    }
}
```

在这里面，如果我们用get走`/test/1/1`则会调用test1，用post则会调用test2

## 使用路径变量的好处？

- 使路径变得更加简洁；
- 获得参数更加方便，框架会自动进行类型转换。
- 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

# 重定向和转发

由于SpringMVC底层仍然是Servlet，故我们可以

## 直接使用Servlet的方式做重定向和转发

转发

```java
@RequestMapping("/m1/t1")
public void test1(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    HttpSession session=request.getSession();
    System.out.println(session.getId());
    request.getRequestDispatcher("/WEB-INF/jsp/hello.jsp").forward(request,response);
}
```

重定向

```java
    @RequestMapping("/m1/t1")
    public void test1(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        HttpSession session=request.getSession();
        System.out.println(session.getId());
        response.sendRedirect("/SpringMVC03annotation_war_exploded/index.jsp");
    }
```

## 用SpringMVC方式做重定向和转发

重定向

```java
@RequestMapping("/m1/t1")
public String test1(){
    return"redirect:/index.jsp";
}
```

视图解析器会对redirect进行过滤，即视图解析器不对redirect起作用

转发

转发会走视图解析器

默认情况下return就会是转发

```java
@RequestMapping("/m1/t1")
public String test1(){
    return"hello";
}
```

注意不要用`forward:hello`,因为转发会走视图解析器，最后会拼接出![image-20210522113544231](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522113544231.png)

# 接收请求参数及数据回显

## 接收前端数据

接收的变量名和前端一致

```java
@GetMapping("/u1/t1")
public String test1(String name, Model model){
    //1.接收前端参数
    System.out.println("接收到前端的数据为:"+name);
    //2.将返回的结果传递给前端
    model.addAttribute("msg",name);
    // 视图跳转
    return "hello";
}
```

我们推荐使用@RequestParam(前端取的变量名)标记后使用

```java
@GetMapping("/u1/t1")
public String test1(@RequestParam("username")String name, Model model){
    //1.接收前端参数
    System.out.println("接收到前端的数据为:"+name);
    //2.将返回的结果传递给前端
    model.addAttribute("msg",name);
    //视图跳转
    return "hello";
}
```

也可以直接接收对象，要保证实体类和传参严格一致，否则接收到的为null

```java
//前端接收的是一个对象：id,name,age
@GetMapping("/u1/t2")
public  String test2(User user){
    System.out.println(user);
    return "hello";
}
```

## 传输数据到前端

**通过ModelAndView**

这个对象可以同时存Model和View

```java
public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
    //返回一个模型视图对象
    ModelAndView mv = new ModelAndView();
    mv.addObject("msg","ControllerTest1");
    mv.setViewName("test");
    return mv;
}
```

通过ModelMap，ModelMap继承了LinkedHashMap,其拥有LinkedHashMap的所有方法

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name);
    model.addAttribute("name",name);
    System.out.println(name);
    return "hello";
}
```

通过Model，Model是简化的ModelMap，方法比较少，但是在实际开发中很有用

```java
@RequestMapping("/ct2/hello")
public String hello(@RequestParam("username") String name, Model model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name);
    model.addAttribute("msg",name);
    System.out.println(name);
    return "test";
}
```

# 过滤乱码

编写一个简单的form由前端传参到后端

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="/SpringMVC03annotation_war_exploded/e/t1" method="post">
    <input type="text" name="username"/>
    <input type="submit"/>
</form>
</body>
</html>
```

```java
@Controller
public class EncodingController {
    @PostMapping("/e/t1")
    public String test1(@RequestParam("username") String username, Model model){
        model.addAttribute("msg",username);
        return "hello";
    }
}
```

会发现出现了乱码

![image-20210522152315779](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522152315779.png)

![image-20210522152326572](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522152326572.png)

我们尝试

```java
@PostMapping("/e/t1")
public String test1(@RequestParam("username") String username, Model model, HttpServletRequest req,HttpServletRequest res) throws UnsupportedEncodingException {
    model.addAttribute("msg",username);
    req.setCharacterEncoding("utf-8");
    res.setCharacterEncoding("utf-8");
    return "hello";
}
```

仍然是乱码，说明在前端传输的过程中出现了问题

## 自定义filter解决乱码

我们再尝试利用filter，因为filter在服务器启动的时候就会发挥作用，对Servlet进行过滤

创建filter

```java
public class EncodingFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");
        filterChain.doFilter(servletRequest,servletResponse);
    }

    public void destroy() {

    }
}
```

配置filter

```xml
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>com.leo.fliter.EncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

注意用`/*`而不是`/`，`/`会过滤掉.jsp，所以需要/*进行匹配

## Spring自带过滤器解决乱码

只需要配置web.xml就行

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 某大神的自定义filter（必然不是我）

```java
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Map;
 
/**
 * 解决get和post请求 全部乱码的过滤器
 */
public class GenericEncodingFilter implements Filter {
 
    @Override
    public void destroy() {
    }
 
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        //处理response的字符编码
        HttpServletResponse myResponse=(HttpServletResponse) response;
        myResponse.setContentType("text/html;charset=UTF-8");
 
        // 转型为与协议相关对象
        HttpServletRequest httpServletRequest = (HttpServletRequest) request;
        // 对request包装增强
        HttpServletRequest myrequest = new MyRequest(httpServletRequest);
        chain.doFilter(myrequest, response);
    }
 
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }
 
}
 
//自定义request对象，HttpServletRequest的包装类
class MyRequest extends HttpServletRequestWrapper {
 
    private HttpServletRequest request;
    //是否编码的标记
    private boolean hasEncode;
    //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
    public MyRequest(HttpServletRequest request) {
        super(request);// super必须写
        this.request = request;
    }
 
    // 对需要增强方法 进行覆盖
    @Override
    public Map getParameterMap() {
        // 先获得请求方式
        String method = request.getMethod();
        if (method.equalsIgnoreCase("post")) {
            // post请求
            try {
                // 处理post乱码
                request.setCharacterEncoding("utf-8");
                return request.getParameterMap();
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
        } else if (method.equalsIgnoreCase("get")) {
            // get请求
            Map<String, String[]> parameterMap = request.getParameterMap();
            if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                for (String parameterName : parameterMap.keySet()) {
                    String[] values = parameterMap.get(parameterName);
                    if (values != null) {
                        for (int i = 0; i < values.length; i++) {
                            try {
                                // 处理get乱码
                                values[i] = new String(values[i]
                                        .getBytes("ISO-8859-1"), "utf-8");
                            } catch (UnsupportedEncodingException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                hasEncode = true;
            }
            return parameterMap;
        }
        return super.getParameterMap();
    }
 
    //取一个值
    @Override
    public String getParameter(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        if (values == null) {
            return null;
        }
        return values[0]; // 取回参数的第一个值
    }
 
    //取所有值
    @Override
    public String[] getParameterValues(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        return values;
    }
}
```

# Jackson



## json返回一个实体类

创建实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private int age;
}
```

Controller

```java
@Controller
public class UserController{
    @RequestMapping("/j1")
     //    不走视图构造器
    @ResponseBody
    public String json1() throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();
        User user = new User(1, "牛牛", 1);
        String s = objectMapper.writeValueAsString(user);
        return s;
    }
}
```

虽然我们开启了过滤器，但是仍然出现了乱码

![image-20210522181454256](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522181454256.png)

我们可以通过注解解决

![image-20210522181814296](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522181814296-1621678779768.png)

修改注解

```java
@RequestMapping(value = "/j1",produces = "application/json;charset=utf-8")
@ResponseBody
```

变为正常

![image-20210522182022214](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522182022214.png)

但是一个Request设置一次太麻烦了

我们可以在Spring配置文件中配置

```xml
<!--开启注解支持-->
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg name="defaultCharset" value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

恢复正常，但是前端展示字变粗了，目前不知道为什么

![image-20210522181404232](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522181404232-1621678764656.png)

## @ResponseBody和@RestController

```java
@RestController
```

和

```java
@Controller
@ResponseBody
```

共同作用的效果一样，都是去掉视图解析器

## json返回一个实体类列表

```java
//    返回一个集合
    @RequestMapping( "/j2")
    public String json2() throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();
        User user1 = new User(1, "牛牛", 1);
        User user2 = new User(1, "牛牛", 1);
        User user3= new User(1, "牛牛", 1);
        User user4 = new User(1, "牛牛", 1);
        User user5 = new User(1, "牛牛", 1);
        ArrayList<User> users = new ArrayList<User>();
        users.add(user1);
        users.add(user2);
        users.add(user3);
        users.add(user4);
        users.add(user5);
        String s = objectMapper.writeValueAsString(users);
        return s;
    }
```

## json传时间并格式化为yyyy-MM-dd HH-mm-ss

### 利用纯java的方式

```java
@RequestMapping( "/j3")
public String json3() throws JsonProcessingException {
    ObjectMapper objectMapper = new ObjectMapper();
    Date date = new Date();
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    String s = objectMapper.writeValueAsString(simpleDateFormat.format(date));
    return s;
}
```

![image-20210522184912834](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210522184912834.png)

### 利用jackson自带的方式

```
    @RequestMapping( "/j3")
    public String json3() throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();
//        不使用时间戳的方式
        objectMapper.configure(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS,false);
        Date date = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        objectMapper.setDateFormat(simpleDateFormat);
        String s = objectMapper.writeValueAsString(date);
        return s;
    }
```

### 建工具类

```java
public class JsonUtil {
    public static String getJason(Object object,String dateFormat) throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();
        //        不使用时间戳的方式
        objectMapper.configure(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS,false);
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(dateFormat);
        objectMapper.setDateFormat(simpleDateFormat);
        return objectMapper.writeValueAsString(object);
    }
}
```

对于工具类，我们可以让其使用范围扩大到所有类获取json而不是仅仅Date类

```java
public class JsonUtil {
    public static String getJson(Object object) throws JsonProcessingException {
        //空字符串是因为getJson要传两个参数
        return getJson(object,"");
    }
    public static String getJson(Object object,String dateFormat) throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();
        //        不使用时间戳的方式
        objectMapper.configure(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS,false);
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(dateFormat);
        objectMapper.setDateFormat(simpleDateFormat);
        return objectMapper.writeValueAsString(object);
    }
}
```

注意其中的函数复用思想，我们重载了getJson函数，同时在重载函数中通过传一个空字符串来实现了原函数的复用，这种思想在很多源码里经常见。

利用工具类，我们能更简单地实现上面的几种类型Json的生成

```java
@RequestMapping( "/j4")
public String json4() throws JsonProcessingException {
    Date date = new Date();
    return JsonUtil.getJson(date,"yyyy-MM-dd HH:mm:ss");
}
@RequestMapping("/j5")
public String json5() throws JsonProcessingException {
    User user = new User(1, "牛牛", 1);
    return JsonUtil.getJson(user);
}
@RequestMapping( "/j6")
public String json6() throws JsonProcessingException {
    User user1 = new User(1, "牛牛", 1);
    User user2 = new User(1, "牛牛", 1);
    User user3= new User(1, "牛牛", 1);
    User user4 = new User(1, "牛牛", 1);
    User user5 = new User(1, "牛牛", 1);
    ArrayList<User> users = new ArrayList<User>();
    users.add(user1);
    users.add(user2);
    users.add(user3);
    users.add(user4);
    users.add(user5);
    return JsonUtil.getJson(users);
}
```

# FastJson

fastjson 的 pom依赖！

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```

fastjson 三个主要的类：

JSONObject  代表 json 对象

+ JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的。

+ JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用诸如size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的。

JSONArray   代表 json 对象数组

+ 内部是有List接口中的方法来完成操作的。

JSON代表 JSONObject和JSONArray的转化

+ JSON类源码分析与使用

+ 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化。

代码测试，我们新建一个FastJsonDemo 类

```java
	System.out.println("*******Java对象 转 JSON字符串*******");
    String str1 = JSON.toJSONString(list);
    System.out.println("JSON.toJSONString(list)==>"+str1);
    String str2 = JSON.toJSONString(user1);
    System.out.println("JSON.toJSONString(user1)==>"+str2);
 
    System.out.println("\n****** JSON字符串 转 Java对象*******");
    User jp_user1=JSON.parseObject(str2,User.class);
    System.out.println("JSON.parseObject(str2,User.class)==>"+jp_user1);
 
    System.out.println("\n****** Java对象 转 JSON对象 ******");
    JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
    System.out.println("(JSONObject) JSON.toJSON(user2)==>"+jsonObject1.getString("name"));
 
    System.out.println("\n****** JSON对象 转 Java对象 ******");
    User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
    System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>"+to_java_user);

```

这种工具类，我们只需要掌握使用就好了，在使用的时候在根据具体的业务去找对应的实现。和以前的commons-io那种工具包一样，拿来用就好了！

Json在我们数据传输中十分重要，一定要学会使用！

# 做一个图书管理系统（大量代码）

## Controller

### BookController

```java
@Controller
@RequestMapping("/book")
public class BookController {
    @Autowired
    @Qualifier("BookServiceImpl")
    private BookService bookService;

    @RequestMapping("/allBook")
    public String list(Model model){
        List<Books> books = bookService.queryAllBook();
        model.addAttribute("list",books);
        return "allBook";
    }
    @RequestMapping("/toAddPaper")
    public String toAddPaper(){
        return "addBooks";
    }
    @RequestMapping("/addBook")
    public String addBook(Books books){
        bookService.addBook(books);
//        加了redirect会走java代码的@RequestMapping("/allBook")
//        不加则会直接走allbook.jsp，这时需要传list给前端，比如最后一个搜索操作
        return "redirect:/book/allBook";
    }
    @RequestMapping("/toUpdatePaper")
    public String toUpdatePaper(int id,Model model){
        Books books = bookService.queryBookById(id);
        model.addAttribute("QBook",books);
        return "updateBook";
    }
    @RequestMapping("/updateBook")
    public String updateBook(Books books){
        System.out.println("updateBook=>"+books);
        bookService.updateBook(books);
        return "redirect:/book/allBook";
    }
    @RequestMapping("/deleteBook/{bookID}")
    public String deleteBook(@PathVariable("bookID") int id){
        bookService.deleteBookById(id);
        return "redirect:/book/allBook";
    }
    @RequestMapping("/queryBook")
    public String queryBook(String bookName,Model model){
        List<Books> list = bookService.queryBookByName(bookName);
        if(list.isEmpty()){
            list= (ArrayList<Books>) bookService.queryAllBook();
            model.addAttribute("error","未查到");
        }
        model.addAttribute("list",list);
        return "allBook";
    }
}
```

## Dao

### BookMapper

```java
public interface BookMapper {
//    增加一本书
    void addBook(Books books);
//    删除一本书
    void deleteBookById(@Param("bookID") int id);
//    更新一本书
    void updateBook(Books books);
//    查询一本书
    Books queryBookById(@Param("bookID") int id);
//    查询全部的书
    List<Books> queryAllBook();
//    根据书名查书
    List<Books> queryBookByName(String bookName);
}
```

### BookMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.leo.dao.BookMapper">
    <insert id="addBook" parameterType="Books">
        insert into ssmbuild.books (bookName, bookCounts, detail) VALUES (#{bookName},#{bookCounts},#{detail})
    </insert>
    <delete id="deleteBookById" parameterType="int">
        delete from ssmbuild.books where bookID=#{bookID}
    </delete>
    <update id="updateBook" parameterType="Books">
        update ssmbuild.books set bookName=#{bookName},bookCounts=#{bookCounts},detail=#{detail}
        where bookID=#{bookID}
    </update>
    <select id="queryBookById" parameterType="int" resultType="Books">
        select * from ssmbuild.books where bookID=#{bookID}
    </select>
    <select id="queryAllBook" resultType="Books">
        select * from ssmbuild.books
    </select>
    <select id="queryBookByName" resultType="Books">
        select * from ssmbuild.books where bookName like concat('%',#{bookName},'%')
    </select>
</mapper>
```

## pojo

### Books

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Books {
    private int bookID;
    private String bookName;
    private int bookCounts;
    private String detail;
}
```

## Service

### BookService

```java
public interface BookService {
    //    增加一本书
    void addBook(Books books);
    //    删除一本书
    void deleteBookById(@Param("bookID") int id);
    //    更新一本书
    void updateBook(@Param("books") Books books);
    //    查询一本书
    Books queryBookById(@Param("bookID") int id);
    //    查询全部的书
    List<Books> queryAllBook();
    //    根据书名查书
    List<Books> queryBookByName(String bookName);
}
```

### BookServiceImpl

```java
@Data
public class BookServiceImpl implements BookService {
    private BookMapper bookMapper;
    public void addBook(Books books) {
        bookMapper.addBook(books);
    }

    public void deleteBookById(int id) {
        bookMapper.deleteBookById(id);
    }

    public void updateBook(Books books) {
        bookMapper.updateBook(books);
    }

    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }

    public List<Books> queryBookByName(String bookName) {
        return bookMapper.queryBookByName(bookName);
    }
```

## Resources

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="Spring-Dao.xml"/>
    <import resource="Spring-MVC.xml"/>
    <import resource="Spring-Service.xml"/>
</beans>
```

### databases.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=False&useUnicode=true&characterEncoding=utf-8&servetTimezone=UTC
jdbc.username=root
jdbc.password=###164
```

### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置数据源，交给Spring去做-->
    <!--取别名-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <typeAliases>
        <!--<package name="com.leo.pojo.Books"/>-->
        <typeAlias type="com.leo.pojo.Books" alias="Books"/>
    </typeAliases>

</configuration>
```

### Spring-Dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--关联数据库配置文件-->
    <context:property-placeholder location="classpath:databases.properties"/>
    <!--连接池
        dbcp:半自动化操作，不能自动连接
        c3p0:自动化操作（自动化的加载配置文件，并且可以自动设置到对象中）
        druid:hikari
    -->
    <!--c3p0连接池的私有属性-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <property name="autoCommitOnClose" value="false"/>
        <property name="checkoutTimeout" value="10000"/>
        <property name="acquireRetryAttempts" value="2"/>
    </bean>
    <!--sqlSessionFactory-->
    <bean name="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <!--<property name="mapperLocations" value="classpath:com/leo/dao/*.xml"/>-->
    </bean>
    <!--配置dao接口扫描包，动态的实现了Dao接口可以注入到Spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <property name="basePackage" value="com.leo.dao"/>
    </bean>
</beans>
```

### Spring-MVC.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--注解驱动-->
    <mvc:annotation-driven/>
    <!--静态资源过滤-->
    <mvc:default-servlet-handler/>
    <context:component-scan base-package="com.leo.controller"/>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="suffix" value=".jsp"/>
        <property name="prefix" value="/WEB-INF/jsp/"/>
    </bean>
</beans>
```

### Spring-Service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
    <context:component-scan base-package="com.leo.service"/>
    <!--将我们所有的业务类，注入到Spring，可以通过配置，或者注解实现-->
    <bean id="BookServiceImpl" class="com.leo.service.BookServiceImpl">
        <property name="bookMapper" ref="bookMapper"/>
    </bean>
    <!--声明式事务配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <aop:config>
        <aop:pointcut id="pointcut" expression="execution(* com.leo.dao.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```

## Jsp

### addBooks.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>添加书籍</title>
    <link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>新增书籍</small>
                </h1>
            </div>
        </div>
    </div>
    <form action="${pageContext.request.contextPath}/book/addBook" method="post">
        <div class="form-group">
            <label>书籍名称：</label>
            <input type="text"  name="bookName" class="form-control" required>
        </div>
        <div class="form-group">
            <label>书籍数量：</label>
            <input type="text" name="bookCounts" class="form-control" required>
        </div>
        <div class="form-group">
            <label>书籍描述：</label>
            <input type="text" name="detail" class="form-control" required>
        </div>
        <div class="form-group">
            <input type="submit" class="form-control" value="添加" required>
        </div>
    </form>
</div>
</body>
</html>
```

### allBook.jsp

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%--
  Created by IntelliJ IDEA.
  User: Lenovo
  Date: 2021/5/24
  Time: 16:28
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>书籍展示</title>
    <link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>书籍列表-----显示所有书籍</small>
                </h1>
            </div>
        </div>
        <div class="row">
            <div class="col-md-4 column">
                <a  class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddPaper">新增书籍</a>
            </div>
            <div class="col-md-4 column">
                <a  class="btn btn-primary" href="${pageContext.request.contextPath}/book/allBook">查询全部书籍</a>
            </div>
            <div class="col-md-4 column">
               <form action="${pageContext.request.contextPath}/book/queryBook" method="post" class="form-inline">
                   <span style="color:red;font-weight: bold">${error}</span>
                   <input type="text" name="bookName" class="form-control" placeholder="请输入要查询的书籍名称">
                   <input type="submit" value="查询" class="btn btn-primary">
               </form>
            </div>
        </div>
    </div>
    <div class="row clearfix">
        <div class="col-md-12 column">
            <table class="table table-hover table-striped">
                <thead>
                    <tr>
                        <th>书籍编号</th>
                        <th>书籍名称</th>
                        <th>书籍数量</th>
                        <th>书籍详情</th>
                    </tr>
                </thead>
                <tbody>
                    <c:forEach var="book" items="${list}">
                        <tr>
                            <td>${book.bookID}</td>
                            <td>${book.bookName}</td>
                            <td>${book.bookCounts}</td>
                            <td>${book.detail}</td>
                            <td>
                                <a href="${pageContext.request.contextPath}/book/toUpdatePaper?id=${book.bookID}">修改</a>
                                &nbsp;|&nbsp;
                                <a href="${pageContext.request.contextPath}/book/deleteBook/${book.bookID}">删除</a>
                            </td>
                        </tr>
                    </c:forEach>
                </tbody>
            </table>
        </div>
    </div>
</div>
</body>
</html>
```

### updateBook.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>修改书籍</title>
    <link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>修改书籍</small>
                </h1>
            </div>
        </div>
    </div>
    <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
        <input type="hidden" name="bookID" value="${QBook.bookID}">
        <div class="form-group">
            <label>书籍名称：</label>
            <input type="text"  name="bookName" class="form-control"  value="${QBook.bookName}" required>
        </div>
        <div class="form-group">
            <label>书籍数量：</label>
            <input type="text" name="bookCounts" class="form-control" value="${QBook.bookCounts}" required>
        </div>
        <div class="form-group">
            <label>书籍描述：</label>
            <input type="text" name="detail" class="form-control" value="${QBook.detail}" required>
        </div>
        <div class="form-group">
            <input type="submit" class="form-control" value="修改" required>
        </div>
    </form>
</div>
</body>
</html>
```

## web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>
</web-app>
```

## Index.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Lenovo
  Date: 2021/5/24
  Time: 15:16
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>首页</title>
    <style>
      a{
        text-decoration: none;
        color: black;
        font-size: 18px;
      }
      h3{
        width: 180px;
        height: 38px;
        margin: 100px auto;
        text-align: center;
        line-height: 38px;
        background-color: deepskyblue;
        border-radius: 5px;
      }
    </style>
  </head>
  <body>
  <h3>
    <a href="${pageContext.request.contextPath}/book/allBook">进入书籍页面</a>
  </h3>
  </body>
</html>
```

## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leo</groupId>
    <artifactId>ssmbuild</artifactId>
    <version>1.0-SNAPSHOT</version>
<!--依赖，junit，数据库驱动,连接池，Servlet，jsp，mybatis，mybatis-Spring，Spring-->
<dependencies>
    <!--Junit，@Test用来测试-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    <!--数据库驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
    <!--数据库连接池-->
    <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
    <dependency>
        <groupId>com.mchange</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.5.2</version>
    </dependency>
    <!--servlet和jsp-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp.jstl</groupId>
        <artifactId>jstl-api</artifactId>
        <version>1.2</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.3</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.5</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.6</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.6</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.16</version>
        <scope>compile</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/taglibs/standard -->
    <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>
</dependencies>
    <!--静态资源导出-->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
</project>
```

# 关于Ajax

**注意ajax已经是前端的知识了，目前我还不太会，所以可能会有问题。**

## iframe 模拟ajax

首先我们用iframe模拟Ajax的无刷新操作

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>iframe测试体验页面无刷新</title>
    <script>
        function go() {
            var url=document.getElementById("url").value;
            document.getElementById("iframe1").src=url;
        }
    </script>
</head>
<body>
<div>
    <p>请输入地址</p>
    <p>
        <input type="text" id="url">
        <input type="button" value="提交" onclick="go()">
    </p>
</div>
<div>
    <iframe id="iframe1" style="width: 100%" height="500px" ></iframe>
</div>
</body>
</html>
```

![image-20210526161753137](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210526161753137.png)

## ajax

关注点转移的时候，会发送一个请求

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
    <script>
      function a(){
        $.post({
          url:"${pageContext.request.contextPath}/a1",
          data:{"name":$("#username").val()},
          success:function (data) {
            alert(data);
          }
        })
      }
    </script>
  </head>
  <body>
  <input type="text" id="username" onblur="a()">
  </body>
</html>
```

剩余内容没弄明白前端没法搞，我会在前端学完之后再补完这一部分。

# 拦截器

拦截器采用标准的AOP操作，我们这里给一个实例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--扫描包-->
    <context:component-scan base-package="com.leo.controller"/>
    <!--开启注解-->
    <mvc:annotation-driven/>
    <!--静态资源过滤-->
    <mvc:default-servlet-handler/>
    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.leo.config.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
</beans>
```

首先配置interceptor

创建类实现HandlerInterceptor接口

```java
public class MyInterceptor implements HandlerInterceptor{
//    return true;执行下一个拦截器，放行
//    return false;不执行下一个拦截器
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("=======拦截前===========");
        return true;
    }
//     后面两个方法用来加日志
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("=======处理后===========");
    }

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("=======清理===========");
    }
}
```

执行结果

![image-20210527134944251](C:\Users\Lenovo\Desktop\笔记\SpringMVC.assets\image-20210527134944251.png)

注意，一般我们只用preHandle用来进行执行Controller之前的执行，return true则通过拦截器，return false则不能通过拦截器，我们可以利用HttpServletRequest转发或者HttpServletResponse重定向到其他页面，一般是首页。

# 登录验证判断即做一个类似token保持登录状态的功能

原理是利用拦截器，我们这里在session中存一个字符串userLoginInfo来模拟token，对于每一个请求，我们先让它过拦截器，如果Session中userLoginInfo不为空（在实际开发中会验证token）则认为用户保持登录状态，则允许调用Controller，如果Session中userLoginInfo为空，则认为已经注销或者没登录，转发到登录页面

## 首页

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <a href="${pageContext.request.contextPath}/user/main">首页</a>
  <a href="${pageContext.request.contextPath}/user/goLogin">登录界面</a>
  </body>
</html>
```

## 登录

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>登录</h1>
<form action="${pageContext.request.contextPath}/user/login">
    用户名:<input type="text" name="username"/>
    密码：<input type="text" name="password"/>
    <input type="submit" value="登录">
</form>
</body>
</html>
```

## 主页

```xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${userLoginInfo}
<a href="${pageContext.request.contextPath}/user/destory">注销</a>
</body>
</html>
```

## Controller

```java
@Controller
@RequestMapping("/user")
public class LoginController {
    @RequestMapping("/main")
    public String main(){
        return "main";
    }
    @RequestMapping("/goLogin")
    public String login(){
        return "login";
    }
    @RequestMapping("/login")
    public String login(HttpSession session,String username, String password){
        System.out.println("login==>"+username);
        System.out.println("password==>"+password);
        session.setAttribute("userLoginInfo",username);
        return "main";
    }
    @RequestMapping("/destory")
    public String destory(HttpSession session){
        session.removeAttribute("userLoginInfo");
        return "redirect:/user/main";
    }
}
```

## Interceptor

```java
public class LoginInterceptor implements HandlerInterceptor {
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        HttpSession session = request.getSession();
//        第一次登录时Session中userLoginInfo为空，因此我们需要放行才能正常运行，否则会一直执行转发
        if(request.getRequestURI().contains("goLogin")){
            return true;
        }
        if(request.getRequestURI().contains("login")){
            return true;
        }
        if(session.getAttribute("userLoginInfo")!=null){
            return true;
        }
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
        return false;
    }
}
```

