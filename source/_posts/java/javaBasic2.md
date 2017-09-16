---
title: java基础知识2
tags: [java]
---

**继承/多态**
父类：雇员类，有id，name，salary域，以及相应的getter和setter；
```
public class Employee{
	private int id;
	private double salary;
	private String name;
	
	public Employee(){
		
	}
	
	public Employee(int id,double salary,String name){
		this.id=id;
		this.salary=salary;
		this.name=name;
	}
	
	public void setId(int id) {
		this.id = id;
	}
	public int getId() {
		return id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public double getSalary() {
		return salary;
	}
}
```
<!--more-->
子类：经理类，具有更加丰富的实现，添加bonus域；构造器调用父类的构造器需写在第一行；同时重写计薪方法：
```
public class Manager  extends Employee{
     //不需要重定义父类的域
	//添加新的域即可
	private double bonus;//奖金域
	public Manager(){
	  super();
	  this.bonus=0;
	}
	
	public Manager(int id,double salary,double bonus,String name){
		super(id, salary, name);//要作为构造器的第一条语句出现
		this.bonus=bonus;
	}
	
	//重写父类的getSalary方法
   public double getSalary(){
	   return this.bonus+super.getSalary();//通过super来调用父类的方法，不能直接访问父类的域
   }
	
}
```
主函数：
```
public class FatherAndSonTest {
  public static void main(String[] args){
	  Employee[] sEmployees=new Employee[3];
	  //Manager manager=new Manager(1, 5000,1000, "Wiky");
	  sEmployees[0]=new Manager(1, 5000,1000, "Wiky");
	  sEmployees[1]=new Employee(2, 3000, "Yiky");
	  sEmployees[2]=new Employee(3, 4000, "Piky");
	  
	  //e可以引用Employee对象，也可以引用Manager对象，虚拟机知道e的实际引用类型
	  Employee t=(Employee)sEmployees[0];
	  System.out.println(t.getSalary());//强制类型转换也没有用
	  for(Employee e:sEmployees){
		  System.out.println(e.getSalary());
	  }
  }
}

```
输出：
6000.0
6000.0
3000.0
4000.0

可以看到e可以引用Employee对象，也可以引用Manager对象，虚拟机知道e的实际引用类型;（即使强制类型转换也没有用）

引申多态：
一个对象变量可以指示多种实际类型的现象被称为多态；
置换语法：如is-a准则，表示任何超类出现都可以用子类替换；

注：
```
Manager[] managers=new Manager[10];
	  
	  Employee[] employees=managers;
	  
	  employees[0]=new Employee();//由于managers和employees引用同一个对象，用超类代替子类时，运行时会出现ArrayStoreException
	  
	 managers[0].setbous(1000);
	 
	 System.out.println(managers[0].getSalary());
```

动态绑定：运行时自动选择调用哪个方法；
一个例子：

	class A{
		/*A(){
			System.out.println("new a A class");
		}*/
		public String show(D obj){
			return "A and D";
		}
		public String show(A obj){
			return "A and A";
		}
	}
	class B extends A{
		/*B(){
			System.out.println("new a B class");
		}*/
		public String show(B obj){
			return "B and  B";
		}
		public String show(A obj){
			return "B and  A";
		}
		
	}
	class C extends B{
		/*C(){
			System.out.println("new a C class");
		}*/
	}
	class D extends B{
		/*D(){
			System.out.println("new a D class");
		}*/
	}
	public class dynamicbindingTest {
	    public static void main(String[] args){
	          A a1=new A();
	          A a2=new B();//上转型对象
	          B b=new B();
	          C c=new C();
	          D d=new D();
	          
	          System.out.println("a1.show(b)"+a1.show(b));//A and A
	          System.out.println("a1.show(c)"+a1.show(c));//A and A
	          System.out.println("a1.show(d)"+a1.show(d));//A and D
	          
	          // 一个准则：***当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，
	          //但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法。***
	          System.out.println("a2.show(b)"+a2.show(b));//B and A,
	          System.out.println("a2.show(c)"+a2.show(c));//B and A
	          System.out.println("a2.show(d)"+a2.show(d));//这里没有覆盖，但会去父类找，所以输出为A and D
	          
	          System.out.println("b.show(b)"+b.show(b));//B and B
	          System.out.println("b.show(c)"+b.show(c));//B and B
	          System.out.println("b.show(d)"+b.show(d));//这里没有覆盖，但会去父类找，所以输出为A and D
	    }
	}


这里主要涉及到上转型对象的操作，下面是上转型对象的例子：
```

class Animal{
	public int a;
	public Animal see(){
		System.out.println("animal see");
		return new Animal();
	}
	public void call(){
		System.out.println("animal call!");
	}
	public static void a(){
		System.out.println("animal static");
	}
}

class Dog extends Animal{
	
	//下面的方法编译不通过，需为void的返回类型,子类的返回类型须小于父类
	//下面的方法编译通过
   /*public  Dog see(){
	    System.out.println();
	    return new Dog();
	}*/
	
	//下面的方法编译不通过，子类的可访问性需要大于父类
	/*protected  Dog see(){
	    System.out.println();
	    return new Dog();
	}*/
	public int d;
	public void call(){
		System.out.println("dog call");
	}
	public void d(){
		d++;
	}
	public static void a(){
		System.out.println("dog static");
	}
}
public class TransitionalObjTest {
    public static void main(String[] args){
    	Animal dog=new Dog();
    	dog.call();//dog call
    	dog.see();
    	dog.a();//animal static
    	System.out.println(dog.a);
    	//子类的域和静态方法不具有多态性质
    	//dog.d;//编译不通过，不能访问子类的域
    	//dog.d();//编译不通过，不能调用不是重写的方法
    }
}

```
可以看到，上转型对象实际上失去了很多属性和方法，不能访问子类的域，不能调用不是不是子类重写的方法，包括静态的方法；
同时这里也涉及到多台的重载和重写，总结如下：
**重写**
父子类多态的体现，总结：
两同，两小，一大：
两同：方法名字，参数列表相同；
两小：抛出的异常要小于父类，返回类型也要小于父类；
一大：访问权限要大于父类；


**重载**
一同，一不同，一无关：
一同：在同一个类里面，方法名相同；
一不同：参数列表（类型）不同；
一无关：与函数的返回类型无关；















