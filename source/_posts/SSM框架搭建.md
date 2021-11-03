---
title: SSM框架搭建
date: 2021-04-26 08:45:05
tags: 
    - SSM框架
    - 后台
---

# SSM框架搭建（基于IDEA）(未完成，部分内容未验证)

> 本文编写与2021年4月，内容可能具有时效性，如相关软件版本更迭，相关服务器停止维护等。
> 
> 相关软件版本：
> 1. IntelliJ IDEA 2020.2.1 (Ultimate Edition)
Build #IU-202.6948.69, built on August 25, 2020

## 一、IDEA新建项目

1. IDEA点击新建项目
2. 选中Maven项目
3. 右侧勾选从原型创建(create from archetype)
4. 底下列表选中`org.apache.maven.archetype:maven-archetype-webapp`后下一步
   
   <img src="p1.jpg">

5. 填写项目名称，目录，点击`构建坐标`还可以指定项目使用的包的前缀域名。
   
   <img src="p2.jpg" width="80%">
   
6. 下一步指定Maven相关，一般配好环境就不需要改动。
7. 点击`完成`，等待项目加载。
> 如果是第一次使用SSM，时间可能很长。尤其在国内，如果maven没有换源，很容易构建失败，如果出现对应情况，百度`maven换源`，自行操作，然后重启IDEA，Maven构建会自动重新开始

## 二、完善项目结构

如图添加目录，构建标准SSM框架模块目录。

<img src="p3.jpg" width="70%">

在我目前使用的版本，只需要手动添加`java`和`resources`两个文件夹，其余的会自动创建。完成后的目录应当如图所示：

<img src="p4.jpg" width="50%">

## 三、配置运行设置

1. 点击右上角的添加配置
2. 点弹出窗口左上角的加号
3. 找到`Tomcat Server`，展开选中`本地`，如图
   
   <img src="p5.jpg">

4. 此时，左侧会显示一个小红叉，右下角显示`警告：没有为部署标记工作`，点击警告后面的修复，选择`<项目名称>:war exploded`，IDEA会自动配置。
5. 点击应用、确定
6. 点击右上角的运行图标或者调试图标。一般会自动启动浏览器打开项目首页。

<img src="p6.jpg">

## 四、SSM框架配置

按照下图进一步完善项目目录结构，文件可以不急着新建。如果仅使用SSM作为后台API工具，整个`webapp`目录可以不用动。

<img src="p7.jpg" width="40%">

接下来编辑相关配置文件

* `/pom.xml`
  在`dependencies`标签中，`junit`标签后添加框架相关依赖，这能够让Maven随后自动下载对应的jar包。
```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>cn.milkyship</groupId>
  <artifactId>ggt-backend</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>ggt-backend Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <!--spring相关-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.9.RELEASE</version>
    </dependency>
    <!--Spriing jdbc-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.7.4</version>
    </dependency>

    <!-- 数据库相关 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.26</version>
    </dependency>
    <!--数据库连接池druid-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.0.11</version>
    </dependency>
    <!-- mybatis依赖 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.2</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.1</version>
    </dependency>
    <!-- 事务的配置标签 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>

    <!-- json -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.5.0</version>
    </dependency>
    <!-- fastjson -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.47</version>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!-- log日志  -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.14</version>
    </dependency>
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
      <version>1.2</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.6.1</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.6.1</version>
    </dependency>

    <!--文件上传下载 commons-->
    <!--<dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.2.1</version>
    </dependency>
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.2</version>
    </dependency>-->

  </dependencies>

  <build>
    <finalName>ggt-backend</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```

* `/src/main/webapp/WEB-INF/web.xml`
  这里定义各个模块配置文件的导入和一些服务器基本设置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

  <!-- 编码过滤器 -->
  <filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <async-supported>true</async-supported>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!-- Spring监听器 监听加载相关配置文件-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <!--防止Spring内存溢出监听器-->
  <listener>
    <listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>
  </listener>

  <!--spring的其他配置文件(包括mybatis配置文件) -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>
      classpath*:spring-applicationContext.xml
    </param-value>
  </context-param>

  <!--log4j日志-->
  <context-param>
    <param-name>log4jConfigLocation</param-name>
    <param-value>
      classpath:log4j.xml
    </param-value>
  </context-param>

  <!--Spring MVC servlet-->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath*:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <!-- 访问根路径时的默认访问页面，从上到下匹配 -->
  <welcome-file-list>
    <welcome-file>/index.jsp</welcome-file>
    <welcome-file>/index.html</welcome-file>
  </welcome-file-list>
</web-app>
```

* `/src/main/resources/spring-mvc.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:task="http://www.springframework.org/schema/task"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/task
       http://www.springframework.org/schema/task/spring-task-3.0.xsd">

    <!--开启springMVC注解模式-->
    <mvc:annotation-driven />

    <aop:aspectj-autoproxy proxy-target-class="true"/>

    <!-- 自动扫描，完成bean创建和自动依赖注入-->
    <context:component-scan base-package="com.test" />

    <!--避免IE执行AJAX时，返回JSON出现下载文件 -->
    <bean id="mappingJacksonHttpMessageConverter"
          class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
        <property name="supportedMediaTypes">
            <list>
                <value>text/html;charset=UTF-8</value>
            </list>
        </property>
    </bean>

    <!-- 启动SpringMVC的注解功能，完成请求和注解POJO的映射 -->
    <bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
        <property name="messageConverters">
            <list>
                <bean class="org.springframework.http.converter.ByteArrayHttpMessageConverter"/>
                <ref bean="mappingJacksonHttpMessageConverter" />  <!-- JSON转换器 -->
            </list>
        </property>
    </bean>

    <!-- 对模型视图名称的解析,即在模型视图名称添加前后缀 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 查找视图页面的前缀和后缀 -->
        <property name="prefix" value="/" />
        <property name="suffix" value=".html" />
    </bean>

    <!--<!–对上传文件的支持，springMVC其实是用common-upload来实现 –>
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!–设置上传文件的最大值,这里是字节–>
        <property name="maxUploadSize" value="102400000"></property> <!– 100M –>
        <property name="defaultEncoding" value="utf-8"></property>
    </bean>-->

    <!-- 总错误处理-->
    <bean id="exceptionResolver" class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <!--<property name="defaultErrorView">
            <value>/base/error</value>
        </property>-->
        <property name="defaultStatusCode">
            <value>500</value>
        </property>
        <property name="warnLogCategory">
            <value>org.springframework.web.servlet.handler.SimpleMappingExceptionResolver</value>
        </property>
    </bean>

    <!--静态资源配置-->
    <mvc:default-servlet-handler/>

    <!--定时器配置-->
    <task:executor id="executor" pool-size="5" />
    <task:scheduler id="scheduler" pool-size="10" />
    <task:annotation-driven executor="executor" scheduler="scheduler" />

</beans>
```

* `/src/main/resources/spring-applicationContext.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">
    <!--mybatis配置-->
    <import resource="classpath*:spring-mybatis.xml"/>
</beans>
```

* `/src/main/resources/jdbc.properties`
```properties
#mysql
jdbc.driver=com.mysql.jdbc.Driver

jdbc.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf-8
jdbc.username=root
jdbc.password=root

jdbc.initialSize=3
jdbc.maxActive=1000
jdbc.minIdle=0
jdbc.maxWait=6000
jdbc.removeAbandoned=true
jdbc.removeAbandonedTimeout=1800
jdbc.timeBetweenEvictionRunsMillis=60000
jdbc.minEvictableIdleTimeMillis=25200000
```

* `/src/main/resources/spring-mybatis.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd">

    <!-- 自动扫描，Bean注入 -->
    <context:component-scan base-package="com.test"/>

    <!-- 读取配置文件信息 -->
    <context:property-placeholder ignore-unresolvable="true" location="classpath:jdbc.properties"/>

    <bean name="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 数据库基本配置 -->
        <property name="driverClassName" value="${jdbc.driver}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />

        <!-- 初始化连接数量 -->
        <property name="initialSize" value="${jdbc.initialSize}"/>
        <!-- 最大并发连接数量 -->
        <property name="maxActive" value="${jdbc.maxActive}"/>
        <!-- 最小空闲连接数 -->
        <property name="minIdle" value="${jdbc.minIdle}"/>
        <!-- 配置获取连接等待超时的时间 -->
        <property name="maxWait" value="${jdbc.maxWait}" />
        <!-- 超过时间限制是否回收 -->
        <property name="removeAbandoned" value="${jdbc.removeAbandoned}" />
        <!-- 超过时间限制多长 -->
        <property name="removeAbandonedTimeout" value="${jdbc.removeAbandonedTimeout}" />
        <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
        <property name="timeBetweenEvictionRunsMillis" value="${jdbc.timeBetweenEvictionRunsMillis}" />
        <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
        <property name="minEvictableIdleTimeMillis" value="${jdbc.minEvictableIdleTimeMillis}" />
        <!-- 用来检测连接是否有效的sql，要求是一个查询语句-->
        <property name="validationQuery" value="${jdbc.validationQuery}" />
        <!-- 申请连接的时候检测 -->
        <property name="testWhileIdle" value="${jdbc.testWhileIdle}" />
        <!-- 申请连接时执行validationQuery检测连接是否有效，配置为true会降低性能 -->
        <property name="testOnBorrow" value="${jdbc.testOnBorrow}" />
        <!-- 归还连接时执行validationQuery检测连接是否有效，配置为true会降低性能  -->
        <property name="testOnReturn" value="${jdbc.testOnReturn}" />
        <property name="logAbandoned" value="true" />
        <!-- 配置监控统计拦截的filters，wall用于防止sql注入，stat用于统计分析 -->
        <property name="filters" value="stat" />
    </bean>

    <!-- MyBatis SqlSessionFactoryBean 配置 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 自动扫描Mapping.xml文件 -->
        <property name="mapperLocations" value="classpath:com/test/mapper/xml/*.xml"/>
        <!-- 配置MyBaties全局配置文件:mybatis-config.xml -->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <!-- 扫描model包 xml中parameterType就可以使用类名，不用全路径 -->
        <property name="typeAliasesPackage" value="com.test.model"/>
    </bean>

    <!-- 加载 mapper.xml对应的接口 配置文件 -->
    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 给出需要扫描mapper接口包 -->
        <property name="basePackage" value="com.test.mapper"/>
        <!-- 注入sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
    </bean>

    <!-- (事务管理)transaction manager, use JtaTransactionManager for global tx -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>

</beans>
```

* `/src/main/resources/mybatis-config.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD SQL Map Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--
 |   plugins在配置文件中的位置必须符合要求，否则会报错，顺序如下:
 |   properties?, settings?,
 |   typeAliases?, typeHandlers?,
 |   objectFactory?,objectWrapperFactory?,
 |   plugins?,
 |   environments?, databaseIdProvider?, mappers?
 |-->
<configuration>
    <!--
     | 全局配置设置
     |
     | 可配置选项                   默认值,     描述
     |
     | aggressiveLazyLoading       true,     当设置为‘true’的时候，懒加载的对象可能被任何懒属性全部加载。否则，每个属性都按需加载。
     | multipleResultSetsEnabled   true,     允许和不允许单条语句返回多个数据集（取决于驱动需求）
     | useColumnLabel              true,     使用列标签代替列名称。不同的驱动器有不同的作法。参考一下驱动器文档，或者用这两个不同的选项进行测试一下。
     | useGeneratedKeys            false,    允许JDBC 生成主键。需要驱动器支持。如果设为了true，这个设置将强制使用被生成的主键，有一些驱动器不兼容不过仍然可以执行。
     | autoMappingBehavior         PARTIAL,  指定MyBatis 是否并且如何来自动映射数据表字段与对象的属性。PARTIAL将只自动映射简单的，没有嵌套的结果。FULL 将自动映射所有复杂的结果。
     | defaultExecutorType         SIMPLE,   配置和设定执行器，SIMPLE 执行器执行其它语句。REUSE 执行器可能重复使用prepared statements 语句，BATCH执行器可以重复执行语句和批量更新。
     | defaultStatementTimeout     null,     设置一个时限，以决定让驱动器等待数据库回应的多长时间为超时
     | -->
    <settings>
        <!-- 这个配置使全局的映射器启用或禁用缓存 -->
        <setting name="cacheEnabled" value="true"/>
        <!-- 全局启用或禁用延迟加载。当禁用时，所有关联对象都会即时加载 -->
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="multipleResultSetsEnabled" value="true"/>
        <setting name="useColumnLabel" value="true"/>
        <setting name="defaultExecutorType" value="REUSE"/>
        <setting name="defaultStatementTimeout" value="25000"/>
        <!-- 让控制台打印sql语句，注释掉则没有 -->
        <setting name="logImpl" value="STDOUT_LOGGING" />
        <setting name="callSettersOnNulls" value="true" />
    </settings>
</configuration>
```


* `/src/main/resources/log4j.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j='http://jakarta.apache.org/log4j/'>

    <!--ConsoleAppender：输出到控制台-->
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <!--<param name="encoding" value="UTF-8"/>-->
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%d [%t] %p - %m%n"/>
        </layout>
        <filter class="org.apache.log4j.varia.LevelRangeFilter">
            <!-- 设置日志输出的最小级别 -->
            <param name="levelMin" value="DEBUG" />
            <!-- 设置日志输出的最大级别 -->
            <param name="levelMax" value="FATAL" />
            <!-- 设置日志输出的xxx，默认是false -->
            <param name="AcceptOnMatch" value="true" />
        </filter>
    </appender>

    <!--输出到日志文件-->
    <appender name="PROJECT" class="org.apache.log4j.DailyRollingFileAppender">
        <!-- 设置日志信息输出文件全路径名 -->
        <param name="file" value="${catalina.home}/logs/ceshi/info.log"/>
        <!--日志文件编码-->
        <param name="encoding" value="UTF-8"/>
        <!--此日志文件级别-->
        <param name="threshold" value="info"/>
        <!-- 设置是否在重新启动服务时，在原有日志的基础添加新日志 -->
        <param name="Append" value="true" />
        <!-- 设置保存备份回滚日志的最大个数 -->
        <param name="MaxBackupIndex" value="10" />
        <!-- 设置当日志文件达到此阈值的时候自动回滚，单位可以是KB，MB，GB，默认单位是KB -->
        <param name="MaxFileSize" value="50MB" />
        <!-- 设置日志输出的样式 -->
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%d [%X{requestURIWithQueryString}] %-5p %c{2} - %m%n"/>
        </layout>
    </appender>

    <appender name="PROJECT-ERROR" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="file" value="${catalina.home}/logs/ceshi/error.log"/>
        <param name="append" value="true"/>
        <param name="encoding" value="UTF-8"/>
        <param name="threshold" value="error"/>
        <param name="MaxBackupIndex" value="10" />
        <param name="MaxFileSize" value="50MB" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%d [%X{requestURIWithQueryString}] %-5p %c{2} - %m%n"/>
        </layout>
    </appender>

    <!--开发测试使用debug级别-->
    <appender name="PROJECT-DEBUG" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="file" value="${catalina.home}/logs/ceshi/debug.log"/>
        <param name="append" value="false"/>
        <param name="encoding" value="UTF-8"/>
        <param name="threshold" value="debug"/>
        <param name="MaxBackupIndex" value="10" />
        <param name="MaxFileSize" value="50MB" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%d [%X{requestURIWithQueryString}] %-5p %c{2} - %m%n"/>
        </layout>
    </appender>

    <!--总的日志级别-->
    <root>
        <level value="DEBUG"/>
        <appender-ref ref="PROJECT"/>
        <appender-ref ref="PROJECT-DEBUG"/>
        <appender-ref ref="PROJECT-ERROR"/>
        <appender-ref ref="STDOUT"/>
    </root>
</log4j:configuration>
```

