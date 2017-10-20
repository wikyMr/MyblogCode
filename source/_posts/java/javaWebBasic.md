---
title: javaweb基础知识
tags: [java web]
---

**jsp提供的8个内置对象**

--1.PageContext pageContext；
jsp页面的用法：
pageContext.setAttribute("name","pageContext对象");
--2.HttpSession session；
session.setAttribute("name","session对象");

--**3.ServletContext application;**
1.每个web应用对应于一个ServletContext，可以再多个servlet间传递数据；
ServletConfig对象中维护了ServletContext对象的引用，可以通过ServletConfig.getServletContext方法获得ServletCOntext；
```
ServletContext context = this.getServletConfig().getServletContext();//获得ServletContext对象

ServletContext context = this.getServletContext();
```
例子1：一个Servlet通过上面的方法获得ServletCOntext，调用setAttribute("name");
在另一个通过上面的方法获得ServletCOntext，调用getAttribute("name");
2.获取整个站点的初始化数据：
在web.xml全局配置
<context-param>
<param-name><param-value>
</param-value></param-name>
</context-param>
属性后，可以在Servlet里面
application.setAttribute("name","application对象");

----**4.ServletConfig config：**
配置web.xml时，会自动把参数封装到ServletConfig对象里面；
在调用Servlet.init时，传到Servlet,通过ServletConfig对象可以得到当前Servlet的初始化参数;

---5.JspWriter out;
---6.ObjectPage page=this;
--7.HttpServletRequest request;
--8.HttpServletResponse response;

这8个对象可以再jsp页面里面直接使用；
