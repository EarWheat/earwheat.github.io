---
layout: post
title: [Java]Spring拦截器
date: 2018-08-14 
tag: Java
---

# [Java]Spring拦截器

背景：学习编写SSO，需要根据Session判断用户是否登陆或者登陆是否过期。

## 拦截器简介

Spring Web MVC的处理器拦截器（如无特殊说明，下文所说的拦截器即处理器拦截器）

类似于Servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理。


## 常见应用场景

1、日志记录：记录请求信息的日志，以便进行信息监控、信息统计、计算PV（Page View）等。

2、权限检查：如登录检测，进入处理器检测检测是否登录，如果没有直接返回到登录页面；

3、性能监控：有时候系统在某段时间莫名其妙的慢，可以通过拦截器在进入处理器之前记录开始时间，在处理完后记录结束时间，从而得到该请求的处理时间（如果有反向代理，如apache可以自动记录）；

4、通用行为：读取cookie得到用户信息并将用户对象放入请求，从而方便后续流程使用，还有如提取Locale、Theme信息等，只要是多个处理器都需要的即可使用拦截器实现。

5、OpenSessionInView：如Hibernate，在进入处理器打开Session，在完成后关闭Session。

…………本质也是AOP（面向切面编程），也就是说符合横切关注点的所有功能都可以放入拦截器实现。

## 拦截器接口源码


```
package org.springframework.web.servlet;  
public interface HandlerInterceptor {  
    boolean preHandle(  
            HttpServletRequest request, HttpServletResponse response,   
            Object handler)   
            throws Exception;  
  
    void postHandle(  
            HttpServletRequest request, HttpServletResponse response,   
            Object handler, ModelAndView modelAndView)   
            throws Exception;  
  
    void afterCompletion(  
            HttpServletRequest request, HttpServletResponse response,   
            Object handler, Exception ex)  
            throws Exception;  
}  
```

理解源码：
从源码中可以看出，拦截器实现了三种回调方法，分别是==preHandle：预处理回调方法、postHandle：后处理回调方法，afterCompletion：整个请求处理完毕回调方法，==

- preHandle：预处理回调方法，实现处理器的预处理（如登录检查），第三个参数为响应的处理器；返回值：true表示继续流程（如调用下一个拦截器或处理器）；false表示流程中断（如登录检查失败），不会继续调用其他的拦截器或处理器，此时我们需要通过response来产生响应；

- postHandle：后处理回调方法，实现处理器的后处理（但在渲染视图之前），此时我们可以通过modelAndView（模型和视图对象）对模型数据进行处理或对视图进行处理，modelAndView也可能为null。

- afterCompletion：整个请求处理完毕回调方法，即在视图渲染完毕时回调，如性能监控中我们可以在此记录结束时间并输出消耗时间，还可以进行一些资源清理，类似于try-catch-finally中的finally，但仅调用处理器执行链中preHandle返回true的拦截器的afterCompletion。


## 拦截器适配器

有时候我们可能只需要实现三个回调方法中的某一个，如果实现HandlerInterceptor接口的话，三个方法必须实现，不管你需不需要，此时spring提供了一个HandlerInterceptorAdapter适配器（一种适配器设计模式的实现），允许我们只实现需要的回调方法。

流程图如下：
![image](http://sishuok.com/forum/upload/2012/9/4/1217fc1b3d71239c0267e1b9bfeec893__1.JPG)


## 拦截器实现

拦截器端代码：
```
public class HandlerInterceptor1 extends HandlerInterceptorAdapter {//此处一般继承HandlerInterceptorAdapter适配器即可  
    @Override  
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
        System.out.println("===========HandlerInterceptor1 preHandle");  
        return true;  
    }  
    @Override  
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {  
        System.out.println("===========HandlerInterceptor1 postHandle");  
    }  
    @Override  
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {  
        System.out.println("===========HandlerInterceptor1 afterCompletion");  
    }  
}  
```

Controller代码：

```
public class TestController implements Controller {  
    @Override  
    public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {  
        System.out.println("===========TestController");  
        return new ModelAndView("test");  
    }  
}  
```

配置端代码：

```
<bean id="handlerInterceptor3"   
class="cn.javass.chapter5.web.interceptor.HandlerInterceptor3"/>  
<bean id="handlerInterceptor4"   
class="cn.javass.chapter5.web.interceptor.HandlerInterceptor4"/> 
```


```
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">  
    <property name="interceptors">  
        <list>  
           <ref bean="handlerInterceptor3"/>  
          <ref bean="handlerInterceptor4"/>  
        </list>  
    </property>  
</bean>  
```

interceptors：指定拦截器链，拦截器的执行顺序就是此处添加拦截器的顺序；


## 拦截器应用之登录检测

在访问某些资源时（如订单页面），需要用户登录后才能查看，因此需要进行登录检测。

 

流程：

1、访问需要登录的资源时，由拦截器重定向到登录页面；

2、如果访问的是登录页面，拦截器不应该拦截；

3、用户登录成功后，往cookie/session添加登录成功的标识（如用户编号）；

4、下次请求时，拦截器通过判断cookie/session中是否有该标识来决定继续流程还是到登录页面；

5、在此拦截器还应该允许游客访问的资源。

```
@Override  
public boolean preHandle(HttpServletRequest request, HttpServletResponse response,   
Object handler) throws Exception {  
    //1、请求到登录页面 放行  
    if(request.getServletPath().startsWith(loginUrl)) {  
        return true;  
    }  
          
    //2、TODO 比如退出、首页等页面无需登录，即此处要放行 允许游客的请求  
          
    //3、如果用户已经登录 放行    
    if(request.getSession().getAttribute("username") != null) {  
        //更好的实现方式的使用cookie  
        return true;  
    }  
          
    //4、非法请求 即这些请求需要登录后才能访问  
    //重定向到登录页面  
    response.sendRedirect(request.getContextPath() + loginUrl);  
    return false;  
}  
```

