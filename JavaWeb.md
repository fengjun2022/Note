---
 typora-root-url: image
---

#                              JavaWeb



## **Springboot注解**

```java

                                                    注解
@Retention()
//声明注解，指定注解什么时候生效@Retention(RetentionPolicy.RUNTIME)

@Target
//指定注解在什么类上生效 @Target(ElementType.METHOD) 

                                                Spring Boot IOC
@Controller
//注解表明了这个类是一个控制器类 该类代表控制器类(控制层/表现层)。这里控制层里面的每个方法，都可以去调用      @Service标识的类（业务逻辑层）@Service标识的类中的方法可以继续调用@Resposity标识的接口实现类（Dao层/持久层）
    
@RestController
//接口请求处理注解 等同于@Controller + @ResponseBody

@RequestMapping 
//配合RestController使用实现接口控制

@ResponseBody
//解的作用是将Controller的方法返回的对象，通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据。

@Component
//定义Spring管理Bean（也就是将标注@Component注解的类交由spring管理）

@Service
//@Service注解用于类上，标记当前类是一个service类，加上该注解会将当前类自动注入到spring容器中

@Repository
// 标注在数据访问类上

@Autowired
//运行时ioc容器会注入相对应的bean对象，并赋值

@Primary
// 确认使用哪一个bean，该注解加载当前需要生效的bean上

@Qualifier
//按照名字装配Bean

@Resource
//@Resource默认按照名字装配Bean，即会按照name属性的值来找到具有相同id的Bean Definition  并注入

@Mapper 
// 程序运行会自动生成该接口的实现类对象（代理对象），并且将该对象交给IOC容器管理

@SpringBootTest
// 测试文件中声明该注解既可以开始测试

@Test
//在需要测试的函数上声明2

@Getter
//Lombok注解，为所有属性提供get方法

@Setter
 //Lombok注解，为所有属性提供set方法

@ToString
 //Lombok注解，生成string方法

@EqualsAndHashCode
 //Lombok注解，为所有字段重写equals和HashCode方法

@Data
// Lombok注解 综合了@Getter @Setter @EqualsAndHashCode 注解所有功能


@NoArgsConstructor
//Lombok注解 生成一个无参构造

@AllArgsConstructor
//Lombok注解 生成处static以外，其他各参数的有参构造

 @Delete
// mybatis注解 dao层操控数据库删除

 @Insert
// mybatis注解 dao层操控数据库新增

 @Options
//  mybatis注解 dao层操控主键返回 @Options(useGeneratedKeys = true,keyProperty = "id")

 @Update
// mybatis注解 dao层操控数据库更新

 @Select
 // mybatis注解 dao层操控数据库查询

 @Slf4j
 //日志  log.info()

 @PathVariable 
 // 路径请求参数解析

@RequestMapping
//抽取为公共路径

@GetMapping
// 接受get请求

@PostMapping
// 接受post请求

@DeleteMapping
// 接受post请求

@RequestParam
// 设置请求参数默认值

 @DateTimeFormat
// 指定前端传递的日期格式  @DateTimeFormat(pattern = "yyyy-MM-dd")

@Value
// Value注解常用于外部配置的属性注入，具体用法为@Value("${配置文件中的key}")

@ConfigurationProperties
// yml文件配置自动一键注入，但是类内同时需要get set

                                         过滤器注解

@WebFilter
//过滤器拦截的注解 配置拦截哪些请求 ，@WebFilter(urlPatterns = /*)

@ServletComponentScan
// sping boot 入口类加上，表示支持当前javaWeb过滤器 filter 拦截需要搭配该注解，

                                            拦截器注解

@Configuration 
// 代表当前类是一个拦截器配置类

@RestControllerAdvice 
// 全局异常处理器

@ExceptionHandler(Exception.class) 
// 表示捕获捕获所有异常

@Transactional
// 将当前方法交给spring进行事务管理，方法执行前开启事务，成功执行完毕，提交事务，出现异常回滚事务 @Transactional(rollbackFo=Exception)表示出现所有异常都进行回滚 


                                            AOP注解

@Aspect
//AOP类

@Around
// AOP类作用于哪些方法的注解。@Around("execution(* com.itheima.service."*"(..))")

@Pointcut
// 切入点表达式，如果多个@Around切入点一致则可以使用@Pointcut("execution(* com.itheima.service."*"(..))") 而@Around只需要输入@Around("pt()")
                                             
 @Order
 // 指定AOP类执行顺序  @Order(1)
                                                        
 
                                                        
  ----------------------------------------------------------------                                                                    AOC                           
  @Socpe
//  配置bean作用域，可以让Bean成为非单例对象
  
  @Lazy
 // 实例延迟加载 ，会让实例第一次调用时加载                                                 
  
 @Configuration
   // 表明当前是一个配置类           
                                                        
  @Import
 // 导入一个第三方配置类，交给IOC容器管理  @Import（{xxxx.class}）
```











![image-20230719212903654](/image-20230719212903654.png)



## web接口

![](C:\Users\32784\AppData\Roaming\Typora\typora-user-images\image-20230627003833902.png)

```java


package com.example.demo.controller;

import com.example.demo.user.User;
import com.example.demo.user.userInfo;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.*;

import java.sql.Time;
import java.time.LocalDateTime;

@RestController
public class resquestConroller {
    // 简单参数
    @RequestMapping("/User")
    public String User(@RequestParam(name="name") String userName, int age){

        System.out.println(userName+":"+age);
        return "名字"+userName+":"+"年龄"+age;
    }

 //实体参数
    @RequestMapping("/userInfo")
    public Object UserInfo(userInfo userInfo){
        System.out.println(userInfo+"---");
        return userInfo;
    }

  //  日期参数
@RequestMapping("/upDateTime")
    public LocalDateTime upDateTime(@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss" ) LocalDateTime time){
    System.out.println(time);
    return time;
}

    // json参数
    
@RequestMapping("/dataJson")
public Object jsonData (@RequestBody User user){
    System.out.println(user);
    return user;
}

   // 路径参数
@RequestMapping("/path/{id}")
    public String lj (@PathVariable Integer id){
    System.out.println(id);
    return "hello world";
}


}

```





## 分层解耦与依赖注入

![image-20230709100442427](/image-20230709100442427-16888685882822.png)



#### IOC与DI步骤

![image-20230709100928283](/image-20230709100928283.png)





#### Bean的声明

![image-20230709101358416](/image-20230709101358416.png)



#### Bean组件扫描

![image-20230709101931110](/image-20230709101931110.png)

![image-20230709102234433](/image-20230709102234433.png)



#### Bean注入 & @Autowired注解

![image-20230709104224295](/image-20230709104224295.png)

![image-20230709104404187](/image-20230709104404187.png)

通过@Primary生效，在需要生效的类上加上@Primary

![image-20230709104544638](/image-20230709104544638.png)



 通过 @Autowired @Qualifier生效(按照类型进行注入)

![image-20230709104844574](/image-20230709104844574.png)



通过@Resource

![image-20230709105109878](/image-20230709105109878.png)

![image-20230709105234191](/image-20230709105234191.png)

![image-20230709105257248](/image-20230709105257248.png)



## MySQL



#### SQL简介

![image-20230709153938611](/image-20230709153938611.png)



SQL分类

![image-20230709154153710](/image-20230709154153710.png)

#### DDL数据库操作

![image-20230709160527035](/image-20230709160527035.png)





#### 约束

![image-20230709181247973](/image-20230709181247973.png)

示例

```sql
 create table tb_user2(
id int primary key auto_increment comment   '用户ID',
username varchar(20) not null unique comment '用户名',
name varchar(10) not null comment  '姓名',
age int comment  '年龄',
gender char(1) default '男' comment '性别'
 ) comment '用户表';
```





#### 常见数据类型



##### 数值类型

| 类型      | 大小(byte) | 有符号(SIGNED)范围                                    | 无符号(UNSIGNED)范围                                       | 描述             | 备注                                               |
| --------- | ---------- | ----------------------------------------------------- | ---------------------------------------------------------- | ---------------- | -------------------------------------------------- |
| tinyint   | 1          | (-128，127)                                           | (0，255)                                                   | 小整数值         |                                                    |
| smallint  | 2          | (-32768，32767)                                       | (0，65535)                                                 | 大整数值         |                                                    |
| mediumint | 3          | (-8388608，8388607)                                   | (0，16777215)                                              | 大整数值         |                                                    |
| int       | 4          | (-2147483648，2147483647)                             | (0，4294967295)                                            | 大整数值         |                                                    |
| bigint    | 8          | (-2^63，2^63-1)                                       | (0，2^64-1)                                                | 极大整数值       |                                                    |
| float     | 4          | (-3.402823466 E+38，3.402823466351  E+38)             | 0 和 (1.175494351  E-38，3.402823466 E+38)                 | 单精度浮点数值   | float(5,2)：5表示整个数字长度，2  表示小数位个数   |
| double    | 8          | (-1.7976931348623157 E+308，1.7976931348623157 E+308) | 0 和  (2.2250738585072014 E-308，1.7976931348623157 E+308) | 双精度浮点数值   | double(5,2)：5表示整个数字长度，2  表示小数位个数  |
| decimal   |            |                                                       |                                                            | 小数值(精度更高) | decimal(5,2)：5表示整个数字长度，2  表示小数位个数 |

默认有符号如果需要无符号则要加  tinyint unsigned



##### 字符串类型

| 类型       | 大小                  | 描述                         |      |        |          |
| ---------- | --------------------- | ---------------------------- | ---- | ------ | -------- |
| char       | 0-255 bytes           | 定长字符串                   | AB   | 性能高 | 浪费空间 |
| varchar    | 0-65535 bytes         | 变长字符串                   | ABC  | 性能低 | 节省空间 |
| tinyblob   | 0-255 bytes           | 不超过255个字符的二进制数据  |      |        |          |
| tinytext   | 0-255 bytes           | 短文本字符串                 |      |        |          |
| blob       | 0-65 535 bytes        | 二进制形式的长文本数据       |      |        |          |
| text       | 0-65 535 bytes        | 长文本数据                   |      |        |          |
| mediumblob | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据 |      |        |          |
| mediumtext | 0-16 777 215 bytes    | 中等长度文本数据             |      |        |          |
| longblob   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据     |      |        |          |
| longtext   | 0-4 294 967 295 bytes | 极大文本数据                 |      |        |          |

char(10)最多只能存10个字符，不足10个字符，则占用10个字符空间

varchat(10)最多只能存10个字符，不足10个字符，按照实际长度存储



##### 日期时间类型

| 类型      | 大小(byte) | 范围                                       | 格式                | 描述                     |
| --------- | ---------- | ------------------------------------------ | ------------------- | ------------------------ |
| date      | 3          | 1000-01-01 至  9999-12-31                  | YYYY-MM-DD          | 日期值                   |
| time      | 3          | -838:59:59 至  838:59:59                   | HH:MM:SS            | 时间值或持续时间         |
| year      | 1          | 1901 至 2155                               | YYYY                | 年份值                   |
| datetime  | 8          | 1000-01-01 00:00:00 至 9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| timestamp | 4          | 1970-01-01 00:00:01 至 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |





### DDL表操作

![image-20230709212347301](/image-20230709212347301.png)

![image-20230709212403052](/image-20230709212403052.png)

![image-20230709213606357](/image-20230709213606357.png)





### DML



#### insert into （添加字段）

![image-20230709214102424](/image-20230709214102424.png)

```sql
 #指定字段
 insert into table_name(id, username, name, gender, image, job, entrydate, create_time, update_time) values (null,'平泽呆呆唯2','平泽唯',2,null,null,'2009-11-27',now(),now())
 
 #所有字段
 insert into table_name
 values (null, '田井中律', '田中律', 2, null, null, '2009-11-27', now(), now()),
        (null, 'mio', '秋山澪', 2, null, null, '2009-11-27', now(), now());
 update table_name
```



#### update （修改字段）![image-20230709222528486](/image-20230709222528486.png)



```sql
 update table_name
 set username        = '中野悻',
     update_time = now()
 where id = 2;


```



#### delete（删除）

![image-20230709223846076](/image-20230709223846076.png)

![image-20230709224456625](/image-20230709224456625.png)



### DQL

![image-20230711211830023](/image-20230711211830023.png)





#### DQL基本查询

![image-20230711212211224](/image-20230711212211224.png)

#### DQL条件查询

![image-20230711213408173](/image-20230711213408173.png)

```sql
select  * from  tb_user2 where id between 1 and '2';查询范围内的
select  * from tb_user2 where  id != 1; 查询id不等于1
select  * from tb_user2 where age is null ;查询为null的
select  * from tb_user2 where id in (1,3);查询多少到多少的
select  * from tb_user2 where  name like '__'; // 查询两个字的
select  * from tb_user2 where  username like '平泽%' 模糊查询。以xx%开头的
```



#### DQL聚合函数

![image-20230711220255740](/image-20230711220255740.png)

```sql
select  count(*) from tb_user2;
select  min(id) from tb_user2;
select  max(id) from tb_user2;
select  avg(id) from tb_user2;
select  sum(id) from tb_user2;

```





#### DQL分组查询

![image-20230711222624837](/image-20230711222624837.png)

```sql
select  id,count(*) from tb_user2 where id > 2 group by id having  count(*)=1; having 分组之后的过滤条件
```



#### DQL排序查询

![image-20230711222759137](/image-20230711222759137.png)

```sql
select * from tb_user2 order by id asc;升序
select * from tb_user2 order by id desc ;降序
```

![image-20230711223918128](/image-20230711223918128.png)

![image-20230711223957402](/image-20230711223957402.png)



#### DQL分页查询

![image-20230711224518703](/image-20230711224518703.png)

```sql
select * from tb_user2 limit 0,5;
select * from tb_user2 limit 5,5;


#起始索引 = （页码 - 1 * 每页展示数）

```

**起始索引公式 = （页码 - 1 * 每页展示数）**





#### 流程控制函数

![image-20230712232602845](/image-20230712232602845.png)





#### 查询语句书写顺序

![image-20230712232752409](/image-20230712232752409.png)





### 多表设计

#### 外键

![image-20230713210110407](/image-20230713210110407.png)

![image-20230713211403782](/image-20230713211403782.png)

多对多

![image-20230713214001683](/image-20230713214001683.png)

![image-20230713225106152](/image-20230713225106152.png)





### 多表查询

![image-20230714214905808](/image-20230714214905808.png)

![image-20230714215049018](/image-20230714215049018.png)



#### 内连接

![image-20230714215145781](/image-20230714215145781.png)

```sql
--隐式内连接
select tb_emp.name,tb_dept.name from tb_emp ,tb_dept where  tb_dept.id=tb_emp.dept_id;

--显式内连接
select tb_emp.name,tb_dept.name from tb_emp inner join tb_dept  on tb_dept.id=tb_emp.dept_id;

重命名

select e.name,d.name from tb_emp e ,tb_dept d where  d.id=e.dept_id;
```



#### 外连接



![image-20230714220805189](/image-20230714220805189.png)





```sql
#左外链接
select e.name,d.name from tb_emp e left join  tb_dept d on e.dept_id = d.id
#右外链接
select e.name,d.name from tb_emp e right join  tb_dept d on e.dept_id = d.id

```





#### 子查询

![image-20230715172036720](/image-20230715172036720.png)

![image-20230715172115748](/image-20230715172115748.png)

```sql
select * from tb_emp where dept_id = (select id from tb_dept where name = '教研部');
```




![image-20230715173759362](/image-20230715173759362.png)



**列子查询**

```sql
select  * from  tb_emp where dept_id in ((select id from tb_dept where name='教研部' or name='咨询部'));
```



**行子查询与表子查询**

![image-20230715180353400](/image-20230715180353400.png)



**行子查询**

```sql
select * from tb_emp where (entrydate,job ) = (select entrydate,job from tb_emp where name = '韦一笑');
```



**表子查询**

```sql
select e.*,d.name from (select  * from tb_emp where entrydate > '2006-01-01') e ,tb_dept d where e.dept_id = d.id;
```







### 事务

![image-20230716001232701](/image-20230716001232701.png)



#### 事务的四大特性





![image-20230716003930349](/image-20230716003930349.png)

![image-20230716004137586](/image-20230716004137586.png)



### 索引

![image-20230716013040192](/image-20230716013040192.png)



![image-20230716014438346](/image-20230716014438346.png)



#### 结构

![image-20230716013826641](/image-20230716013826641.png)



#### B+Tree多路平衡搜索树

![image-20230716014258829](/image-20230716014258829.png)















## Mybatis

![image-20230716154343318](/image-20230716154343318.png)







### mybatis使用

1.application.properties定义连接数据库语句

```properties
#驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#数据库连接的url
spring.datasource.url=jdbc:mysql://localhost:3306/db06?user=user100w
#连接数据库的用户名
spring.datasource.username=root
#连接数据库的密码
spring.datasource.password=123456


```



2.创建查询接口

@Mapper // 程序运行会自动生成该接口的实现类对象（代理对象），并且将该对象交给IOC容器管理,成为ioc容器内的bin

```java
@Mapper // 程序运行会自动生成该接口的实现类对象（代理对象），并且将该对象交给IOC容器管理
public interface  UserMapper {

    @Select("select * from user100w ") 查询语句
    public List<User> list();
}

```



3.MybetisApplicationTests文件中测试

```java
@SpringBootTest
class MybetisApplicationTests {

    @Autowired springI/O容器进行依赖注入
    private UserMapper userMapper;

    @Test
 public  void  testListUser(){
        List<User> userList = userMapper.list();
        userList.stream().forEach(user -> System.out.println(user));
    }

}

```





### 数据库连接池

![image-20230716214654404](/image-20230716214654404.png)





![image-20230716215350704](/image-20230716215350704.png)

![image-20230716220000941](/image-20230716220000941.png)





### lombok

![image-20230716221428376](/image-20230716221428376.png)



### mybatis基本操作

#### **删除**

```java
//定义接口
    @Delete("delete from emp where  id = #{id}")
    public void del (Integer id);
}


// 调用

@SpringBootTest
class MtvetisDemoApplicationTests {
    @Autowired
private mapper mapper;

    @Test
   public void delet (){
mapper.del(14);
    };

}

```





**日志输出**

![image-20230717213709757](/image-20230717213709757.png)





#### 参数占位符

![image-20230717215229427](/image-20230717215229427.png)

![image-20230717215331442](/image-20230717215331442.png)



#### 新增

![image-20230717223111450](/image-20230717223111450.png)

```java
   @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) " +
            "values (#{username},#{name},#{gender},#{image},#{job},#{entrydate},#{deptId},#{createTime},#{updateTime}) ")

    public void insert(Emp emp);
```





#### 主键返回

```java
  增加@Options注解  @Options(useGeneratedKeys = true,keyProperty = "id")
    @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) " +
            "values (#{username},#{name},#{gender},#{image},#{job},#{entrydate},#{deptId},#{createTime},#{updateTime}) ")

    public void insert(Emp emp);
```





#### 更新

![image-20230717230722092](/image-20230717230722092.png)

```java
    @Update("update emp set username =#{username},name=#{name},gender=#{gender},image=#{image},job=#{job},entrydate=#{entrydate},dept_id=#{deptId},update_time=#{updateTime} where id=#{id}")
    public void update(Emp emp);

```





#### 查询

![image-20230717231547389](/image-20230717231547389.png)



```java
 @Select("select * from emp where id=#{id}")
    public Emp select(Integer id);
```



**mybetis自动封装**

![image-20230717231811729](/image-20230717231811729.png)



#### 数据封装：类型装填



```java
开启mybatis驼峰命名法

mybatis.configuration.map-underscore-to-camel-case=true
```

![image-20230717233858862](/image-20230717233858862.png)





#### 条件查询

![image-20230718000656686](/image-20230718000656686.png)

![image-20230718000616648](/image-20230718000616648.png)

```java
使用concat
@Select("select * from emp where name like concat('%',#{name},'%') and gender =#{gender} and entrydate between #{begin} and  #{end} order by  update_time desc ")
 public List<Emp> selectName(String name, Integer gender, LocalDate begin,LocalDate end);

使用${}
@Select("select * from emp where name like  '%${name}%'  and gender =#{gender} and entrydate between #{begin} and  #{end} order by  update_time desc ")
 public List<Emp> selectName(String name, Integer gender, LocalDate begin,LocalDate end);
```







#### XML映射文件

![image-20230718001027442](/image-20230718001027442.png)



#### 配置xml映射

resources 下创建同类名包

创建同接口名xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
以上为映射
namespace mapper类的全类名
<mapper namespace="com.itheima.Mapper.mapper" >
    id与mapper内定义的接口同名，resultType返回的类型，与接口定义的类型一致
<select id="selectName" resultType="com.itheima.pojo.Emp">
    #sql语句
  select * from emp where name like '%${name}%' and gender =#{gender} and
                          entrydate between #{begin} and  #{end} order by  update_time desc
</select>
</mapper>

```





#### 动态sql

![image-20230718003848133](/image-20230718003848133.png)





##### <if>标签

![image-20230718005247407](/image-20230718005247407.png)

```xml
  select * from emp
      <where>
    <if test="name!=null">
        name like '%${name}%'
    </if>
    <if test="gender!=null">
        and gender =#{gender}
    </if>
    <if test="begin !=null and end !=null">
        and
        entrydate between #{begin} and  #{end}
    </if>
    order by  update_time desc
</where>
```





set标签

![image-20230718011153198](/image-20230718011153198.png)

```xml
   <update id="update">
        update emp
      <set>
          <if test="username!=null"> username   =#{username},</if>
          <if test="name!=null">  name=#{name},</if>
          <if test="gender!=null">  gender=#{gender},</if>
          <if test="image!=null">  image=#{image},</if>
          <if test="job!=null">  job=#{job},</if>
          <if test="entrydate!=null">  entrydate=#{entrydate},</if>
          <if test="deptId!=null">  dept_id=#{deptId},</if>
          <if test="updateTime!=null">  update_time=#{updateTime}</if>
          where id = #{id}
      </set>


    </update>

```





#### 小结

![image-20230718011341181](/image-20230718011341181.png)





#### foreach



![image-20230718013416685](/image-20230718013416685.png)



```xml
<!--   collection 要遍历的集合 -->
<!--   item 遍历出来的元素-->
<!--    sparator 分隔符-->
<!--   open遍历开始前拼接的SQL片段-->
<!--   close遍历结束后拼接的SQL片段-->

    <delete id="deleteByIds">
        delete from emp where id in
                        <foreach collection="ids" item="id" separator="," open="(" close=")">
                          #{id}
                        </foreach>
    </delete>

```





### 代码抽取封装与复用

![image-20230718013809754](/image-20230718013809754.png)









## 案例



**日志记录**



```java
    private static Logger log = LoggerFactory.getLogger(DeptControlle.class);

或者是
@Slf4j

```





**指定请求方式**

```java
    @RequestMapping(value = "/depts",method = RequestMethod.GET)
or
    @GetMapping("/depts")


```



**接受请求参数**

```java
 @PostMapping("/depts/{id}")
    public Result delete (@PathVariable Integer id){
        log.info("删除数据部门");

        return Result.success();
    };


```



接受json格式传参

```java
@PostMapping("/depts")
public Result addDepts(@RequestBody Dept dept){
    
    return Result.success();
}
```

抽取为公共路径

```java
@RequestMapping("/depts")

```

分页查询

```java
  @Select("select * from emp limit #{start},#{pageSize}")
    public List<Emp> page(Integer start , Integer pageSize);
```

![image-20230721224025204](/image-20230721224025204.png)



分页查询插件使用方法

![image-20230721225742650](/image-20230721225742650.png)



动态sql

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">
    <select id="page" resultType="com.itheima.pojo.Emp">
          select * from emp
      <where>
          <if test="name != null">
              name like  concat('%',#{name},'%')
          </if>
          <if test="gender != null">
              and gender = #{gender}
          </if>
          <if test="begin != null and end !=null ">
              and entrydate between #{begin} and #{end}

          </if>
      </where>
          order by update_time desc
    </select>
</mapper>
```





本地存储

![image-20230723184209704](/image-20230723184209704.png)

```java
@PostMapping("/upload")
    public Result upload(String username, Integer age , MultipartFile image)throws Exception {
        log.info(username,age,image);
        // 获取原始文件名
        String or = image.getOriginalFilename();
    // 将文件存储在服务器磁盘的目录中
//        image.transferTo(new File("D:\\下载"+or));
    image.transferTo(
            new File("D:\\下载\\"+or)
    );
    return Result.success();

    }
    
```

**配置上传文件大小**

![image-20230723185146238](/image-20230723185146238.png)

![image-20230723185649290](/image-20230723185649290.png)





**文件上传服务器**



```java
@Component
public class AliOSSUtils {

    private String endpoint =  "https://oss-cn-wulanchabu.aliyuncs.com";
    private String accessKeyId =  "LTAI5tShnxDAVkvsmp4v6tTT";
    private String accessKeySecret = "uEpr4F7mtfqRFFKgQ0YVyOXNS5pzhp";
    private String bucketName = "fenghun";

    /**
     * 实现上传图片到OSS
     */
    public String upload(MultipartFile file) throws IOException {
        // 获取上传的文件的输入流
        InputStream inputStream = file.getInputStream();

        // 避免文件覆盖
        String originalFilename = file.getOriginalFilename();
        String fileName = UUID.randomUUID().toString() + originalFilename.substring(originalFilename.lastIndexOf("."));

        //上传文件到 OSS
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        ossClient.putObject(bucketName, fileName, inputStream);

        //文件访问路径
        String url = endpoint.split("//")[0] + "//" + bucketName + "." + endpoint.split("//")[1] + "/" + fileName;
        // 关闭ossClient
        ossClient.shutdown();
        return url;// 把上传到oss的路径返回
    }

}

```





## Spring Boot配置文件

![image-20230724215029256](/image-20230724215029256.png)



#### 阿里云配置

```java
//导入
@Value("${aliyun.oss.endpoint}")
    private String endpoint;

    @Value("${aliyun.oss.accessKeyId}")
    private String accessKeyId;

    @Value("${aliyun.oss.accessKeySecret}")
    private String accessKeySecret;

    @Value("${aliyun.oss.bucketName}")
    private String bucketName ;

```







```properties
#阿里云配置文件
aliyun.oss.endpoint = https://oss-cn-wulanchabu.aliyuncs.com
aliyun.oss.accessKeyId =  LTAI5tShnxDAVkvsmp4v6tTT
aliyun.oss.accessKeySecret = uEpr4F7mtfqRFFKgQ0YVyOXNS5pzhp
aliyun.oss.bucketName = fenghu
```





### **yml**

![image-20230724224300296](/image-20230724224300296.png)



定义数组和对象

![image-20230724224740423](/image-20230724224740423.png)







## 会话跟踪

![image-20230729170059749](/image-20230729170059749.png)



## JWT

![image-20230729170906238](/image-20230729170906238.png)

![image-20230729171753057](/image-20230729171753057.png)







**生成jwt**

```java
 public  void  testGenJwt(){
        Map<String, Object> claims = new HashMap<>();
        claims.put("id", 1);
        claims.put("name", "tom");
        String jwt = Jwts.builder()
                .signWith(SignatureAlgorithm.HS256, "itheima") // 签名算法
                .setClaims(claims) // 自定义内容
                .setExpiration(new Date(System.currentTimeMillis() + 3600 * 1000)) // 设置有效期为一小时
                .compact();
        System.out.println(jwt);


    }

```



**解析jwt**

```java
    public  void  testParseJwt (){
    Claims claims = Jwts.parser()
            .setSigningKey("itheima") // 签名算法
            .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoidG9tIiwiaWQiOjEsImV4cCI6MTY5MDYyNzAzMn0.10DzZiCEjoba2AkotB4VV7sxtShggtzdZX3zYn4jgvw")
            .getBody(); // 获取内容
    System.out.println(claims);
}
```





## 过滤器

![image-20230729212309384](/image-20230729212309384.png)

![image-20230729212638705](/image-20230729212638705.png)



```java
package com.itheima.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter(urlPatterns = "/*") // 表示匹配所有url
public class DemoFilter implements Filter {
    @Override // 初始化方法，只调用一次


    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("// 初始化方法");
        Filter.super.init(filterConfig);
    }

    @Override // 拦截到请求之后都会调用，调用多次
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
// 调用放行方法，传递请求对象，响应对象
        System.out.println("拦截执行");
        filterChain.doFilter(servletRequest,servletResponse);

    }

    @Override // 销毁方法，只执行一次
    public void destroy() {
        Filter.super.destroy();
    }
}



入口类加上 @ServletComponentScan 注解

```

![image-20230729215947601](/image-20230729215947601.png)

**执行逻辑**

![image-20230729220518167](/image-20230729220518167.png)





**拦截路径**

![image-20230729220609192](/image-20230729220609192.png)



**过滤器链**

![image-20230729221210558](/image-20230729221210558.png)

![image-20230729221757177](/image-20230729221757177.png)







## 拦截器

![image-20230730024221989](/image-20230730024221989.png)

![image-20230730024415382](/image-20230730024415382.png)

![image-20230730024443703](/image-20230730024443703.png)





**定义拦截器**

```java
//*
//第一步定义拦截器
// */
// 拦截器要实现HandlerInterceptor内方法
@Component // 需要交给ioc容器管理
public class LoginChecklnterceptor implements HandlerInterceptor {
    @Override // 目标资源运行前运行，返回true 放行，返回false不放行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle 运行");
        return true;
    }

    @Override // 目标资源方法运行后运行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

        System.out.println("postHandle...");


    }

    @Override // 视图渲染完毕后运行，最后运行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion 运行");
    }
}

```



**第二步配置拦截器**

```java
/*
第二步
定义配置类
* */
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Autowired
    private LoginChecklnterceptor loginChecklnterceptor;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        /*定义拦截器配置*/
        registry.addInterceptor(loginChecklnterceptor).addPathPatterns("/**");
    }
}

```





**拦截器路径配置**

![image-20230730030429103](/image-20230730030429103.png)



**拦截器执行流程**

![image-20230730030909632](/image-20230730030909632.png)





## 全局异常处理器

![image-20230730145019332](/image-20230730145019332.png)

```java
@RestControllerAdvice // 全局异常处理器

public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class) // 表示捕获捕获所有异常
public Result ex (Exception ex) {
    ex.printStackTrace();
    return Result.error("对不起，操作失败");
}

}

```







## 事务管理

![image-20230730151943453](/image-20230730151943453.png)

默认情况下运行时异常才会出现事务回滚  



## 事务传播行为

![image-20230730154505953](/image-20230730154505953.png)

**事务传播就是，当一个事务方法调用了另一个具有事务的方法**

![image-20230730154558696](/image-20230730154558696.png)



开启新事务

![image-20230730155338258](/image-20230730155338258.png)





## AOP基础

![image-20230730161154115](/image-20230730161154115.png)

![image-20230730161247443](/image-20230730161247443.png)

![image-20230730161447170](/image-20230730161447170.png)

![image-20230730162220110](/image-20230730162220110.png)

![image-20230730162652322](/image-20230730162652322.png)

![image-20230730163210538](/image-20230730163210538.png)



### 通知类型

![image-20230730165401541](/image-20230730165401541.png)



@PointCut

![image-20230730170149922](/image-20230730170149922.png)

### **通知顺序**

![image-20230730170910356](/image-20230730170910356.png)



### 切入点表达式

![image-20230730172241878](/image-20230730172241878.png)

### execution

![image-20230730172341290](/image-20230730172341290.png)

![image-20230730173644368](/image-20230730173644368.png)

![image-20230730173725472](/image-20230730173725472.png)



### 注解

![image-20230730174105357](/image-20230730174105357.png)



### 获取连接点参数

![image-20230730175047447](/image-20230730175047447.png)

![image-20230730175303619](/image-20230730175303619.png)





AOP案例



注解

```java
package com.itheima.anno;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Log {

}
// 定义在要作用的类上
```



```java

@Aspect // 切面类
public class LogAspect {

    @Autowired
    private HttpServletRequest request;
    @Autowired
    private OperateLogMapper opesrateLogMapper;

//使用@annotation注解形式
    @Around("@annotation(com.itheima.anno.Log)")
    public  Object recordLog (ProceedingJoinPoint JoinPoint) throws Throwable {
        // 获取token内参数id
        String jwt = request.getHeader("token");
        Claims claims = JwtUtils.parseJWT(jwt);
      Integer userId =  (Integer) claims.get("id");

        // 获取操作时间
        LocalDateTime time = LocalDateTime.now();
        
// 获取类名
        String name = JoinPoint.getTarget().getClass().getName();
       // 获取方法名
        String methName = JoinPoint.getSignature().getName();
        // 获取请求参数转换为字符串
        Object[] args = JoinPoint.getArgs();
        String methodParams = Arrays.toString(args);
        //获取开始时间
        long begin = System.currentTimeMillis();
        // 调用原始方法
        Object result = JoinPoint.proceed();
        // 获取结束时间
        long end = System.currentTimeMillis();
     //   获取返回值
        String returnValue = JSONObject.toJSONString(result);
        // 方法结束时间
        Long costTime = end - begin;

        // 方法返回值
        OperateLog op = new OperateLog(null,userId,time,name,methName,methodParams,returnValue,costTime);
        operateLogMapper.insert(op);
        log.info("AOP记录操作日志：{}",op);


        return result;
    }

}

```







## Spring Boot高级

### springBoot配置文件

优先级

![image-20230802195705434](/image-20230802195705434.png)

![image-20230802200242967](/image-20230802200242967.png)

![image-20230802200302278](/image-20230802200302278.png)



### Bean

声明 IOC容器对象

```java
 @Autowired
    private ApplicationContext applicationContext;
```



通过ApplicationContext 获取Ben对象

![image-20230802201818613](/image-20230802201818613.png)

根据名称获取的其实是一个object对象，需要强制类型转换，获取bean名字时首字母小写

![image-20230802202440225](/image-20230802202440225.png)

![image-20230802203113463](/image-20230802203113463.png)



### 第三方bean

![image-20230802211141810](/image-20230802211141810.png)

![image-20230802211224578](/image-20230802211224578.png)





Spring Boot原理

自动装配原理

![image-20230802213019738](/image-20230802213019738.png)

![image-20230802214120387](/image-20230802214120387.png)

![image-20230802220617382](/image-20230802220617382.png)

![image-20230802220951831](/image-20230802220951831.png)



![image-20230802222225416](/image-20230802222225416.png)

![image-20230802223053069](/image-20230802223053069.png)

![image-20230803213358929](/image-20230803213358929.png)





## Maven高级



分模块

![image-20230803220321308](/image-20230803220321308.png)

![image-20230803220346000](/image-20230803220346000.png)



### 继承与聚合

![image-20230803220634325](/image-20230803220634325.png)

![image-20230803220755500](/image-20230803220755500.png)





![image-20230803221100117](/image-20230803221100117.png)

**父工程准备，继承**spring boot

![image-20230803221359053](/image-20230803221359053.png)

配置继承父工程

![image-20230803221600740](/image-20230803221600740.png)

![image-20230803221847603](/image-20230803221847603.png)

![image-20230803221937617](/image-20230803221937617.png)



版本锁定

![image-20230803222309182](/image-20230803222309182.png)

![image-20230803224338882](/image-20230803224338882.png)

![image-20230803225423835](/image-20230803225423835.png)



### 聚合

![image-20230803225739305](/image-20230803225739305.png)

![image-20230803225955610](/image-20230803225955610.png)

![image-20230803230439503](/image-20230803230439503.png)

![image-20230803230533016](/image-20230803230533016.png)

![image-20230803231240184](/image-20230803231240184.png)

![image-20230803232016458](/image-20230803232016458.png)



资源上传和下载

![image-20230803232518997](/image-20230803232518997.png)

![image-20230803232553888](/image-20230803232553888.png)

![image-20230803232641740](/image-20230803232641740.png)

![image-20230803232930518](/image-20230803232930518.png)

![image-20230803233027400](/image-20230803233027400.png)







## HttpClient服务端http请求使用

首先导入pom HttpClient坐标



### get使用方法

```java
    /**
     * get请求
     * @throws IOException
     */
    @Test
    public void testGET() throws IOException {
    // 创建httpClient对象

    CloseableHttpClient httpClient = HttpClients.createDefault();

     // 创建请求对象
    HttpGet httpGet = new HttpGet("http://localhost:8080/user/shop/status");

    // 发送请求
    CloseableHttpResponse response = httpClient.execute(httpGet);

// 获取服务端返回状态码
    Integer statusCode =   response.getStatusLine().getStatusCode();
    System.out.println("状态码"+statusCode);
    HttpEntity entity = response.getEntity();
    String body = EntityUtils.toString(entity);
    System.out.println(body);

    // 关闭资源
    response.close();
    httpClient.close();


}

```





### post使用方法

```java
@Test
    public void  testPost() throws IOException {
    // 创建httpClient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    // 创建请求对象
    JSONObject jsonObject = new JSONObject();
    jsonObject.put("username", "admin");
    jsonObject.put("password", "123456");
    HttpPost httpPost = new HttpPost("http://localhost:8080/admin/employee/login");
    StringEntity entity = new StringEntity(jsonObject.toString());
    // 指定请求编码方式
    entity.setContentEncoding("utf-8");
    // 指定数据格式
    entity.setContentType("application/json");
    httpPost.setEntity(entity);
    // 发送请求
    CloseableHttpResponse response = httpClient.execute(httpPost);
    // 解析返回结果
    int statusCode = response.getStatusLine().getStatusCode();

    System.out.println("响应码"+statusCode);

    HttpEntity entity1 = response.getEntity();
    System.out.println(EntityUtils.toString(entity1));

    // 关闭资源
    httpClient.close();
    response.close();
}


```







### 封装





```java
public class HttpClientUtil {

    static final  int TIMEOUT_MSEC = 5 * 1000;

    /**
     * 发送GET方式请求
     * @param url
     * @param paramMap
     * @return
     */
    public static String doGet(String url,Map<String,String> paramMap){
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();

        String result = "";
        CloseableHttpResponse response = null;

        try{
            URIBuilder builder = new URIBuilder(url);
            if(paramMap != null){
                for (String key : paramMap.keySet()) {
                    builder.addParameter(key,paramMap.get(key));
                }
            }
            URI uri = builder.build();

            //创建GET请求
            HttpGet httpGet = new HttpGet(uri);

            //发送请求
            response = httpClient.execute(httpGet);

            //判断响应状态
            if(response.getStatusLine().getStatusCode() == 200){
                result = EntityUtils.toString(response.getEntity(),"UTF-8");
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            try {
                response.close();
                httpClient.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return result;
    }

    /**
     * 发送POST方式请求
     * @param url
     * @param paramMap
     * @return
     * @throws IOException
     */
    public static String doPost(String url, Map<String, String> paramMap) throws IOException {
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        CloseableHttpResponse response = null;
        String resultString = "";

        try {
            // 创建Http Post请求
            HttpPost httpPost = new HttpPost(url);

            // 创建参数列表
            if (paramMap != null) {
                List<NameValuePair> paramList = new ArrayList();
                for (Map.Entry<String, String> param : paramMap.entrySet()) {
                    paramList.add(new BasicNameValuePair(param.getKey(), param.getValue()));
                }
                // 模拟表单
                UrlEncodedFormEntity entity = new UrlEncodedFormEntity(paramList);
                httpPost.setEntity(entity);
            }

            httpPost.setConfig(builderRequestConfig());

            // 执行http请求
            response = httpClient.execute(httpPost);

            resultString = EntityUtils.toString(response.getEntity(), "UTF-8");
        } catch (Exception e) {
            throw e;
        } finally {
            try {
                response.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return resultString;
    }

    /**
     * 发送POST方式请求
     * @param url
     * @param paramMap
     * @return
     * @throws IOException
     */
    public static String doPost4Json(String url, Map<String, String> paramMap) throws IOException {
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        CloseableHttpResponse response = null;
        String resultString = "";

        try {
            // 创建Http Post请求
            HttpPost httpPost = new HttpPost(url);

            if (paramMap != null) {
                //构造json格式数据
                JSONObject jsonObject = new JSONObject();
                for (Map.Entry<String, String> param : paramMap.entrySet()) {
                    jsonObject.put(param.getKey(),param.getValue());
                }
                StringEntity entity = new StringEntity(jsonObject.toString(),"utf-8");
                //设置请求编码
                entity.setContentEncoding("utf-8");
                //设置数据类型
                entity.setContentType("application/json");
                httpPost.setEntity(entity);
            }

            httpPost.setConfig(builderRequestConfig());

            // 执行http请求
            response = httpClient.execute(httpPost);

            resultString = EntityUtils.toString(response.getEntity(), "UTF-8");
        } catch (Exception e) {
            throw e;
        } finally {
            try {
                response.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return resultString;
    }
    private static RequestConfig builderRequestConfig() {
        return RequestConfig.custom()
                .setConnectTimeout(TIMEOUT_MSEC)
                .setConnectionRequestTimeout(TIMEOUT_MSEC)
                .setSocketTimeout(TIMEOUT_MSEC).build();
    }

}

```

