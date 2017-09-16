---
title: java基础知识4
tags: [java]
---
**abstract**
```
abstract class Person{//不需要abstract方法也可以声明为abstract类
	private String name;
	public String getName(){
		return name;
	}
	abstract String getDecription();//虚类里面只能有声明，不能有具体的实现
}
class Student extends Person{

	@Override
	String getDecription() {//子类继承父类，需实现父类的abstract方法
		// TODO Auto-generated method stub
		return "Student";
	}
	
}
public class abstractTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
       Person p;
       //p=new Person();编译不通过，不能创建虚类的对象
       p=new Student();
      System.out.println( p.getDecription());
	}

}

```
<!--more-->
*小结：*

1.abstract可以修饰类和方法，有abstract方法的类必须为abstract类；
2.声明为abstract类，不一定有abstract方法；
3.abstract类里面的域为与其他类没啥区别；
4.abstract方法只有声明，没有实现；
5.abstract类被继承，要么实现里面的abstract方法，要么声明为abstract类；

