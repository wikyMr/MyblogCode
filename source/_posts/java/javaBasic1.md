
---
title: java基础知识1
tags: [java]
---

**public,protected,private可见性**
public：所有类都可见；
protected：包内所有类以及其他包的子类；
private：己类以及内部类可见；
默认(不加修饰符)：包内可见；
**static:**
静态域：被static修饰的域，它属于类，不属于任何对象实例；
static修饰的方法，属于类，可通过类来调用，同时返回类型也必须是static的类型(基本类型)；
不能直接访问非静态对象和方法，不能使用super和this；
也就意味着只能操作静态域了。
static块：
在类加载时被创建，优先于非static块执行；

总而言之，static修饰的东西会在类加载时准备好，脱离对象执行；

<!--more-->

```

class Employee{
	private int id=1;
	private static int staticid=1;
	
	public int getstaticid(){
		return staticid;
	}
	public int getid(){
		return id;
	}
	public  int  addid(){
		id++;
		return id;
	}
	public static  int  addstaticid(){
		//id++；//error
		//this.name="";//编译不通过
		staticid++;
		return staticid;
	}
}

public class StaticTest {
    
	public static void main(String[] args){
		       //静态块会顺序执行
		Employee e=new Employee();
		System.out.println(e.getstaticid());//1
		Employee e1=new Employee();
		System.out.println(e1.getstaticid());//1
				//通过类的静态方法也可以改变静态域
		System.out.println(Employee.addstaticid());//2
		System.out.println(e.addstaticid());//3
				//所有对象共享一个静态类，它属于类
		System.out.println(e1.getstaticid());//3
		/*final Person s1=new Person();
		final Person s2=new Person();
		s1=s2;//编译不通过
*/	
		System.out.println(e.getid());//1
		e.addid();
		System.out.println(e.getid());//2
		System.out.print(e1.getid());//1
		}
    
}
```

例子二：

```
class Employee{
	private int id=1;
	private static int staticid=1;
	private static String name;
	static{
		staticid++;
		//id++;//编译不通过，须为static变量
	}
	
	{
		id--;
		staticid--;
	}
	public String getOlder(){
		return name;
	}
	public int getstatic(){
		return staticid;
	}
	public int getid(){
		return id;
	}
	public static  int  getstaticid(){
		//staticid++;
		return staticid;
	}
	static{
		staticid++;
	}
	{
		id+=2;
	}
}

public class StaticTest {
    
	public static void main(String[] args){
		Employee e=new Employee();
		System.out.println(e.getstatic());//3
		Employee e1=new Employee();
		System.out.println(e1.getstatic());//3
		System.out.println(Employee.getstaticid());//4
		System.out.println(e1.getstatic());//4
		
		System.out.println(e.getid());
		System.out.println(e.getOlder());
		/*final Person s1=new Person();
		final Person s2=new Person();
		s1=s2;//编译不通过
*/	
}  
}
```
**final:**
final实例域被设置之后，不能被修改；
final修饰的方法不能被重写(父子继承关系)；
final修饰的类，不能被继承；

```
final Person s1=new Person();
		final Person s2=new Person();
		s1=s2;//编译不通过
```

finalize方法：
垃圾回收器在确认对象不再被引用时调用，不确保能被执行；
可以用来释放非Java资源；

import:
导入包，如果两个包都需要使用，则需明确写明；
如：java.sql.date和java.util.date类；

cmd中javac与java的差别：
javac com/wiky/test/a.java
Java com.wiky.test/a
编译器不会监测目录结构，运行时才会出错(包名出错时)；



