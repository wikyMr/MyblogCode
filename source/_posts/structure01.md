---
title: java数据结构之栈和队列的数组以及链表实现
tags: [java_data_struct]
---

如题，这篇博客就是用java实现栈和队列的两种方式：数组以及链表，总的来说，我
比较的喜欢链表，因为在队列里面，pop一个元素时，不需要像数组那样考虑去重新整
合空间，orz，我是偷懒了，没有把数组的空间释放掉(数组实现队列时)，当然，把数组
内容赋值为null之后，java虚拟机也会回收空间的了。
<!--more -->
代码及注解：
**数组来实现队列**
```
package com.wiky.arr;

import java.util.Iterator;

class MyQueue<Item> implements Iterable<Item>{//继承可迭代接口
	private Item[] arr=(Item[])new Object[1];
	private int N;
	private int count;
	public boolean isEmpty(){
		return N==0;
	}
	public int size(){
		return N;
	}
	public void newSize(int n){
		int m=2*n;
		@SuppressWarnings("unchecked")
		Item[] t=(Item[])new Object[m];
		for(int i=0;i<n;i++){
			t[i]=arr[i];
		}
		arr=(Item[])new Object[m];
		arr=t;
	}
	public void push(Item a){
		if(arr.length==N)
      		newSize(arr.length);
      	arr[N]=a;
      	N++;
	}
	public Item pop(){
	    if(N!=count){
		Item a=arr[count];
		arr[count]=null;
		count++;

		return a;
	    }else{
	    	return null;
	    }
		
	}
	public Iterator iterator(){
		return new Iterator(){

			int i=count;
			@Override
			public boolean hasNext() {
				// TODO Auto-generated method stub
				return i!=0;
			}

			@Override
			public Object next() {
				// TODO Auto-generated method stub
				Item t=arr[i];
				i--;
				return t;
			}
			
		};
	}
}
public class QueueTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
       MyQueue<Integer> arr=new MyQueue<Integer>();
       arr.push(1);
       arr.push(2);
       arr.push(3);
       
       System.out.println(arr.pop());
       System.out.println(arr.pop());
       System.out.println(arr.pop());
       System.out.println(arr.pop());
       // //输出：1 2 3;
	}

}
```
<hr/>
**数组方式实现栈**
```
package com.wiky.arr;

import java.util.Iterator;

class MyStack<Item> implements Iterable<Item>{//继承可迭代接口
	private Item[] arr=(Item[])new Object[1];
	private int N;
	public boolean isEmpty(){
		return N==0;
	}
	public int size(){
		return N;
	}
	public void newSize(int n){
		int m=2*n;
		//不能创建一个泛型的数组，但是可以强制类型转换
		@SuppressWarnings("unchecked")
		Item[] t=(Item[])new Object[m];
		for(int i=0;i<n;i++){
			t[i]=arr[i];
		}
		arr=(Item[])new Object[m];
		arr=t;
	}
	public void push(Item a){
      	/*if(arr.length==N){
      		newSize(arr.length);
      	}else{//这里加上个else就会导致扩展空间时不放入那个元素了
      		arr[N]=a;
      		N++;
      	}*/
		if(arr.length==N)
      		newSize(arr.length);
      	arr[N]=a;
      	N++;
	}
	public Item pop(){
	    if(N!=0){
		Item a=arr[--N];
		arr[N]=null;
		//先N--再创建新的空间
		if(arr.length/4==N)
			//newSize--arr.length/2而不是N/2
			newSize(arr.length/2);
		
		return a;
	    }else{
	    	return null;
	    }
		
	}
	public Iterator iterator(){
		return new Iterator(){

			int i=N;
			@Override
			public boolean hasNext() {
				// TODO Auto-generated method stub
				return i!=0;
			}

			@Override
			public Object next() {
				// TODO Auto-generated method stub
				Item t=arr[i];
				i--;
				return t;
			}
			
		};
	}
}
public class StatckTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
       MyStack<Integer> arr=new MyStack<Integer>();
       arr.push(1);
       arr.push(2);
       arr.push(3);
       
       System.out.println(arr.pop());
       System.out.println(arr.pop());
       System.out.println(arr.pop());
       System.out.println(arr.pop());
       //输出：3 2 1 null;
	}

}
```
<hr/>
**链表实现栈**
```
package com.wiky.list;

import java.util.Iterator;

class MyStack<Item>{
	private Node first;
	private int N;
	private class Node{
		Node next;
		Item value;
	}
	public boolean isEmpty(){
		return N==0;
	}
	public int size(){
		return N;
	}
	public void push(Item t){
		Node oldfirst=first;
		first=new Node();
		first.value=t;
		first.next=oldfirst;
		N++;
			
	}
	public Item pop(){
		Node t=first;
		first=first.next;
		N--;
		return t.value;
	}
	
	@SuppressWarnings({ "rawtypes", "unchecked" })
	public Iterator<Item> iterator(){
		return new Iterator(){
           private Node current=first;
			@Override
			public boolean hasNext() {
				// TODO Auto-generated method stub
				return current!=null;
			}

			@Override
			public Item next() {
				// TODO Auto-generated method stub
				Item t=current.value;
				current=current.next;
				return t;
			}
			
		};
	}
}
public class StackTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
       MyStack<Integer> it=new MyStack<Integer>();
       it.push(1);
       it.push(2);
       it.push(3);
       it.push(4);
       
       System.out.println(it.pop());
       System.out.println(it.pop());
       System.out.println(it.pop());
       System.out.println(it.size());
       //输出同数组实现方式
	}

}
```
<hr>
**链表方式实现队列**
```
package com.wiky.list;

import java.util.Iterator;

class MyQueue<Item>{
	private Node first;
    private Node last;
    private int N;
    class Node{
    	Item value;
    	Node next;
    }
    public int size(){
    	return N;
    }
    public boolean isEmpty(){
    	return N==0;
    }
    public void push(Item item){
    	/*//表头插入元素
    	if(first==null&&last==null)
    	{	first.value=item;
    	    last=first;
    	    last.next=null;
    	    first.next=null;
    	    N++;
    	}else{
    		Node t=new Node();
    		t.value=item;
    		t.next=first.next;
    		first.next=t;
    		
    	}*/
    	//在表尾插入元素
    	Node oldLast=last;
    	last=new Node();
    	last.value=item;
    	last.next=null;
    	if(isEmpty()) first=last;
    	else    oldLast.next=last;
    	N++;
    }
    //没有向前取址的指针就比较的麻烦，所以在表头删除元素会比较好
    public Item pop(){
    	if(!isEmpty()){
       Item t=first.value;
       first=first.next;
       N--;
       return t;
     }else{
    		return null;
    	}
    }
    @SuppressWarnings({ "unchecked", "rawtypes" })
	public Iterator<Item> iterator(){
    	return new Iterator(){
           private Node  current=first;
			@Override
			public boolean hasNext() {
				// TODO Auto-generated method stub
				return current!=null;
			}

			@Override
			public Item next() {
				// TODO Auto-generated method stub
				Item t=current.value;
				current=current.next;
				return t;
			}
    		
    	};
    }
}
public class QueueTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
      MyQueue<Integer> mq=new MyQueue<Integer>();
      mq.push(1);
      mq.push(2);
      mq.push(3);
      mq.push(4);
      
     System.out.println(mq.pop());
     System.out.println(mq.pop());
     System.out.println(mq.pop());
     System.out.println(mq.pop());
     System.out.println(mq.pop());
      //输出同数组实现方式
	}

}
```
总结：栈和队列这两个最基本的数据结构，有很多的共同点：
1.它们都是从一段插入然后从一端删除的数据结构，不会在数据结构的中间插入或者删除一个元素；
2.都可以用数组或者链表来实现，数组实现会比较的简介，链表实现则会有点繁琐，但是会很有趣；