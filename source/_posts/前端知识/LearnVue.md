---
title: Vue小结1
tags: [Vue]
---

***1.组件开发模板：***
```
<template> 
<!--
  这里写html代码，用elements组件最好了
-->
</template>



<script>
 import a from "./a.vue"
export defaut{ 
  data(){ //data必须为函数，每次引用该组件返回的是值的拷贝
   return {//返回的是一个对象
    
   }
  },
  mounted(){//生命周期相关

  },
  methods:{//写所需要的函数，返回的是一个对象

  },
  components:{//组件引用别名声明

  }

}
</script>


```

<!--more-->


***2.向组件传值***
当前组件名字：b.vue;

在script标签里面
1.import aa from "./aa.vue"
2.
```
 components：{
    "aa":aa
}
```
在template标签里面：
3.
```
<aa :dd="dd"> </aa>
```
在a.vue里面：
4.script标签里面：
props:["dd"],
这样，dd值可以在a.vue里面直接作为data里面的值来用了。

***3.调用子组件的函数***
当前组件名字：b.vue;

在script标签里面
1.
import aa from "./aa.vue"
2.
```
 components：{
    "aa":aa
}
```
3.在template标签里面：
```<aa ref="ccc"> </aa>```


在script标签methods里面
4.
直接写
```
this.$refs.ccc.getdata();
```
5.当然在aa.vue里面需要声明getdata()函数。