# 使用JDBC对Mysql数据库的CRUD操作

> JDBC全称为JAVA Database Connectivity (java数据库连接),是SUN公司提供的一种数据库访问规则、规范, 由于数据库种类较多，并且java语言使用比较广泛，sun公司就提供了一种规范，让其他的数据库提供商去实现底层的访问规则。 我们的java程序只要使用sun公司提供的jdbc驱动即可。

<!-- more -->
## JDBC驱动
* 首先我们需要去下载对应数据库的JDBC驱动，MySQL就是下载MySQL的JDBC驱动，Oracle就下载Oracle的JDBC驱动

## 新建数据库配置文件xxx.properties
* 在项目根目录下新建xxx.properties文件，这里的信息以key=value的形式书写，我们在连接数据库时就会从这里面读取配置信息；
```java
    driverClass=com.mysql.jdbc.Driver
    url=jdbc:mysql://localhost/student
    name=root
    password=root
```
## 新建工具包，抽离出数据库连接语句
* 我们新建一个工具包比如com.chenxiaolani.util,在这下面我们新建一个JDBCUtil.java的文件，内容主要是使用JDBC抽离出数据库连接语句，还有一些释放资源的语句，以便复用；
```java
package com.chenxiaolani.uitl;

import java.io.FileInputStream;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JDBCUtil {
	
	static String driverClass = null;
	static String url = null;
	static String name = null;
	static String password= null;
	
	static{
		try {
			//1. 创建一个属性配置对象
			Properties properties = new Properties();
            //2. 读取数据库连接配置文件
			InputStream is = new FileInputStream("jdbc.properties");
			//3. 导入输入流。
			properties.load(is);
			
			//4. 读取属性
			driverClass = properties.getProperty("driverClass");
			url = properties.getProperty("url");
			name = properties.getProperty("name");
			password = properties.getProperty("password");
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 获取连接对象
	 * @return
	 */
	public static Connection getConn(){
		Connection conn = null;
		try {
            //1. 这个其实不用写，JDBC源码中已经帮我们注册了一次
			Class.forName(driverClass);
			//2. 建立连接 参数一： 协议 + 访问的数据库 ， 参数二： 用户名 ， 参数三： 密码。
			conn = DriverManager.getConnection(url, name, password);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return conn;
	}
	
	/**
	 * 释放资源
	 * @param conn
	 * @param st
	 * @param rs
	 */
	public static void release(Connection conn , Statement st , ResultSet rs){
		closeRs(rs);
		closeSt(st);
		closeConn(conn);
	}
	public static void release(Connection conn , Statement st){
		closeSt(st);
		closeConn(conn);
	}

	
	private static void closeRs(ResultSet rs){
		try {
			if(rs != null){
				rs.close();
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			rs = null;
		}
	}
	
	private static void closeSt(Statement st){
		try {
			if(st != null){
				st.close();
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			st = null;
		}
	}
	
	private static void closeConn(Connection conn){
		try {
			if(conn != null){
				conn.close();
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			conn = null;
		}
	}
}
```
## DAO模式
* DAO即为（Data Access Object），中文为数据访问对象，DAO模式一般为两个文件，一个是定义方法的接口，一个则是实现这些接口的文件；
* 首先我们新建一个包比如cpm.chenxiaolani.dao, 然后再新建一个dao文件，一般来说一个表就是一个dao文件，比如针对User表新建UserDao.java文件；
```java
package com.chenxiaolani.dao;

/**
 * 定义操作数据库的方法
 */
public interface UserDao {
    /**
	 * 查询所有
	 */
	void findAll();
	/**
	 * 根据id去更新具体的用户名
	 * @param id
	 * @param name
	 */
	void update(int id , String name);
	/**
     * 删除
    */
	void delete(int id);
	
	/**
	 * 执行添加
	 * @param userName
	 * @param password
	 */
	void insert(String userName , String password);
}
```
* 我们再新建一个包com.chenxiaolani.dao.impl，此包下的文件来实现接口中的方法，对数据库直接操作
```java
package com.chenxiaolani.dao.impl;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import com.chenxiaolani.dao.UserDao;
import com.chenxiaolani.uitl.JDBCUtil;

public class UserDaoImpl implements UserDao{
	@Override
	public void findAll() {
		Connection conn = null;
		Statement st = null;
		ResultSet rs = null;
		try {
			//1. 获取连接对象
			conn = JDBCUtil.getConn();
			//2. 创建statement对象
			st = conn.createStatement();
			String sql = "select * from t_user";
			rs = st.executeQuery(sql);
			while(rs.next()){
				String userName = rs.getString("username");
				String password = rs.getString("password");
				System.out.println(userName+"="+password);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			JDBCUtil.release(conn, st, rs);
		}
	}

	@Override
	public void insert(String userName, String password) {
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			 conn = JDBCUtil.getConn();
			String sql = "insert into t_user values(null , ? , ?)";
			 ps = conn.prepareStatement(sql);
			 //给占位符赋值 从左到右数过来，1 代表第一个
			 ps.setString(1, userName);
			 ps.setString(2, password);
			int result = ps.executeUpdate();
			if(result>0){
				System.out.println("添加成功");
			}else{
				System.out.println("添加失败");
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtil.release(conn, ps);
		}
	}

	@Override
	public void delete(int id) {
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			 conn = JDBCUtil.getConn();
			String sql = "delete from t_user where id = ?";
			 ps = conn.prepareStatement(sql);
			 
			 //给占位符赋值 从左到右数过来，1 代表第一个
			 ps.setInt(1, id);
			int result = ps.executeUpdate();
			if(result>0){
				System.out.println("删除成功");
			}else{
				System.out.println("删除失败");
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtil.release(conn, ps);
		}
	}

	@Override
	public void update(int id, String name) {
		Connection conn = null;
		PreparedStatement ps = null;
		try {
            conn = JDBCUtil.getConn();
			String sql = "update t_user set username=? where id =?";
			 ps = conn.prepareStatement(sql);
			 ps.setString(1, name);
			 ps.setInt(2, id);
			 
			int result = ps.executeUpdate();
			if(result>0){
				System.out.println("更新成功");
			}else{
				System.out.println("更新失败");
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtil.release(conn, ps);
		}
	}
}
```
## 单元测试
* 首先需要添加junit,eclipse已经内置了，右键build path 中add libraries即可
* 然后我们就可以新建一个测试包专门管理我们写的测试文件比如com.chenxiaolani.test
```java
package com.chenxiaolani.test;
import org.junit.Test;
import com.chenxiaolani.dao.UserDao;
import com.chenxiaolani.dao.impl.UserDaoImpl;

public class TestUserDaoImpl {
	
	@Test
	public void testInsert(){
		UserDao dao = new UserDaoImpl();
		dao.insert("aobama", "911");
	}
	
	@Test
	public void testUpdate(){
		UserDao dao = new UserDaoImpl();
		dao.update(2, "chenxiaolani");
	}
	
	@Test
	public void testDelete(){
		UserDao dao = new UserDaoImpl();
		dao.delete(30);
	}

	@Test
	public void testFindAll(){
		UserDao dao = new UserDaoImpl();  
		dao.findAll();
	}
}

```




