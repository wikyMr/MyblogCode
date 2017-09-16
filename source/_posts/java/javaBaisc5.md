---
title: Java笔试面试常考常问题总结
tags: [java]
---

本来想着写一些笔试自己遇到的多数的坑，然而写着写着就想到了很多面试遇到的比较有意思的问题，在此特意总结一下，希望自己以后可以吃一暂长一智吧。
**switch**
***1.switch支持的数据类型：***
```
       byte bb=1;
        char cc='测';
        short  ss=12;
        int i=0;
        long ll=12333L;
        float f=1.3f;
        double d=12.3;
        boolean b1=true;
        String string="123";
        switch(bb){
        }
        switch(cc){
        }
        switch(ss){
        }
        switch(i){
        }
       //以下情况会编译报错
        switch (ll){
		}
        switch(f){
        }
        switch(d){
        }
        switch(b1){
        }
        switch(string){
        }
```
可以看到，Java的switch语句支持的数据类型包括8种数据类型中的4种（这四种不需要强制类型转换都能够转换成int类型），另外还支持String类型。不支持的基本数据类型有float，double，long，boolean。
<!-- more -->
##HashMap##
  感觉这个数据结构是大多数面试官考察的重点。所以这一部分不得不说是一个很值得去探讨的方面。
  首先HashCode和equals究竟是长啥样的？
  ###hashcode和equals###
  1.equals返回true,那么hashcode返回值也是一样。但是反之不成立。
  2.Object的默认实现：
  ***hashcode返回的是对象的内存地址，equals实现是x==y;***
```
    Object b=new Object();
    Object a=new Object();
    Object c=a;
    System.out.println(b.hashCode()+" "+a.hashCode());//366712642 1829164700
    System.out.println(a.equals(b));//false
    System.out.println(a==c);//true
    System.out.println(a.equals(c));//true
```
***基本数据类型的封装类型的hashcode和equals**
基本数据类型的封装类型hashcode返回的是值，equals比较的是
值相等。


###发表一下你对Spring，SpringMVC，MyBatis框架的理解###
1.好吧，那天我的语言组织能力有点混乱，然后答得不是很好。
这些问题其实也是很考验我们的能力的，所以会努力的组织一下。

###数据库###
***1.问了jdbc如何获取后台数据***
那天不记得很多具体代码实现了，所以答得也不是特别的好；

```
package jdbc;

import java.io.FileInputStream;
import java.io.IOException;
import java.sql.*;
import java.util.ArrayList;
import java.util.Properties;
import java.util.Scanner;;



public class JDBCTest {
    //驱动
	//private static String jdbDriver="com.mysql.jdbc.Driver";
	private static String jdbcDriver="";
    //数据表的位置
	//private static String jdbUrl="jdbc:mysql://localhost:3306/myuser";
	private static String jdbcUrl="";
	//用户
	//private static String jdbUser="root";
	private static String jdbcUser="";
	//密码
	//private static String jdbpwd="123456";
	private static String jdbcpwd="";
	//连接
	private  static Connection conn;
	//操作数据库的对象
    private static java.sql.Statement st;
    
    
    static{
    	FileInputStream iStream=null;
    	try{
    		iStream=new FileInputStream("database.properties");
    		Properties properties=new Properties();
    	    properties.load(iStream);//在根目录下
    	    jdbcDriver=properties.getProperty("jdbcDriver");
    	    jdbcUrl=properties.getProperty("jdbcUrl");
    	    jdbcUser=properties.getProperty("jdbcUser");
    	    jdbcpwd=properties.getProperty("jdbcpwd");    
    	}catch (IOException e) {
			// TODO: handle exception
    		e.printStackTrace();
		}finally {
			if(iStream!=null){
				try {
					iStream.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
    }
    public static Connection getMyConn(){
    	try {
			Class.forName(jdbcDriver);
			conn=DriverManager.getConnection(jdbcUrl, jdbcUser, jdbcpwd);
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return conn;
    }
     //根据驱动去获得连接
    public static Connection getConnection(){
    	Connection con=null;
    	try {
    		
    		//加载驱动器
			Class.forName(jdbcDriver);
			
    		//驱动利用驱动url，用户名，密码创建连接
			con=(Connection) DriverManager.getConnection(
						jdbcUrl,jdbcUser,jdbcpwd);
    	}catch (ClassNotFoundException e) {//加载驱动器异常
			e.printStackTrace();
		} catch (SQLException e) {//创建连接异常
			e.printStackTrace();
		}
    	
    	return con;
    }
    //创建表，可以这么做但是无法准确得知结果
    public static boolean createTable(){
    	boolean isSucceed=false;
    	conn=getConnection();
    	try{
    		
    		//INSERT INTO `staff` (`ID`, `name`, `age`, `sex`, `address`, `depart`, `worklen`, `wage`) 
    		//VALUES (NULL, 'lisi', '62', 'm', 'ACM', 'personnel', '10', '3000');
    		//创建表的SQL语句
    		String sql="CREATE TABLE JDBCTEST "+
    		         "(ID INTEGER NOT NULL,"+
    				 " FIRST VARCHAR(255),"+
    		         " LAST VARCHAR(255),"+
    				 " AGE INTEGER,"+
    		         " PRIMARY KEY( ID))";
    				
    		
    		//连接创建数据库操作对象，Statement
    		st=conn.createStatement();
    		//Statement利用executeUpdate方法执行sql语句
    		//executeUpdate返回的是影响的行数，对Create/drop table等不操作行的操作返回0；
    	 //int flag=st.executeUpdate(sql);
    	  int flag=st.executeUpdate(sql);
    	   if(flag==0){
    		  isSucceed=false;
    	   }else{
    		   isSucceed=true;
    	   }
    	}catch(SQLException e){
            	e.printStackTrace();
    	}finally{//最后关闭连接，放在finally里面可以保证抛出异常时连接能够正确关闭
    		try {
				conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
    	}
		return isSucceed;
    }
    //插入删除查找数据
    public static boolean operate(){
    	boolean isSucceed=true;
    	conn=getConnection();
    	try{
    		//插入一个记录
    		//String sql="INSERT INTO jdbctest(first,last,age)"+" VALUES('programer','ceo',25);";
    		//删除一行
    	    //String sql="DELETE from jdbctest where id=1;";
    	    //修改一行
    	    String sql="update jdbctest set first='cffo' where id=2;";
    		//连接创建数据库操作对象，Statement
    		st=conn.createStatement();
    		//Statement利用executeUpdate方法执行sql语句
    		//executeUpdate返回的是影响的行数，对Create/drop table等不操作行的操作返回0；
    	 int flag=st.executeUpdate(sql);
    	   if(flag==0){
    		  isSucceed=false;
    	   }else{
    		   isSucceed=true;
    	   }
    	}catch(SQLException e){
            	e.printStackTrace();
    	}finally{//最后关闭连接，放在finally里面可以保证抛出异常时连接能够正确关闭
    		try {
				conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
    	}
		return isSucceed;
    }
    
   //查找数据
    public static  ArrayList<Data> query(){
    	ArrayList<Data> result=new ArrayList<Data>();
    	conn=getConnection();
    	ResultSet rs;
    	Data sData;
    	try {
		st=conn.createStatement();
    	String sql="select * from jdbctest;";
		rs = st.executeQuery(sql);
    	while(rs.next()){
			sData = new Data(rs.getString("first"), rs.getString("last"), rs.getInt("age"));
			
    		result.add(sData);
    	}
    	} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
    	return result;
    }
    
    
    public static void main(String[] args){
    	//getConnection();
       //insert();
    	
    	System.out.println("1.创建表\n2.操作数据\n3.查询数据");
    	Scanner in=new Scanner(System.in);
    	int t=in.nextInt();
    	switch(t){
    	case 1:
    		if(createTable()){//这里会一直返回false
    			System.out.println("创建成功");
    		}else{
    			System.out.println("创建失败");
    		}
           break;
    	case 2:
    		if(operate()){
    			System.out.println("操作成功");
    		}else{
    			System.out.println("操作失败");
    		}
    		break;
    	case 3:
    		ArrayList<Data> sArrayList=query();
    		for(Data aData:sArrayList){
    			System.out.println(aData);
    		}
    		break;
        default:
    		break;
    	}
    	//createTable();
    }
   
}

```
为了证明我自己曾经也是很认真的手写了jdbc的连接，获取数据的代码。所以觉得有必要把它放到这里来，然后以后可以重温一下。

***2.一些特别捉急的问题***
涉及数据库的语句，总之我在简历上写的东西，都有问到了。譬如HAVING一定要跟在GROUP BY后面之类的问题。加上上次被问到的一道数据库题目：
大概描述如下：
| id       | name   |  score  |   date  |
| 01       | cyw    |   90    | 2017-5-4|
| 02       | ywl    |   60    | 2017-5-4|
| 03       | yjf    |   70    | 2017-5-5|
| 04       | pby    |   40    | 2017-5-5|
规定：成绩大于60分代表通过。
需求就是统计每天通过的人数：
譬如：
|   date         | 通过人数    |  失败人数 | 
| 2017-5-4       |  50%       |           |
然后类似这样的题目也把我卡住了。