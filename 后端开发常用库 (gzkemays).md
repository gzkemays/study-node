# 后端开发常用库及基本配置 (gzkemays)

## 一、POM 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <!-- 版本号控制 -->
        <version>2.3.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <!-- 打包模式 -->
    <packaging>war</packaging>
    <!-- 项目组织 -->
    <groupId>com.gzkemays</groupId>
    <!-- 工具名 -->
    <artifactId></artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <!-- 项目名 -->
    <name></name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <!-- JAVA 版本 -->
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!-- JPA -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <!-- Thymeleaf -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <!-- SpringBoot - WEB -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
		<!-- 热部署 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <!-- 数据库连接源 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <!-- lombok 表达式 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <!-- SpringBoot - TEST  -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- alibaba 数据源 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <!-- MybatisPlus -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.3.2</version>
        </dependency>
        <!-- 代码生成器模板 -->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.2</version>
        </dependency>
        <!-- MP 代码生成器 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.3.2</version>
        </dependency>
        <!-- HttpClient -->
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.2</version>
        </dependency>
		<!-- alibaba 自带 JSON 转换器 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.4</version>
        </dependency>
		<!-- 双向通讯 WebSocket -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
        </dependency>
        <!-- 服务器程序 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <version>2.1.5.RELEASE</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

## 二、applicationContext 配置

```yaml
spring:
# 数据库配置
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/supermarket?serverTimezone=UTC&useSSL=false&useUnicode=true&characterEncoding=UTF-8
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
    druid:
      # 最小闲置数
      min-idle: 10
      # 最大活动数
      max-active: 100
      # 初始化内存
      initial-size: 20
  mvc:
    hiddenmethod:
      filter:
        enabled: true
# JPA 的数据库类生成配置
#  jpa:
#    hibernate:
#      ddl-auto: update

# Mybatis 配置（需要将 XML 文件移植至 resouces 目录下）
mybatis-plus:
  type-aliases-package: com.myself.gzkemays.pojo
  mapper-locations: classpath:xml/*.xml
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

server:
  # 端口配置
  port: 9999
  # servlet:
  #  encoding:
  #    force: true
  #    charset: UTF-8

```



