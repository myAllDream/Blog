---
title: JDBC
date: 2018-06-30 20:27:17
categories: "Java"
tags:
---



# JDBC

JDBC API 允许用户访问任何形式的表格数据，尤其是存储在关系数据库中的数据。

执行流程：

- 连接数据源，如：数据库。
- 为数据库传递查询和更新指令。
- 处理数据库响应并返回的结果。

<!--more-->

## JDBC 架构

分为双层架构和三层架构。

### 双层

作用：此架构中，Java Applet 或应用直接访问数据源。

条件：要求 Driver 能与访问的数据库交互。

机制：用户命令传给数据库或其他数据源，随之结果被返回。

部署：数据源可以在另一台机器上，用户通过网络连接，称为 C/S配置（可以是内联网或互联网）。

![](http://www.runoob.com/wp-content/uploads/2015/05/Two-tier-Architecture-for-Data-Access.gif)

### 三层

侧架构特殊之处在于，引入中间层服务。

流程：命令和结构都会经过该层。

吸引：可以增加企业数据的访问控制，以及多种类型的更新；另外，也可简化应用的部署，并在多数情况下有性能优势。

历史趋势： 以往，因性能问题，中间层都用 C 或 C++ 编写，随着优化编译器（将 Java 字节码 转为 高效的 特定机器码）和技术的发展，如EJB，Java 开始用于中间层的开发这也让 Java 的优势突显出现出来，使用 Java 作为服务器代码语言，JDBC随之被重视。

![](http://www.runoob.com/wp-content/uploads/2015/05/Three-tier-Architecture-for-Data-Access.gif)

## JDBC 编程步骤

加载驱动程序：

```java
Class.forName(driverClass)
//加载MySql驱动
Class.forName("com.mysql.jdbc.Driver")
//加载Oracle驱动
Class.forName("oracle.jdbc.driver.OracleDriver")
```

获得数据库连接：

```java
DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/imooc", "root", "root");
```

创建Statement\PreparedStatement对象：

```java
conn.createStatement();
conn.prepareStatement(sql);
```

### 完整实例

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class DbUtil {

    public static final String URL = "jdbc:mysql://localhost:3306/imooc";
    public static final String USER = "liulx";
    public static final String PASSWORD = "123456";

    public static void main(String[] args) throws Exception {
        //1.加载驱动程序
        Class.forName("com.mysql.jdbc.Driver");
        //2. 获得数据库连接
        Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
        //3.操作数据库，实现增删改查
        Statement stmt = conn.createStatement();
        ResultSet rs = stmt.executeQuery("SELECT user_name, age FROM imooc_goddess");
        //如果有数据，rs.next()返回true
        while(rs.next()){
            System.out.println(rs.getString("user_name")+" 年龄："+rs.getInt("age"));
        }
    }
}
```

### 增删改查

```java
public class DbUtil {
    public static final String URL = "jdbc:mysql://localhost:3306/imooc";
    public static final String USER = "liulx";
    public static final String PASSWORD = "123456";
    private static Connection conn = null;
    static{
        try {
            //1.加载驱动程序
            Class.forName("com.mysql.jdbc.Driver");
            //2. 获得数据库连接
            conn = DriverManager.getConnection(URL, USER, PASSWORD);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection(){
        return conn;
    }
}

//模型
package liulx.model;

import java.util.Date;

public class Goddess {

    private Integer id;
    private String user_name;
    private Integer sex;
    private Integer age;
    private Date birthday; //注意用的是java.util.Date
    private String email;
    private String mobile;
    private String create_user;
    private String update_user;
    private Date create_date;
    private Date update_date;
    private Integer isDel;
    //getter setter方法。。。
}

//---------dao层--------------
package liulx.dao;

import liulx.db.DbUtil;
import liulx.model.Goddess;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

public class GoddessDao {
    //增加
    public void addGoddess(Goddess g) throws SQLException {
        //获取连接
        Connection conn = DbUtil.getConnection();
        //sql
        String sql = "INSERT INTO imooc_goddess(user_name, sex, age, birthday, email, mobile,"+
            "create_user, create_date, update_user, update_date, isdel)"
                +"values("+"?,?,?,?,?,?,?,CURRENT_DATE(),?,CURRENT_DATE(),?)";
        //预编译
        PreparedStatement ptmt = conn.prepareStatement(sql); //预编译SQL，减少sql执行

        //传参
        ptmt.setString(1, g.getUser_name());
        ptmt.setInt(2, g.getSex());
        ptmt.setInt(3, g.getAge());
        ptmt.setDate(4, new Date(g.getBirthday().getTime()));
        ptmt.setString(5, g.getEmail());
        ptmt.setString(6, g.getMobile());
        ptmt.setString(7, g.getCreate_user());
        ptmt.setString(8, g.getUpdate_user());
        ptmt.setInt(9, g.getIsDel());

        //执行
        ptmt.execute();
    }

    public void updateGoddess(){
        //获取连接
        Connection conn = DbUtil.getConnection();
        //sql, 每行加空格
        String sql = "UPDATE imooc_goddess" +
                " set user_name=?, sex=?, age=?, birthday=?, email=?, mobile=?,"+
                " update_user=?, update_date=CURRENT_DATE(), isdel=? "+
                " where id=?";
        //预编译
        PreparedStatement ptmt = conn.prepareStatement(sql); //预编译SQL，减少sql执行

        //传参
        ptmt.setString(1, g.getUser_name());
        ptmt.setInt(2, g.getSex());
        ptmt.setInt(3, g.getAge());
        ptmt.setDate(4, new Date(g.getBirthday().getTime()));
        ptmt.setString(5, g.getEmail());
        ptmt.setString(6, g.getMobile());
        ptmt.setString(7, g.getUpdate_user());
        ptmt.setInt(8, g.getIsDel());
        ptmt.setInt(9, g.getId());

        //执行
        ptmt.execute();
    }

    public void delGoddess(){
        //获取连接
        Connection conn = DbUtil.getConnection();
        //sql, 每行加空格
        String sql = "delete from imooc_goddess where id=?";
        //预编译SQL，减少sql执行
        PreparedStatement ptmt = conn.prepareStatement(sql);

        //传参
        ptmt.setInt(1, id);

        //执行
        ptmt.execute();
    }

    public List<Goddess> query() throws SQLException {
        Connection conn = DbUtil.getConnection();
        Statement stmt = conn.createStatement();
        ResultSet rs = stmt.executeQuery("SELECT user_name, age FROM imooc_goddess");

        List<Goddess> gs = new ArrayList<Goddess>();
        Goddess g = null;
        while(rs.next()){
            g = new Goddess();
            g.setUser_name(rs.getString("user_name"));
            g.setAge(rs.getInt("age"));

            gs.add(g);
        }
        return gs;
    }

    public Goddess get(){
        Goddess g = null;
        //获取连接
        Connection conn = DbUtil.getConnection();
        //sql, 每行加空格
        String sql = "select * from  imooc_goddess where id=?";
        //预编译SQL，减少sql执行
        PreparedStatement ptmt = conn.prepareStatement(sql);
        //传参
        ptmt.setInt(1, id);
        //执行
        ResultSet rs = ptmt.executeQuery();
        while(rs.next()){
            g = new Goddess();
            g.setId(rs.getInt("id"));
            g.setUser_name(rs.getString("user_name"));
            g.setAge(rs.getInt("age"));
            g.setSex(rs.getInt("sex"));
            g.setBirthday(rs.getDate("birthday"));
            g.setEmail(rs.getString("email"));
            g.setMobile(rs.getString("mobile"));
            g.setCreate_date(rs.getDate("create_date"));
            g.setCreate_user(rs.getString("create_user"));
            g.setUpdate_date(rs.getDate("update_date"));
            g.setUpdate_user(rs.getString("update_user"));
            g.setIsDel(rs.getInt("isdel"));
        }
        return g;
    }
}
```

## 处理大数据对象

1. 二者都是先获取一个文件
2. 创建一个流
3. 预编译sql语句设置
4. 通过result结果集获取数据

### 处理CLOB(Character large object)

```java
File context=book.getContext(); // 获取文件
InputStream inputStream=new FileInputStream(context);
void setAsciiStream(int parameterIndex, InputStream x, context.length) 
Clob c=result.getClob("context");
```

### 处理BLOB(Binary large object)

```java
File context=book.getContext(); // 获取文件
InputStream inputStream=new FileInputStream(pic);
void setBinaryStream(int parameterIndex, InputStream x,pic.length) 
Blob b=result.getBlob("pic");
```

## Callable Statement接口调用存储过程

存储过程：

```mysql
DELIMITER &&
CREATE PROCEDURE pro_getBookNameById(IN bookId INT,OUT bN VARCHAR(20))
 BEGIN
	SELECT bookName INTO bn FROM t_book WHERE id=bookId;
 END 
&&
DELIMITER ; 

CALL pro_getBookNameById(10,@bookName);
SELECT @bookName;
```

### 示例方法，其他的举一反三

- ```java
  void registerOutParameter(int parameterIndex,int sqlType)
  ```

  - 获取接口实例和获取prepared Statement一样
  - 存储过程带{}**？**--有点不同于普通sql语句
  - 具体到时候用到再查API

```java
String sql="{CALL pro_getBookNameById(?,?)}";
CallableStatement cstmt=con.prepareCall(sql);
cstmt.setInt(1, id); // 设置第一个参数
cstmt.registerOutParameter(2, Types.VARCHAR);  // 设置返回类型
cstmt.execute();
```

## 使用元数据分析数据库

**用时候查API**

- DatabaseMetaData接口获取数据库基本信息
  - 通过connection的get方法获取
- ResultSetMetaData接口获取ResultSet对象中的信息
  - 通过prepareStatement的get方法获取
  - 获取Result中的列数
  - 获取指定列的名称
  - 获取指定SQL类型名称

## JDBC事务处理

### 1.事物的概念

事务处理在数据库开发中有着非常重要的作用，所谓事务就是所有的操作要么一起成功，要么一起失败，事务 本身具有原子性（Atomicity）、一致性（Consistency）、隔离性或独立性（Isolation） 、持久性（Durability）4 个特 性，这 4 个特性也被称为 **ACID** 特征。 **JDBC中每个connection就是一个事物**

- **原子性**：原子性是事务最小的单元，是不可再分隔的单元，相当于一个个小的数据库操作，这些操作必须同时 成功，如果一个失败了，则一切的操作将全部失败。 
- **一致性**：指的是在数据库操作的前后是完全一致的，保证数据的有效性，如果事务正常操作则系统会维持有效 性，如果事务出现了错误，则回到最原始状态，也要维持其有效性，这样保证事务开始时和结束时系统处于一 致状态。 
- **隔离性**：多个事务可以同时进行且彼此之间无法访问，相互独立，只有当事务完成最终操作时，才可以看到结果； 
- **持久性**：事务完成之后，它对于系统的影响是永久性的。该修改即使出现致命的系统故障也将一直保持。

### 2.Mysql对事物的支持

| 序号 | 命令                             | 描述                                  |
| :--: | -------------------------------- | ------------------------------------- |
|  1   | SETAUTOCOMMIT=0                  | 取消自动提交处理，开启事务处理        |
|  2   | SETAUTOCOMMIT=1                  | 打开自动提交处理，关闭事务处理        |
|  3   | STARTTRANSACTION                 | 启动事务                              |
|  4   | BEGIN                            | 启动事务，相当于执行 STARTTRANSACTION |
|  5   | COMMIT                           | 提交事务                              |
|  6   | ROLLBACK                         | 回滚全部事物                          |
|  7   | SAVEPOINT 事务保存点名           | 设置事务保存点                        |
|  8   | ROLLBACK TO SAVEPOINT 保存点名称 | 回滚操作到保存点                      |

### 3.JDBC事物处理

```java
connection.setAutoCommit(false);  //自动提交设置为false
connection.rollback() //在抛异常前进行回滚
connection.commit() //在finally中进行事物提交
```

### 4.事物保存点

事物回滚的点。