---
title: Springboot项目建立
tags: [Spring]
---

概要：

github链接：
https://github.com/wikyMr/LearnSpring；



在一番百度和前辈的指引下，我算是成功的把一个Spring boot项目搭建了起来；
不得不说，spring boot是一个非常方便的框架，大大简化了我们的开发，只需要一点点的配置，然后我们就可以建立起自己的项目了；

***1.搭建maven项目***

http://www.cnblogs.com/qinbb/articles/5762081.html

这个链接给出的搭建过程，可以简单的建立一个可以访问的应用；但是src/main/resource文件夹不见了，怎么也找不出来，而且新建一个文件夹也不可见。
网上的说法是可以新建Source Folder即可。

<!--more-->

还有一个参考的博客链接就是：
http://www.cnblogs.com/NieXiaoHui/p/5990570.html；也是一个建项目的另外一种方式。缺点就是其他的类需要自己创建。

***2.与MyBatis整合***

http://blog.csdn.net/gebitan505/article/details/54929287：
配置足够的少，用的真是太爽了！注意xml要写相应的内容，否则就把它删除。不然的话会报错！

***3.与Swagger-ui整合***

http://www.cnblogs.com/java-zhao/p/5348113.html：
这个链接也是与spring-boot整合在一起，非常的方便。

至此一个很简单的web项目就建立好了。

附上github链接：
https://github.com/wikyMr/LearnSpring