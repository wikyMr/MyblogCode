
---
title: java基础知识3
tags: [java]
---

**可变参数**
Java SE5.0以后，提供了可变参数的调用方法：语法：int... arr
下面是一些需要特别注意的点：
public class VariableMethod {
	
	public static  void showarray(int a,int... arr){
		System.out.println("a:"+a);
		for(int i:arr){
			System.out.print(i+" ");
		} 
		System.out.println();
	}
	//编译不通过，不支持两个可变参数
	/*public static  void showarray(int a,int... arr,String...b){
		System.out.println("a:"+a);
		for(int i:arr){
			System.out.print(i+" ");
		} 
		System.out.println();
	}*/
	//不支持重载void showarray(int a,int... arr),下面的方法编译不通过
	/*public static  void showarray(int a,int[] arr){
		//System.out.println("a:"+a);
		for(int i:arr){
			System.out.print(i+" ");
		} 
		System.out.println();
	}	*/
	//编译不通过，可变参数需要放在最后一个参数
	/*public static void showarr(int... arr,String name){
		
	}*/
    public static void showarray(int a,int b,int c){
	   	System.out.println(a+" "+b+" "+c);
	}
	
     public static void   main(String[] args){
    	showarray(1,2,3);//优先匹配不可变参数的方法,输出1 2 3 而不是a:1
    	//showarray(1,2,3);
    	//可以把数组传递给可变参数的最后一个参数
    	showarray(2,new int[]{1,2,3});
     }
}

<!--more-->
总结：
1.可变参数必须是参数的最后一项，且是唯一一项，支持传入数组；
2.可变参数不能以数组的方式重载；
3.编译器会优先匹配不可变参数的方法；

**类型转化**
注意的点：
需要在类的继承层次上转化，转换前对其进行类型检查（instanceof方法）；

