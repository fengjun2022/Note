# SSM

## spring



### IOC容器

![image-20230807202736549](C:\Users\32784\Desktop\笔记\image\image-20230807202736549.png)



设置Bean



![image-20230807203219085](C:\Users\32784\Desktop\笔记\image\image-20230807203219085.png)

![image-20230807203339921](C:\Users\32784\Desktop\笔记\image\image-20230807203339921.png)

![image-20230807203429479](C:\Users\32784\Desktop\笔记\image\image-20230807203429479.png)



不以new的方式创建，业务层则直接引入

![image-20230807204420653](C:\Users\32784\Desktop\笔记\image\image-20230807204420653.png)

![image-20230807205026068](C:\Users\32784\Desktop\笔记\image\image-20230807205026068.png)

![image-20230807205043754](C:\Users\32784\Desktop\笔记\image\image-20230807205043754.png)



为什么bean是单例

![image-20230807205130654](C:\Users\32784\Desktop\笔记\image\image-20230807205130654.png)



实例化bean



![image-20230807210712057](C:\Users\32784\Desktop\笔记\image\image-20230807210712057.png)





实例工厂bean

![image-20230807211403429](C:\Users\32784\Desktop\笔记\image\image-20230807211403429.png)



4.使用FactoryBean实例化

![image-20230807211729116](C:\Users\32784\Desktop\笔记\image\image-20230807211729116.png)

![image-20230807211907454](C:\Users\32784\Desktop\笔记\image\image-20230807211907454.png)

![image-20230807212031347](C:\Users\32784\Desktop\笔记\image\image-20230807212031347.png)

![image-20230807212115266](C:\Users\32784\Desktop\笔记\image\image-20230807212115266.png)



bean的生命周期

![image-20230807212627111](C:\Users\32784\Desktop\笔记\image\image-20230807212627111.png)

![image-20230808200222618](C:\Users\32784\Desktop\笔记\image\image-20230808200222618.png)

close方法比较暴力如何时候会直接关闭容器

![image-20230808200804105](C:\Users\32784\Desktop\笔记\image\image-20230808200804105.png)

另一个关闭钩子在任何时间都可以，钩子关闭等到虚拟机关闭时再关闭



**使用借口实现**

![image-20230808201509053](C:\Users\32784\Desktop\笔记\image\image-20230808201509053.png)

![image-20230808201605219](C:\Users\32784\Desktop\笔记\image\image-20230808201605219.png)

![image-20230808201623930](C:\Users\32784\Desktop\笔记\image\image-20230808201623930.png)

![image-20230808201704845](C:\Users\32784\Desktop\笔记\image\image-20230808201704845.png)



### 依赖注入

![image-20230808202111162](C:\Users\32784\Desktop\笔记\image\image-20230808202111162.png)

![image-20230808202621442](C:\Users\32784\Desktop\笔记\image\image-20230808202621442.png)

![image-20230808203421235](C:\Users\32784\Desktop\笔记\image\image-20230808203421235.png)

![image-20230808203446857](C:\Users\32784\Desktop\笔记\image\image-20230808203446857.png)





### bean自动注入





![image-20230808204522928](C:\Users\32784\Desktop\笔记\image\image-20230808204522928.png)

![image-20230808204534492](C:\Users\32784\Desktop\笔记\image\image-20230808204534492.png)



### 集合注入的方式

![image-20230808205014983](C:\Users\32784\Desktop\笔记\image\image-20230808205014983.png)





![image-20230808205341526](C:\Users\32784\Desktop\笔记\image\image-20230808205341526.png)

![image-20230808205353780](C:\Users\32784\Desktop\笔记\image\image-20230808205353780.png)

![image-20230808205406834](C:\Users\32784\Desktop\笔记\image\image-20230808205406834.png)

![image-20230808205413490](C:\Users\32784\Desktop\笔记\image\image-20230808205413490.png)

![image-20230808205422405](C:\Users\32784\Desktop\笔记\image\image-20230808205422405.png)



### 管理第三方bean

![image-20230808210226828](C:\Users\32784\Desktop\笔记\image\image-20230808210226828.png)

![image-20230808210417549](C:\Users\32784\Desktop\笔记\image\image-20230808210417549.png)





### 加载property文件





![image-20230808213251863](C:\Users\32784\Desktop\笔记\image\image-20230808213251863.png)



![image-20230808213435105](C:\Users\32784\Desktop\笔记\image\image-20230808213435105.png)



加载多个文件（当前工程）

![image-20230808213709817](C:\Users\32784\Desktop\笔记\image\image-20230808213709817.png)



加载多个文件（包括其他工程）

![image-20230808214133908](C:\Users\32784\Desktop\笔记\image\image-20230808214133908.png)



![image-20230808214154974](C:\Users\32784\Desktop\笔记\image\image-20230808214154974.png)





### 容器

![image-20230808214701555](C:\Users\32784\Desktop\笔记\image\image-20230808214701555.png)



![image-20230808215728415](C:\Users\32784\Desktop\笔记\image\image-20230808215728415.png)

容器相关 

![image-20230808215857514](C:\Users\32784\Desktop\笔记\image\image-20230808215857514.png)

bean相关

![image-20230808215914866](C:\Users\32784\Desktop\笔记\image\image-20230808215914866.png)





依赖注入

![image-20230808220029311](C:\Users\32784\Desktop\笔记\image\image-20230808220029311.png)





### 注解开发定义bean

![image-20230808222123339](C:\Users\32784\Desktop\笔记\image\image-20230808222123339.png)

![image-20230808222150994](C:\Users\32784\Desktop\笔记\image\image-20230808222150994.png)

![image-20230808225112736](C:\Users\32784\Desktop\笔记\image\image-20230808225112736.png)![image-20230808225550670](C:\Users\32784\Desktop\笔记\image\image-20230808225550670.png![image-20230808225608946](C:\Users\32784\Desktop\笔记\image\image-20230808225608946.png)



### 注解依赖注入

![image-20230808230056883](C:\Users\32784\Desktop\笔记\image\image-20230808230056883.png)

![image-20230808230102250](C:\Users\32784\Desktop\笔记\image\image-20230808230102250.png)

![image-20230808231602386](C:\Users\32784\Desktop\笔记\image\image-20230808231602386.png)





### 第三方bean管理

手写，然后交给bean管理

![image-20230808232511780](C:\Users\32784\Desktop\笔记\image\image-20230808232511780.png)

![image-20230808232659330](C:\Users\32784\Desktop\笔记\image\image-20230808232659330.png)

![image-20230808232722135](C:\Users\32784\Desktop\笔记\image\image-20230808232722135.png)

![image-20230808232733766](C:\Users\32784\Desktop\笔记\image\image-20230808232733766.png)



![image-20230808233123681](C:\Users\32784\Desktop\笔记\image\image-20230808233123681.png)

![image-20230808233149565](C:\Users\32784\Desktop\笔记\image\image-20230808233149565.png)









### mybits整合spring

![image-20230809204128260](C:\Users\32784\Desktop\笔记\image\image-20230809204128260.png)

![image-20230809204210148](C:\Users\32784\Desktop\笔记\image\image-20230809204210148.png)





### spring整合Junit

![image-20230809204651507](C:\Users\32784\Desktop\笔记\image\image-20230809204651507.png)







### AOP

![image-20230809211523290](C:\Users\32784\Desktop\笔记\image\image-20230809211523290.png)





![image-20230809220246497](C:\Users\32784\Desktop\笔记\image\image-20230809220246497.png)

![image-20230809220539033](C:\Users\32784\Desktop\笔记\image\image-20230809220539033.png)

![image-20230809220734353](C:\Users\32784\Desktop\笔记\image\image-20230809220734353.png)

![image-20230809222136657](C:\Users\32784\Desktop\笔记\image\image-20230809222136657.png)

![image-20230809222142277](C:\Users\32784\Desktop\笔记\image\image-20230809222142277.png)

![image-20230809222235216](C:\Users\32784\Desktop\笔记\image\image-20230809222235216.png)

![image-20230810013521701](C:\Users\32784\Desktop\笔记\image\image-20230810013521701.png)

![image-20230810013612793](C:\Users\32784\Desktop\笔记\image\image-20230810013612793.png)



![image-20230810013759707](C:\Users\32784\Desktop\笔记\image\image-20230810013759707.png)







### 事务

![image-20230810014400969](C:\Users\32784\Desktop\笔记\image\image-20230810014400969.png)



![image-20230810014447940](C:\Users\32784\Desktop\笔记\image\image-20230810014447940.png)

![image-20230810014505371](C:\Users\32784\Desktop\笔记\image\image-20230810014505371.png)

![image-20230810014717672](C:\Users\32784\Desktop\笔记\image\image-20230810014717672.png)

![image-20230810014813671](C:\Users\32784\Desktop\笔记\image\image-20230810014813671.png)

![image-20230810015140783](C:\Users\32784\Desktop\笔记\image\image-20230810015140783.png)

![image-20230810015653763](C:\Users\32784\Desktop\笔记\image\image-20230810015653763.png)

![image-20230810015730737](C:\Users\32784\Desktop\笔记\image\image-20230810015730737.png)

















##                           Spring MVC









### 入门导入

![image-20230810211834696](C:\Users\32784\Desktop\笔记\image\image-20230810211834696.png)





![image-20230810211857371](C:\Users\32784\Desktop\笔记\image\image-20230810211857371.png)





![image-20230810211917138](C:\Users\32784\Desktop\笔记\image\image-20230810211917138.png)







![image-20230810211937547](C:\Users\32784\Desktop\笔记\image\image-20230810211937547.png)





![image-20230810220050098](C:\Users\32784\Desktop\笔记\image\image-20230810220050098.png)



### bean加载控制

![image-20230810221028660](C:\Users\32784\Desktop\笔记\image\image-20230810221028660.png)

![image-20230810221121386](C:\Users\32784\Desktop\笔记\image\image-20230810221121386.png)



简化config配置类



![image-20230810221350001](C:\Users\32784\Desktop\笔记\image\image-20230810221350001.png)

![image-20230810221432490](C:\Users\32784\Desktop\笔记\image\image-20230810221432490.png)



### 请求与响应

![image-20230810222319576](C:\Users\32784\Desktop\笔记\image\image-20230810222319576.png)



解析中文乱码

![image-20230810222924838](C:\Users\32784\Desktop\笔记\image\image-20230810222924838.png)







### JSON支持

![image-20230810224138197](C:\Users\32784\Desktop\笔记\image\image-20230810224138197.png)

![image-20230810224146924](C:\Users\32784\Desktop\笔记\image\image-20230810224146924.png)

![image-20230810224200555](C:\Users\32784\Desktop\笔记\image\image-20230810224200555.png)

![image-20230810224223213](C:\Users\32784\Desktop\笔记\image\image-20230810224223213.png)

![image-20230810224245704](C:\Users\32784\Desktop\笔记\image\image-20230810224245704.png)

![image-20230810224256239](C:\Users\32784\Desktop\笔记\image\image-20230810224256239.png)



@RequestBody与@RequestParam的区别

![image-20230810224345153](C:\Users\32784\Desktop\笔记\image\image-20230810224345153.png)





### 日期类型传递

![image-20230810224810037](C:\Users\32784\Desktop\笔记\image\image-20230810224810037.png)

![image-20230810224824047](C:\Users\32784\Desktop\笔记\image\image-20230810224824047.png)







### 响应

![image-20230810225332152](C:\Users\32784\Desktop\笔记\image\image-20230810225332152.png)





### REST风格





![image-20230811203926945](C:\Users\32784\Desktop\笔记\image\image-20230811203926945.png)

![image-20230811205001896](C:\Users\32784\Desktop\笔记\image\image-20230811205001896.png)

![image-20230811205013732](C:\Users\32784\Desktop\笔记\image\image-20230811205013732.png)

![image-20230811205021951](C:\Users\32784\Desktop\笔记\image\image-20230811205021951.png)









### SSM整合配置

springconfig配置

![image-20230811225255068](C:\Users\32784\Desktop\笔记\image\image-20230811225255068.png)

JDBC配置

![image-20230811225331439](C:\Users\32784\Desktop\笔记\image\image-20230811225331439.png)



jdbc.properties配置

![image-20230811225423652](C:\Users\32784\Desktop\笔记\image\image-20230811225423652.png)



MyBatisConfig配置类

![image-20230811225638486](C:\Users\32784\Desktop\笔记\image\image-20230811225638486.png)



配置事务

![image-20230811225750464](C:\Users\32784\Desktop\笔记\image\image-20230811225750464.png)



SpringMvc配置类

![image-20230811225931335](C:\Users\32784\Desktop\笔记\image\image-20230811225931335.png)



### 异常处理器

![image-20230811231954819](C:\Users\32784\Desktop\笔记\image\image-20230811231954819.png)

![image-20230811232436280](C:\Users\32784\Desktop\笔记\image\image-20230811232436280.png)

![image-20230811232500667](C:\Users\32784\Desktop\笔记\image\image-20230811232500667.png)





### 项目异常处理方案

![image-20230812122546382](C:\Users\32784\Desktop\笔记\image\image-20230812122546382.png)



自定义项目系统级异常

![image-20230812124847754](C:\Users\32784\Desktop\笔记\image\image-20230812124847754.png)



自定义业务异常

![image-20230812124930357](C:\Users\32784\Desktop\笔记\image\image-20230812124930357.png)

、![image-20230812124947168](C:\Users\32784\Desktop\笔记\image\image-20230812124947168.png)



### 拦截器配置

![image-20230812154847640](C:\Users\32784\Desktop\笔记\image\image-20230812154847640.png)

![image-20230812154917035](C:\Users\32784\Desktop\笔记\image\image-20230812154917035.png)

![image-20230812154938912](C:\Users\32784\Desktop\笔记\image\image-20230812154938912.png)

![image-20230812155420651](C:\Users\32784\Desktop\笔记\image\image-20230812155420651.png)



拦截器参数

![image-20230812160018120](C:\Users\32784\Desktop\笔记\image\image-20230812160018120.png)

![image-20230812160029159](C:\Users\32784\Desktop\笔记\image\image-20230812160029159.png)





拦截器执行顺序

![image-20230812160451033](C:\Users\32784\Desktop\笔记\image\image-20230812160451033.png)

![image-20230812160652484](C:\Users\32784\Desktop\笔记\image\image-20230812160652484.png)





## Maven



### 可选依赖

![image-20230812163713239](C:\Users\32784\Desktop\笔记\image\image-20230812163713239.png)

排除依赖

![image-20230812163734410](C:\Users\32784\Desktop\笔记\image\image-20230812163734410.png)





### 聚合

![image-20230812164032759](C:\Users\32784\Desktop\笔记\image\image-20230812164032759.png)

![image-20230812164302929](C:\Users\32784\Desktop\笔记\image\image-20230812164302929.png) 



### 继承

![image-20230812164924347](C:\Users\32784\Desktop\笔记\image\image-20230812164924347.png)



配置可选依赖

![image-20230812164954181](C:\Users\32784\Desktop\笔记\image\image-20230812164954181.png)



配置继承关系

![image-20230812165028129](C:\Users\32784\Desktop\笔记\image\image-20230812165028129.png)



### 配置属性

配置bulid

![image-20230812181031039](C:\Users\32784\Desktop\笔记\image\image-20230812181031039.png) 

![image-20230812165601503](C:\Users\32784\Desktop\笔记\image\image-20230812165601503.png)

![image-20230812165629734](C:\Users\32784\Desktop\笔记\image\image-20230812165629734.png)



### 资源文件引用pom中的属性







![image-20230812174603526](C:\Users\32784\Desktop\笔记\image\image-20230812174603526.png)



### 版本管理

![image-20230812174846514](C:\Users\32784\Desktop\笔记\image\image-20230812174846514.png)



![image-20230812180504463](C:\Users\32784\Desktop\笔记\image\image-20230812180504463.png)

![image-20230812180730784](C:\Users\32784\Desktop\笔记\image\image-20230812180730784.png)





### 跳过测试

![image-20230812181958888](C:\Users\32784\Desktop\笔记\image\image-20230812181958888.png)



细粒度控制跳过测试

![image-20230812182025945](C:\Users\32784\Desktop\笔记\image\image-20230812182025945.png)





### 私服

![image-20230812191736109](C:\Users\32784\Desktop\笔记\image\image-20230812191736109.png)



![image-20230812191548088](C:\Users\32784\Desktop\笔记\image\image-20230812191548088.png)



### 配置本地仓库访问私服

![image-20230812191856058](C:\Users\32784\Desktop\笔记\image\image-20230812191856058.png)



![image-20230812192055120](C:\Users\32784\Desktop\笔记\image\image-20230812192055120.png)

![image-20230812192211446](C:\Users\32784\Desktop\笔记\image\image-20230812192211446.png)

![image-20230812192424977](C:\Users\32784\Desktop\笔记\image\image-20230812192424977.png)





![image-20230812192714893](C:\Users\32784\Desktop\笔记\image\image-20230812192714893.png)

![image-20230812192723995](C:\Users\32784\Desktop\笔记\image\image-20230812192723995.png)

![image-20230812192737408](C:\Users\32784\Desktop\笔记\image\image-20230812192737408.png)

![image-20230812192756757](C:\Users\32784\Desktop\笔记\image\image-20230812192756757.png)







## Sprig Boot

### 起步依赖



![image-20230812212440945](C:\Users\32784\Desktop\笔记\image\image-20230812212440945.png)



![image-20230812212530700](C:\Users\32784\Desktop\笔记\image\image-20230812212530700.png)







读取yml配置文件 



1.@value

![image-20230812215925052](C:\Users\32784\Desktop\笔记\image\image-20230812215925052.png)

![image-20230812215934674](C:\Users\32784\Desktop\笔记\image\image-20230812215934674.png)





**封装到Environment对象**

![image-20230812221022848](C:\Users\32784\Desktop\笔记\image\image-20230812221022848.png)





**自定义对象封装指定数据**

![image-20230812221201344](C:\Users\32784\Desktop\笔记\image\image-20230812221201344.png)





![image-20230812221223192](C:\Users\32784\Desktop\笔记\image\image-20230812221223192.png)





### 多环境配置

![image-20230812221612740](C:\Users\32784\Desktop\笔记\image\image-20230812221612740.png)

![image-20230812221814015](C:\Users\32784\Desktop\笔记\image\image-20230812221814015.png)



![image-20230812221831119](C:\Users\32784\Desktop\笔记\image\image-20230812221831119.png)







### 多环境启动命令格式

![image-20230812222154770](C:\Users\32784\Desktop\笔记\image\image-20230812222154770.png)





### maven与springBoot多环境兼容

![image-20230812222804746](C:\Users\32784\Desktop\笔记\image\image-20230812222804746.png)



![image-20230812222905893](C:\Users\32784\Desktop\笔记\image\image-20230812222905893.png)



添加占位符对操作进行解析



![image-20230812222939142](C:\Users\32784\Desktop\笔记\image\image-20230812222939142.png)



### 配置文件分类

![image-20230812223644888](C:\Users\32784\Desktop\笔记\image\image-20230812223644888.png)







### 测试

![image-20230812224358525](C:\Users\32784\Desktop\笔记\image\image-20230812224358525.png)

![image-20230812224404556](C:\Users\32784\Desktop\笔记\image\image-20230812224404556.png)





### 整合mybits

![image-20230813105419166](C:\Users\32784\Desktop\笔记\image\image-20230813105419166.png)







## MybatisPlus





SpringBoot整合MybatisPlus入门程序

![image-20230813113702165](C:\Users\32784\Desktop\笔记\image\image-20230813113702165.png)



![image-20230813113714233](C:\Users\32784\Desktop\笔记\image\image-20230813113714233.png)



![image-20230813113844475](C:\Users\32784\Desktop\笔记\image\image-20230813113844475.png)





![image-20230813113901184](C:\Users\32784\Desktop\笔记\image\image-20230813113901184.png)

![image-20230813115914828](C:\Users\32784\Desktop\笔记\image\image-20230813115914828.png)





### 分页查询

![image-20230813130752569](C:\Users\32784\Desktop\笔记\image\image-20230813130752569.png)



配置

```java
package com.itheima.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor pageInterCeptor(){
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }
}


```



![image-20230813132659491](C:\Users\32784\Desktop\笔记\image\image-20230813132659491.png)





###  

![image-20230813142533336](C:\Users\32784\Desktop\笔记\image\image-20230813142533336.png)





### null值判定



![image-20230813143631995](C:\Users\32784\Desktop\笔记\image\image-20230813143631995.png)





### 查询投影

![image-20230813144059809](C:\Users\32784\Desktop\笔记\image\image-20230813144059809.png)



![image-20230813144621048](C:\Users\32784\Desktop\笔记\image\image-20230813144621048.png)