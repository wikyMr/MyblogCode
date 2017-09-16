---
title: java基础知识篇之接口
tags: [java]
---

**接口**
接口中的域为static且final的，且不能为空的final；
主要涉及到设计模式：策略模式，适配者模式，工厂模式；
```
package interfaceTest5;
interface I1{
	public void f();
}
interface I2{
	public int f();
}
interface I3{
	public int f();
}
class C1{
	public void f(){
		
	}
}
//以下代码编译不通过，不能有接口和父类一样的方法
/*class C2 extends C1 implements I1,I2{
	
}*/

public class interfaceTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

	}

}
```
当与重写重载实现混合在一起的时候，要尽量避免方法名的一致；
