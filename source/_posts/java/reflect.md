
---
title: java反射学习
tags: [java]
---


   看了核心技术卷的知识后,对java的反射有了一定的了解；也去了解了更多的反射的应用，除了了解到可以查看类的方法和域以外，
让我比较惊讶的是反射居然可以对类的私有属性和方法进行调用；代码如下：
person.java:

```
public class Person {
    private String name="李四";
    private Integer age;
    public Person() {
    super();
    System.out.println("我是无参构造器");
    // TODO Auto-generated constructor stub
  }
    
    public Person(String name,Integer age) {
    this.name=name;
    this.age=age;
    
    System.out.println("我是有参构造器");
    // TODO Auto-generated constructor stub
  }
    
    public void getName(){
      System.out.println(this.name);
    }
    public void getPar(){
      System.out.println(this.name+this.age);
    }
    @SuppressWarnings("unused")
  private void  print(String name,Integer integer){
      System.out.println("我被可见了"+name+integer);
    }
}
```
<!--more-->
```
public class ReflectTest {

	/**
	 * @param args
	 */
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//使得私有域可见
       Person person=new Person();
       Class cl=person.getClass();
       try{
         Field field=cl.getDeclaredField("name");
         field.setAccessible(true);
         field.set(person, "张三");
         person.getName();
       }catch(Exception e){
    	   e.printStackTrace();
       }
       //使得私有方法可见
       try{
        Method method=cl.getDeclaredMethod("print",String.class,Integer.class);
        method.setAccessible(true);
        method.invoke(person,"张三",123);
       }catch(Exception e){
    	   e.printStackTrace();
       }
       //调用无参构造器
       try{
    	 @SuppressWarnings("unchecked")
        Constructor c=cl.getConstructor(null);
        Person p2=(Person)c.newInstance(null);
        p2.getName();
       }catch(Exception e){
    	   e.printStackTrace();
       }
       
       //调用有参构造器
       try{
           Constructor c=cl.getConstructor(String.class,Integer.class);
           Person p2=(Person)c.newInstance("王五",50);
           p2.getPar();
          }catch(Exception e){
       	   e.printStackTrace();
          }
	}

}
```
程序输出：
我是无参构造器
张三
我被可见了张三123
我是无参构造器
李四
我是有参构造器
王五50
   可以看到Java的反射机制对类的控制显然比较的高。