---
title : java与json相关
tags : [java]
---

现在前端的数据基本上都通过json来传输，不得不说，json比xml之类的文本格式好多了，能够最大程度的把数据展现给我们，不需要太拘泥于格式。
而对于Java后端程序员来说，我们也需要去调用其他域名（项目）的接口，而这个接口返回的数据是JSON格式，那么我们就需要把它转化成一个对象进行数据的处理；

***Alibaba的fastjson***

Fastjson介绍：
Fastjson是一个Java语言编写的JSON处理器,由阿里巴巴公司开发。遵循http://json.org标准，为其官方网站收录的参考实现之一。支持JDK的各种类型，包括基本的JavaBean、Collection、Map、Date、Enum、泛型。有如此好的轮子，所以可以使用上。
pom.xml加入依赖：
```
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>fastjson</artifactId>
	<version>1.0.4</version>
</dependency>
```

好吧，阿里巴巴的东西确实蛮好的，看那些json文档以及阿里巴巴的github学习了好多fastjson的东西。恩，好好学习天天向上吧！！！

附上同性交友链接：
https://github.com/alibaba
